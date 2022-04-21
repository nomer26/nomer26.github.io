---
layout: post
title: Ansible_Role
categories: [Ansible]
tags: [role, ansible]
fullview: true
comments: true
---




## ROLE

    - defaults : 우선순위가 낮은 기본 변수 집합. (공용 변수)
    - files : 파일을 넣어놓고 필요할 때 사용가능 ( copy -> unarchive ) 대량 파일 처리
    - handlers : 데몬,서비스 관리
    - meta : 의존성 관련한 것
    - tasks : 작업들
    - templates : 템플릿들
    - vars : 변수 집합 ( defaults 보다 우선순위 )
    - dependencies : role을 실행하기 전 선수 조건 실행
    - tags : 태그들

<br><br>

> ansible-galaxy init --offline [path] > 기본 role 구조를 만들어줍니다.

```
-- roles
    `-- test
        |-- README.md
        |-- defaults
        |   `-- main.yml
        |-- handlers
        |   `-- main.yml
        |-- meta
        |   `-- main.yml
        |-- tasks
        |   `-- main.yml
        |-- tests
        |   |-- inventory
        |   `-- test.yml
        `-- vars
            `-- main.yml
```
