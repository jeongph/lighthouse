# naming convention

## project name (==github repository name)

### 규칙

- {모듈범위}-{모듈타입}-{모듈명}
    - e.g)
        - spring-module-abc -> Spring 프로젝트 전반부에서 사용할 abc 모듈
        - team-module-abc -> 보통 이런경우는 팀에서 사용하는건데 개인은 팀이 없으니까 그룹 (쓸일이 있나?)
        - project-module-abc -> 해당 서비스(프로젝트)에서만 사용하는 모듈

### 서비스타입 종류

- svc = service
- mlt = multi (service + modules)
- mdl = module

### 주의사항

- 기본적으로 프로젝트 package 는 {서비스타입} 을 제외하고 작성
    - e.g) ~.spring.abc