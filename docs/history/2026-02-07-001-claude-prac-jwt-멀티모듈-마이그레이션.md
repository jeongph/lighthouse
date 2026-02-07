---
title: prac-jwt → tools-of-spring 멀티모듈 마이그레이션
description: prac-jwt 히스토리를 보존하며 tools-of-spring 멀티모듈 JWT 인증 라이브러리로 전환
date: 2026-02-07
author: claude
project: tools-of-spring
tags:
  - feat
  - refactor
---

# prac-jwt → tools-of-spring 멀티모듈 마이그레이션

## Why

- prac-jwt는 JWT 인증 스켈레톤(SecurityConfig, HomeController만 존재, JWT 미구현)이었다
- tools-of-spring은 Spring 활용 모듈을 모아두는 빈 프로젝트였다
- prac-jwt의 의도를 살려 완전한 JWT 인증 모듈을 구현하고, tools-of-spring을 멀티모듈로 전환하여 첫 모듈로 배치할 필요가 있었다
- prac-jwt 레포를 삭제할 수 있도록 커밋 히스토리 보존이 필요했다

## How

1. **커밋 히스토리 병합**: `git subtree add --prefix=spring-module-jwt prac-jwt/main`으로 prac-jwt 4개 커밋을 tools-of-spring에 보존
2. **멀티모듈 전환**: 루트가 예제 앱 역할을 겸하고, `spring-module-jwt`를 서브모듈로 배치
3. **JWT 라이브러리 구현**: jjwt 0.12.6 기반, Spring Boot Auto-configuration 패턴 적용
4. **Java 21 toolchain**: 시스템에 Java 17이 없어 Java 21로 toolchain 변경
5. **Git Flow**: main → develop → feature/multi-module-jwt, PR 생성 후 머지

## What

### 프로젝트 구조
- `spring-module-jwt`: JWT 인증 라이브러리 (plain JAR)
  - JwtProperties, JwtTokenProvider, JwtAuthenticationFilter
  - JwtAuthenticationEntryPoint, JwtAccessDeniedHandler
  - JwtAutoConfiguration (Spring Boot 자동 설정)
  - 단위 테스트 3개 (Provider, Filter, AutoConfiguration)
- 루트 `src/example/`: 예제 앱 (루트 프로젝트가 Spring Boot 앱 겸임)
  - 회원가입/로그인 API, H2 DB
  - 통합 테스트 (전체 인증 흐름)

### 커밋 이력 (feature/multi-module-jwt → develop)
1. `refactor: prac-jwt 코드를 멀티모듈 구조로 전환`
2. `feat(jwt): JWT 인증 라이브러리 모듈 구현`
3. `test(jwt): JWT 모듈 단위 테스트 작성`
4. `feat(jwt-example): JWT 모듈 사용 예제 애플리케이션`
5. `docs: README 모듈 구조 및 사용법 업데이트`
6. `refactor: 예제 모듈을 루트 src/example 패키지로 통합`

### PR
- https://github.com/jeongph/tools-of-spring/pull/1
