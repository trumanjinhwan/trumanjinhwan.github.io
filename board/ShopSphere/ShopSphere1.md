---
layout: post
title: "1. Spring Boot(백엔드) 절차"
date: 2025-06-14
---

> 대부분의 웹 애플리케이션은 CRUD(Create, Read, Update, Delete)로 이루어져 있으며, Spring에서는 이러한 흐름이 일정한 패턴을 따릅니다. 본 글에서는 '회원가입(Register)' 기능을 중심으로, Spring 백엔드의 핵심 구조인 Controller → DTO → Service → Entity → Repository → DTO → Controller 흐름을 예제로 분석해보겠습니다.


# 전체 흐름 개요

회원가입 시 사용자의 정보를 JSON으로 받아, DB에 저장한 후 그 결과를 다시 JSON 형태로 응답하는 기본 구조는 다음과 같습니다.
<div style="text-align: center;">
    <img src="/사진들/ShopSphere/백엔드흐름.png" alt="alt text" />
</div>
이 시퀀스는 대부분의 CRUD 로직에 동일하게 적용되며, 각 계층의 책임이 명확하게 분리된 구조입니다.


# 1. Controller: 요청 수신과 응답 처리

```java
@PostMapping("/register")
public ResponseEntity<?> register(@RequestBody UserDTO.RegisterRequest userDTO) {
    try {
        UserDTO.Response user = userService.register(userDTO);
        return ResponseEntity.ok(user);
    } catch (RuntimeException e) {
        return ResponseEntity.badRequest().body(Map.of("message", e.getMessage()));
    }
}
```

## 설명

* `/register` 경로로 들어온 POST 요청을 처리합니다.
* JSON → `UserDTO.RegisterRequest`로 역직렬화됩니다.
* Service 레이어로 요청을 위임하고, 응답은 HTTP 200 + DTO로 반환합니다.


# 2. DTO: 데이터 전달 객체

```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public static class Response {
    private String id;
    private String email;
    private String name;
    private String phoneNumber;
    private String address;
    private String role;
}
```

## 설명

* Controller ↔ Service ↔ Entity 간 데이터 이동에 사용됩니다.
* `RegisterRequest`는 입력 데이터, `Response`는 출력 데이터용입니다.
* Entity의 모든 필드를 노출하지 않고 필요한 필드만 선택적으로 반환할 수 있도록 합니다.

# 3. Service: 비즈니스 로직 처리

```java
public UserDTO.Response register(UserDTO.RegisterRequest userDTO) {
    Optional<User> existingUser = userRepository.findById(userDTO.getId());
    if (existingUser.isPresent()) {
        throw new RuntimeException("이미 아이디가 존재합니다.");
    }

    User user = User.builder()
        .id(userDTO.getId())
        .name(userDTO.getName())
        .email(userDTO.getEmail())
        .password(passwordEncoder.encode(userDTO.getPassword()))
        .phoneNumber(userDTO.getPhoneNumber())
        .address(userDTO.getAddress())
        .role("USER")
        .build();

    User savedUser = userRepository.save(user);

    return UserDTO.Response.builder()
        .id(savedUser.getId())
        .name(savedUser.getName())
        .email(savedUser.getEmail())
        .phoneNumber(savedUser.getPhoneNumber())
        .address(savedUser.getAddress())
        .role(savedUser.getRole())
        .build();
}
```

## 설명

* ID 중복 체크 → 비밀번호 암호화 → Entity 변환 → 저장 → DTO 반환
* 비즈니스 로직의 핵심 처리 구간이며, 예외 발생 시 Controller에 위임합니다.

# 4. Entity: 데이터베이스 매핑 객체

```java
@Entity
@Table(name = "user")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class User {
    @Id
    @Column(name = "user_id", nullable = false, unique = true)
    private String id;

    @Column(nullable = true)
    private String email;

    @Column(nullable = true)
    private String password;

    @Column(nullable = true)
    private String name;

    @Column(name = "phone_number", nullable = true)
    private String phoneNumber;

    @Column(nullable = true)
    private String address;

    @Column(nullable = true)
    private String role;

    @Column(name = "created_at", nullable = true)
    private LocalDateTime createdAt;
}
```

## 설명

* `@Entity`는 JPA가 관리하는 테이블 객체임을 명시합니다.
* `@Id`로 기본 키 지정, `@Column`으로 필드 매핑
* Builder 패턴을 통해 객체 생성을 간결하게 처리할 수 있습니다.

# 5. Repository: DB 접근 계층

```java
@Repository
public interface UserRepository extends JpaRepository<User, String> {
    Optional<User> findByEmail(String email);
    Optional<User> findById(String id);
}
```

## 설명

* Spring Data JPA를 사용하면 기본적인 CRUD는 자동 구현됩니다.
* 메서드 명명 규칙을 통해 커스텀 쿼리도 작성할 수 있습니다.

---

# 정리

이러한 구조는 Spring 애플리케이션에서 가장 전형적인 CRUD 처리 흐름으로, 다음과 같은 장점이 있습니다:

* **관심사 분리**: 각 계층의 역할이 명확해 유지보수 용이
* **확장성**: DTO, Entity, Service를 재사용하여 새로운 기능도 쉽게 구현 가능
* **일관성**: Create뿐 아니라 Read, Update, Delete에도 동일한 흐름을 재활용 가능
