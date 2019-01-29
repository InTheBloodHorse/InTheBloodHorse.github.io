---
title: Spring-data-jpa 多条件动态查询
date: 2019-01-29 09:20:49
tags:
- spring-data-jpa
categories:
- Java
---
条件查询，前端传入的参数并不确定，在sql或者jdbc里，我们可以动态拼接sql来解决这个问题。但是在jpa里我们定义的repository接口需要继承JpaSpecificationExecutor接口。

```Java
public interface MeetingRoomRepository extends JpaRepository<MeetingRoom, Integer>, JpaSpecificationExecutor<MeetingRoom> {

}
```

## 常规方法
```Java
public List<MeetingRoom> findByManyConditions(MeetingRoomQueryForm meetingRoomQueryForm) {
        List<MeetingRoom> meetingRoomList = new ArrayList<>();
        Specification specification = new Specification() {
            @Override
            public Predicate toPredicate(Root root, CriteriaQuery criteriaQuery, CriteriaBuilder criteriaBuilder) {
                //and 条件
                List<Predicate> predicateListAnd = new ArrayList<>();
                //or 条件
                List<Predicate> predicateListOr = new ArrayList<>();
                if (meetingRoomQueryForm.getBuildingId() != null) {
                    predicateListAnd.add(criteriaBuilder.equal(root.get("buildingId"), meetingRoomQueryForm.getBuildingId()));
                }
                if (meetingRoomQueryForm.getMinVolume() != null) {
                    predicateListOr.add(criteriaBuilder.greaterThanOrEqualTo(root.get("volume"), meetingRoomQueryForm.getMinVolume()));
                }
                if (meetingRoomQueryForm.getMaxVolume() != null) {
                    predicateListOr.add(criteriaBuilder.lessThanOrEqualTo(root.get("volume"), meetingRoomQueryForm.getMaxVolume()));
                }
                if (meetingRoomQueryForm.getIsUsing() != null) {
                    predicateListAnd.add(criteriaBuilder.equal(root.get("isUsing"), meetingRoomQueryForm.getIsUsing()));
                }
                Predicate queryAnd = criteriaBuilder.and(predicateListAnd.toArray(new Predicate[predicateListAnd.size()]));
                Predicate queryOr = criteriaBuilder.or(predicateListOr.toArray(new Predicate[predicateListOr.size()]));
                return criteriaQuery.where(queryAnd, queryOr).getRestriction();
            }
        };
        meetingRoomList = meetingRoomRepository.findAll(specification);
        return meetingRoomList;
    }
```

## lamdba
```Java
 @Override
    public List<MeetingRoom> findByManyConditions(MeetingRoomQueryForm meetingRoomQueryForm) {
        return meetingRoomRepository.findAll((root, cq, cb) -> {
            //and 条件
            List<Predicate> predicateListAnd = new ArrayList<>();
            //or 条件
            List<Predicate> predicateListOr = new ArrayList<>();
            if (meetingRoomQueryForm.getBuildingId() != null) {
                predicateListAnd.add(cb.equal(root.get("buildingId"), meetingRoomQueryForm.getBuildingId()));
            }
            if (meetingRoomQueryForm.getMinVolume() != null) {
                predicateListOr.add(cb.greaterThanOrEqualTo(root.get("volume"), meetingRoomQueryForm.getMinVolume()));
            }
            if (meetingRoomQueryForm.getMaxVolume() != null) {
                predicateListOr.add(cb.lessThanOrEqualTo(root.get("volume"), meetingRoomQueryForm.getMaxVolume()));
            }
            if (meetingRoomQueryForm.getIsUsing() != null) {
                predicateListAnd.add(cb.equal(root.get("isUsing"), meetingRoomQueryForm.getIsUsing()));
            }
            Predicate queryAnd = cb.and(predicateListAnd.toArray(new Predicate[predicateListAnd.size()]));
            Predicate queryOr = cb.or(predicateListOr.toArray(new Predicate[predicateListOr.size()]));
            return cq.where(queryAnd, queryOr).getRestriction();
        });
    }**粗体**
```



