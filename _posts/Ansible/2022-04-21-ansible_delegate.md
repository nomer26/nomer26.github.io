---
layout: post
title: Ansible_Delegate
categories: [Ansible]
tags: [delegate, ansible]
fullview: true
comments: true
---


## Delegate

`delegate_to` 모듈로 현재 작업을 넘겨줄 수 있습니다.

대상이 인벤토리에 없는경우 `add_host` 모듈을 사용해 임시로 인벤토리에 추가하여 사용합니다.

```yaml
- name: add delegation host
  add_host:
         name: demo
         ansible_host: 172.21.250.10
         ansible_user: devops

- name: echo hello
  command: cmd "Hello from {{ inventory_hostname }}"
         delegate_to: demo
         register: ouput
- debug:
     msg: "{{ output.stdout }}"
```

<br><br>

`delegate_facts` 모듈을 사용해 fact 위임도 가능합니다.
```yaml
- name: echo hello
  command: cmd "Hello from {{ inventory_hostname }}"
         delegate_to: demo
         delegate_facts: yes
         register: ouput
```


<br><br><br><br><br>

> `출처` <br>
> https://taesany.tistory.com/140?category=675528