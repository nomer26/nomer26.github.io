---
layout: post
title: Ansible_Tag
categories: [Ansible]
tags: [tag, ansible]
fullview: true
comments: true
---


## Tag

Playbook 에 Task 가 많은 경우 일부분만 실행 또는 실행하지 않고 싶을 때 Tag 를 사용할 수 있습니다.

Tag 는 Play , Block , Role , Task 에 설정할 수 있고 실행할 때 Tag를 선택하면 됩니다.

```yaml
tasks:
- name: Install the servers
  yum:
    name:
      - httpd
      - memcached
  state: present
  tags:
    - packages
    - webservers
- name: Configure the service
  template:
    src: templates/src.j2
    dest: /etc/foo.conf
  tags:
    - configuration
```


command option

--tags all : 모든 작업 실행 (default)
--tags [ tag1, tag2 ] : tag1 또는 tag2가 있는 작업만 실행
--skip-tags [tag3, tag4] : tag3 또는 tag4 태그가 있는 작업 건너뛰기
--tags tagged : 태그가 설정된 작업만 실행
--tags untagged: 태그가 없는 작업만 실행

--list-tags : 사용 가능한 태그 목록 확인
--list-tasks --tags : 태그가 붙은 작업 목록 확인
--list-tasks --skip-tags : 태그가 없는 작업 목록 확인