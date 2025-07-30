---
layout: post
title: 2. N+1 문제 해결(Fetch Join)
date: 2025-07-30
---

> 쇼핑몰 프로젝트에서 리뷰 API의 응답 시간이 3초 이상 소요되는 심각한 병목이 발생했습니다. 해결하기 위한 분석, 접근 방식, 그리고 성능 개선 결과를 단계적으로 정리합니다.

# 문제 상황: 리뷰 API 응답 지연

* 대상 API: `/api/reviews/product/{productId}` (특정 상품에 대한 리뷰 목록 조회)
* 문제 지점: 리뷰 하나당 연관된 유저 정보(User), 상품 정보(Product), 이미지/옵션 등의 반복 조회 발생
* 평균 응답 시간: **3.2초 이상**
* 병목 원인: **전형적인 N+1 문제**

> N+1 문제란?
> 하나의 쿼리로 가져온 데이터 N개에 대해 각각 추가 쿼리가 발생하는 상황으로, 성능 저하의 대표적인 원인입니다.

# 원인 분석: 쿼리 로그 & APM 분석

* `ReviewService.getReviewsByProductId()` 내부에서 `reviewRepository.findByProduct_Id()` 호출
* 해당 메서드가 반환한 `List<Review>` 내 각 `Review` 객체의 `.getUser()`, `.getProduct()` 접근 시마다 추가 쿼리 발생
* Product 내부의 연관 객체들(`images`, `options`, `category`, `seller`)까지 지연 로딩되어 수십 개의 쿼리 실행
* 총 발생 쿼리 수: **최대 1 + (N × 5)** 수준

# 해결 방법 1: @EntityGraph 통한 Fetch Join 적용

```java
@EntityGraph(attributePaths = {"user", "product"})
@Query("SELECT r FROM Review r WHERE r.product.id = :productId")
List<Review> findByProduct_Id(@Param("productId") Integer productId);
```

* `user`, `product`를 명시적으로 fetch join하여 지연 로딩 제거
* 첫 쿼리 한 번으로 Review + User + Product 데이터 일괄 조회 가능

## 개선 효과

* 쿼리 수: 약 90% 감소
* 평균 응답 시간: **3.2초 → 1.4초**

# 해결 방법 2: 리뷰 통계 계산 최적화

* 이전 방식: 리뷰마다 `reviewRepository.countByProductId`, `findAverageRatingByProductId` 호출
* 개선 후: 해당 필드를 Product Entity의 캐시 필드로 반영하거나, 필요한 경우 다음과 같이 DTO 프로젝션으로 한 번에 조회

```java
@Query("SELECT new com.example.dto.ReviewSummaryDTO(r.product.id, COUNT(r), AVG(r.rating)) FROM Review r GROUP BY r.product.id")
List<ReviewSummaryDTO> findReviewSummaries();
```

* ProductService 혹은 배치성 갱신으로 정적 필드 유지
* 또는, 프론트에서 평균/개수 분리 요청으로 분산 처리 가능

## 개선 효과

* 평균 응답 시간: **1.4초 → 0.5초**
* 중복 호출 제거, 부하 분산

# 최종 결과: 84% 성능 개선

| 개선 전            | 개선 후           |
| --------------- | -------------- |
| 평균 응답 시간: 3.2초  | 평균 응답 시간: 0.5초 |
| 총 쿼리 수: 최대 120개 | 총 쿼리 수: 2\~3개  |
| 서버 CPU 사용률 급증   | 서버 부하 안정화      |

> APM 기준 요청 수 대비 평균 처리 시간 84% 감소 확인

# 꺠달음

* **EntityGraph를 통한 Fetch Join은 중요**: 즉시로딩 대상 명시
* **리뷰 통계는 지연 계산보다 DTO projection 또는 캐시 활용이 효과적**
* **모든 연관 데이터 로딩은 DTO 변환 전 필요 여부를 판단 후 설계**

# 결론

N+1 문제는 단순한 쿼리 문제가 아닌, 설계상의 구조적 문제입니다.
Spring JPA 환경에서 연관 데이터가 많은 도메인을 설계할 때, **Fetch Join 전략**, **DTO Projection**, **지연 계산 분산 처리** 등 성능을 고려한 구조 설계가 중요함을 다시금 실감한 개선 작업이었습니다.

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
