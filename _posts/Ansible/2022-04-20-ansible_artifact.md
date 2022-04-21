---
layout: post
title: Ansible_Artifact
categories: [Ansible]
tags: [ artifact, ansible]
fullview: true
comments: true
---

## Artifact

하나의 Playbook 에 모든 작업을 정의하게 되면 크기가 매우 커지고
전체 빌드의 속도도 느리며 관리가 힘들어질 수 있습니다.

이런 경우 여러 파일로 나누어 작업 집합을 구성하면 재사용할 수 있어 편리합니다.

재사용 가능한 아티팩트

 - Variables file
 - Task file
 - Playbook file
 - Role
  


Playbook 에서 변수를 재사용하는 경우 

`vars_files` 키워드를 사용하여 변수를 참조할 수 있습니다.
```yaml
- hosts: all
  remote_user: root
  vars:
    favcolor: blue
  vars_files:
    - /vars/external_vars.yml
  tasks:
    - name: This is just a placeholder
      ansible.builtin.command: /bin/echo foo
```

Task 에서 변수를 재사용하는 경우

`include_vars` 모듈을 사용하여 변수를 참조할 수 있습니다.

```yaml
tasks:
  - name: Bare include (free-form)
    include_vars: myvars.yaml
tasks:
  - name: Include vars of stuff.yaml into the 'stuff' variable (2.2).
    include_vars:
    file: stuff.yaml
      name: stuff
```