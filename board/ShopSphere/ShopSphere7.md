---
layout: post
title: "3. JPA Specification 패턴으로 유지보수, 확장성↑"
date: 2025-08-01
---

> 쇼핑몰 프로젝트에서 필터·정렬 기반의 상품 검색 API를 구현하며, **향후 다양한 조건이 추가될 수 있는 확장성 있는 구조**가 요구되었습니다. 이를 해결하기 위해 `Specification` 패턴을 도입했습니다.
# 검색 조건은 계속 늘어난다

프로젝트 초기에 다음과 같은 조건만 지원했습니다:

* 카테고리 ID
* 최소/최대 가격

하지만 실제 사용자 요구와 기획 변경에 따라 아래 조건들이 점차 추가되었습니다:

* 판매자 ID
* 등록일 범위
* 상품명/설명 키워드 검색
* (예정) 브랜드, 재고 수량, 할인 여부 등

기존 조건문 방식으로 이 모든 경우를 처리하면 어떻게 될까요?

```java
if (categoryId != null && minPrice != null && maxPrice == null) {
    return productRepository.findByCategoryAndMinPrice(...);
} else if (categoryId == null && minPrice != null && maxPrice != null) {
    return productRepository.findByPriceRange(...);
} // ... 조건 조합 폭발
```

> **조건 조합 지옥** 으로 유지보수 불가


# 기존 방식과 Specification 방식 비교

## Specification 패턴이란?

Spring JPA에서 제공하는 `Specification<T>` 인터페이스는 **동적 쿼리 생성을 위한 전략 패턴**입니다.
간단히 말해, 조건 하나를 메소드로 정의한 다음 여러 조건을 조합하여 쿼리를 구성할 수 있게 합니다.

## 예시: 가격 조건 Specification

```java
public static Specification<Product> minPrice(Integer minPrice) {
    return (root, query, cb) -> cb.greaterThanOrEqualTo(root.get("price"), minPrice);
}
```

## 조합 사용

```java
Specification<Product> spec = Specification.where(minPrice(10000)).and(maxPrice(50000));
Page<Product> result = productRepository.findAll(spec, pageable);
```

---

# 실제 프로젝트 적용 구조

```java
public Page<ProductDTO.Response> findProductsWithFiltersAndSort(...) {
    Specification<Product> spec = Specification.where(null); // 기본 조건

    if (categoryId != null) {
        spec = spec.and(ProductSpecifications.withCategoryId(categoryId));
    }
    if (minPrice != null) {
        spec = spec.and(ProductSpecifications.minPrice(minPrice));
    }
    if (maxPrice != null) {
        spec = spec.and(ProductSpecifications.maxPrice(maxPrice));
    }

    return productRepository.findAll(spec, pageable);
}
```

---

# 도입 효과

### **확장성**

* 브랜드, 할인 여부 등 신규 조건 추가 시 기존 코드 변경 없이 `Specification` 메소드 하나만 추가

### **단일 책임 원칙 준수**

* 각 조건은 하나의 메소드로 책임이 분리됨 → 단위 테스트 및 디버깅 용이

### **조합 유연성**

* `and`, `or`, `not` 등의 메소드로 다양한 쿼리 조합 가능

### **Repository 인터페이스 간결화**

* `findByCategoryAndPriceBetweenAndUserId...` 같은 메소드 폭발 방지

---

# 결론

| 구분       | 전통적인 조건문 방식           | JPA Specification 패턴 적용 방식  |
| -------- | --------------------- | --------------------------- |
| 조건 조합 처리 | if/else로 모든 경우 분기     | 각 조건을 독립적인 객체로 분리 후 동적 조합   |
| 재사용성     | 낮음                    | 높음 (조건 재사용 가능)              |
| 가독성      | 조건이 많아질수록 매우 나빠짐      | 각 조건이 메소드로 분리되어 가독성 우수      |
| 테스트      | 조건 분리 불가로 단위 테스트 어려움  | 각 조건 단독 테스트 가능              |
| 유지보수     | 조건 추가 시 전체 if-else 수정 | 새 Specification 메소드만 추가하면 됨 |

<style>
table {
width: 100%;
border-collapse: collapse;
margin: 20px 0;
}
th, td {
border: 2px solid #333;
padding: 12px;
text-align: center;
}
th {
background-color: #f4f4f4;
font-weight: bold;
}
td {
background-color: #fafafa;
}
table th, table td {
border: 1px solid #ddd;
}
</style>

