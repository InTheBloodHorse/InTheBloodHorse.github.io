---
title: Spring-data-jpa 多条件动态查询
date: 2019-01-29 09:20:49
tags:
- spring-data-jpa
categories:
- Java
---
条件查询211，前端传入的参数并不确定，在sql或者jdbc里，我们可以动态拼接sql来解决这个问题。但是在jpa里我们定义的repository接口需要继承JpaSpecificationExecutor接口。

```Java
public interface MeetingRoomRepository extends JpaRepository<MeetingRoom, Integer>, JpaSpecificationExecutor<MeetingRoom> {

}
```

JapSpecificationExecutor接口实现如下（需要传入Specification对象）
```Java

public interface JpaSpecificationExecutor<T> {
    Optional<T> findOne(@Nullable Specification<T> var1);

    List<T> findAll(@Nullable Specification<T> var1);

    Page<T> findAll(@Nullable Specification<T> var1, Pageable var2);

    List<T> findAll(@Nullable Specification<T> var1, Sort var2);

    long count(@Nullable Specification<T> var1);
}

```

写自己的Specification
```Java
public class MeetingConditions {

    public static Specification getMeetingSpecitication(MeetingRoomQueryForm meetingRoomQueryForm) {
        Specification specification = new Specification() {
            @Override
            public Predicate toPredicate(Root root, CriteriaQuery criteriaQuery, CriteriaBuilder criteriaBuilder) {
                //and 条件
                List<Predicate> predicateListAnd = new ArrayList<>();
                //楼层
                Integer buildingId = meetingRoomQueryForm.getBuildingId();
                if (buildingId != null) {
                    predicateListAnd.add(criteriaBuilder.equal(root.get("buildingId"), buildingId));
                }
                //容量
                Integer minVolume = meetingRoomQueryForm.getMinVolume();
                if (minVolume != null) {
                    predicateListAnd.add(criteriaBuilder.greaterThanOrEqualTo(root.get("volume"), minVolume));
                }
                Integer maxVolume = meetingRoomQueryForm.getMaxVolume();
                if (maxVolume != null) {
                    predicateListAnd.add(criteriaBuilder.lessThanOrEqualTo(root.get("volume"), maxVolume));
                }
                //价格
                Integer minPrice = meetingRoomQueryForm.getMinPrice();
                if (minPrice != null) {
                    predicateListAnd.add(criteriaBuilder.greaterThanOrEqualTo(root.get("price"), minPrice));
                }
                Integer maxPrice = meetingRoomQueryForm.getMaxPrice();
                if (maxPrice != null) {
                    predicateListAnd.add(criteriaBuilder.lessThanOrEqualTo(root.get("price"), maxPrice));
                }
                //是否使用
                Integer isUsing = meetingRoomQueryForm.getIsUsing();
                if (isUsing != null) {
                    predicateListAnd.add(criteriaBuilder.equal(root.get("isUsing"), isUsing));
                }
                //标签
                String tagStr = meetingRoomQueryForm.getTags();
                if (tagStr != null) {
                    List<String> tags = Arrays.asList(tagStr.split(","));
                    for (String tag : tags) {
                        if (tag.length() != KeyUtil.preLen + KeyUtil.sufLen) continue;
                        predicateListAnd.add(criteriaBuilder.like(root.get("tags").as(String.class), "%" + tag + "%"));
                    }
                }
                Predicate queryAnd = criteriaBuilder.and(predicateListAnd.toArray(new Predicate[predicateListAnd.size()]));
                return criteriaQuery.where(queryAnd).getRestriction();
            }
        };
        return specification;
    }
}
```

Service层调用
```Java
    @Override
    public List<MeetingRoomDTO> findByManyConditions(MeetingRoomQueryForm meetingRoomQueryForm) {
        Specification specification = MeetingConditions.getMeetingSpecitication(meetingRoomQueryForm);
        List<MeetingRoom> meetingRoomList = meetingRoomRepository.findAll(specification);
        return MeetingRoom2MeetingRoomDTO.convert(meetingRoomList);
    }
```
