---
title: Spring-data-jpa 重写save方法
date: 2019-01-27 22:17:21
tags:
- spring-data-jpa
categories:
- Java
---
项目需要用户密码进行MD5加密，所以需要在每次保存用户实体类的时候先对password字段进行加密再保存到数据库，因此需要重写jpa的save方法。Ctrl+鼠标左键跳到CurdRepository接口
```Java
package org.springframework.data.repository;

import java.util.Optional;

@NoRepositoryBean
public interface CrudRepository<T, ID> extends Repository<T, ID> {
    <S extends T> S save(S var1);

    <S extends T> Iterable<S> saveAll(Iterable<S> var1);

    Optional<T> findById(ID var1);

    boolean existsById(ID var1);

    Iterable<T> findAll();

    Iterable<T> findAllById(Iterable<ID> var1);

    long count();

    void deleteById(ID var1);

    void delete(T var1);

    void deleteAll(Iterable<? extends T> var1);

    void deleteAll();
}
```
我们可以看到save方法，继续跳，可以看到SimpleJpaRepository类中有save方法。
```Java
public class SimpleJpaRepository<T, ID> implements JpaRepositoryImplementation<T, ID> {
    private static final String ID_MUST_NOT_BE_NULL = "The given id must not be null!";
    private final JpaEntityInformation<T, ?> entityInformation;
    private final EntityManager em;
    private final PersistenceProvider provider;
    @Nullable
    private CrudMethodMetadata metadata;

    @Transactional
    public <S extends T> S save(S entity) {
        if (this.entityInformation.isNew(entity)) {
            this.em.persist(entity);
            return entity;
        } else {
            return this.em.merge(entity);
        }
    }
}
```

## 创建自己的类 继承SimpleJpaRepository
重写即可
```Java
@SuppressWarnings("ALL")
public class MySimpleRepository<T, ID> extends SimpleJpaRepository<T, ID> {

    private final JpaEntityInformation<T, ?> entityInformation;
    private final EntityManager em;

    @Autowired
    public MySimpleRepository(JpaEntityInformation<T, ?> entityInformation, EntityManager entityManager) {
        super(entityInformation, entityManager);
        this.entityInformation = entityInformation;
        this.em = entityManager;
    }

    @Override
    public <S extends T> S save(S entity) {
        if (entity.getClass().equals(User.class)) {
            User user = (User) entity;
            //假如是明文密码，则加密
            if (user.getPassword().length() >= UserConstant.PASSWORD_MIN_SIZE && user.getPassword().length() <= UserConstant.PASSWORD_MAX_SIZE) {
                user.setPassword(DecryptMD5.MD5(user.getPassword()));
            }
        }
        return super.save(entity);
    }
}
```
## 修改默认继承的jpa类
在app入门出加上注解，更改基类JapRepository
```Java
@EnableJpaRepositories(value = "cn.lsu.chicken.room", repositoryBaseClass = MySimpleRepository.class)
@SpringBootApplication
public class RoomApplication {

    public static void main(String[] args) {
        SpringApplication.run(RoomApplication.class, args);
    }

}
```

