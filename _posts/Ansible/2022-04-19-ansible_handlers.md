---
layout: post
title: Ansible_Handler
categories: [Ansible]
tags: [handler,notify,listen, ansible]
fullview: true
comments: true
---



## Handler

ex) /etc/foo.conf 파일에 template.j2 내용을 덮어쓰기
```yaml
- name: Template configuration file
  ansible.builtin.template:
    src: template.j2
    dest: /etc/foo.conf
  notify:
    - Restart memcached
    - Restart apache

  handlers:
    - name: Restart memcached
      ansible.builtin.service:
        name: memcached
        state: restarted

    - name: Restart apache
      ansible.builtin.service:
        name: apache
        state: restarted
```
위 task에서 <ins>changed 상태가 된다면</ins> **`notify` 키워드로 추가적인 작업들을(handler) 정의할 수 있습니다.**

`handler`는 task와 독립적으로 작성해주어야 합니다.<br>
**task의 notify 에 작성된 목록과 handlers에 정의한 이름이 일치한다면 해당 handler를 실행합니다**

<br>
<br>

### **Handler 동작방식**

1. **task를 수행후 changed 상태를 감지합니다.**
2. **changed 상태라면 모든 task작업이 종료된 후 일치하는 handler name 을 찾아 실행합니다.**

+ **handler는 호출된 순서대로 실행됩니다.**

<br>
<br>

ex) handler가 listen하여 memcached 서비스 재시작
```yaml
handlers:
  - name: Restart memcached
    ansible.builtin.service:
      name: memcached
      state: restarted
    listen: "restart web services"

  - name: Restart apache
    ansible.builtin.service:
      name: apache
      state: restarted
    listen: "restart web services"

tasks:
  - name: Restart everything
    ansible.builtin.command: echo "this task will restart the web services"
    notify: "restart web services"
```
위와 같이 `listen` 키워드를 통해 handler가 직접 호출을 받을 수도 있습니다.