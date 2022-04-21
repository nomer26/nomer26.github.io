---
layout: post
title: Ansible_When
categories: [Ansible]
tags: [if, condition, ansible]
fullview: true
comments: true
---

## `when` 의 기본적인 사용법

when 절에는 변수를 다룰때 사용하는 **'{{ }}' 괄호가 필요 없는** <ins>raw Jinja2 표기법을 사용합니다.</ins>

<br>

ex) 문자열 매칭
```yaml
- name: string match
  hosts: ansi-node1
  gather_facts: no
  vars:
    str1: "aabbccddeeff"
  tasks:
  - name: pattern1
    debug:
      msg: "match pattern 1"
    when: str1 is match("aabbccddeeff")
  - name: pattern2
    debug:
      msg: "match pattern 2"
    when: str1 is search("bb..dd")
  
  # match 와 search 비슷하게 사용
```

<br>
ex) 작업상태 확인

```yaml
- name: register confirm
  hosts: ansi-node1
  gather_facts: no
#  vars:
  tasks:
  - command: ls
    register: aa
    ignore_errors: yes

  - debug:
      var: aa
  - debug:
      msg: "It is fail"
    when: aa is failed

  - debug:
      msg: "It is changed"
    when: aa is changed

  - debug:
      msg: "It is success"
    when: aa is success

  - debug:
      msg: "{{ aa.stdout }}"
    when: aa.rc == 0        

    when: (ip_addr | ipaddr) == false
    # 이런 식의 필터활용도 가능합니다
```
<br>
ex)  SELinux가 활성화된 Host만 작업 수행하는 Play

```yaml
tasks:
  - name: Configure SELinux to start mysql on any port
    ansible.posix.seboolean:
      name: mysql_connect_any
      state: true
      persistent: yes
    when: ansible_selinux.status == "enabled"
    # all variables can be used directly in conditionals without double curly braces
```
<br>

### **ansible_facts 기반 조건**

ansible_facts : <br> 
OS , IP 주소 , file system 등등 각각의 호스트의 속성들을 나타내는 Facts 모음

<br>

OS에 따라 특정 패키지를 설치할 수 있습니다.<br>
내부 IP를 가지는 호스트의 방화벽 구성을 건너뜁니다.<br>
파일 시스템이 가득 차면 정리 작업을 수행하도록 조건식을 구성할 수도 있습니다.<br>

ex) OS가 Debian 계열일 때만 실행
```yaml
tasks:
  - name: Shut down Debian flavored systems
    ansible.builtin.command: /sbin/shutdown -t now
    when: ansible_facts['os_family'] == "Debian"
```

ex) when 다중 조건식
```yaml
tasks:
  - name: Shut down CentOS 6 and Debian 7 systems
    ansible.builtin.command: /sbin/shutdown -t now
    when: (ansible_facts['distribution'] == "CentOS" and ansible_facts['distribution_major_version'] == "6") or
          (ansible_facts['distribution'] == "Debian" and ansible_facts['distribution_major_version'] == "7")
```

ex) or , and 지정 없이 사용하는 경우 and로 인식
```yaml
tasks:
  - name: Shut down CentOS 6 systems
    ansible.builtin.command: /sbin/shutdown -t now
    when:
      - ansible_facts['distribution'] == "CentOS"
      - ansible_facts['distribution_major_version'] == "6"
```

<br><br>

### **registered variables 기반 조건**

이전 작업의 결과로 작업을 실행하려는 경우,<br> `register` 키워드로 결과를 변수로 등록하고, 변수에 대한 조건식을 작성합니다.

<br>

모듈의 실행 결과를 저장한 register 변수값

![registered](/assets/images/register_variables.png)

<pre>
cmd : 실행 명령  
rc : return code  (정상 : 0 , 오류 : 2)
stderr : 표준에러 문자열
stdout : 표준출력 문자열
</pre>

<br><br>

ex) cat 결과를 motd_contents 변수로 저장 후 표준출력에 'hi'가 포함되어있다면 shell 명령 실행
```yaml
- name: Test play
  hosts: all

  tasks:

      - name: Register a variable
        ansible.builtin.shell: cat /etc/motd
        register: motd_contents

      - name: Use the variable in conditional statement
        ansible.builtin.shell: echo "motd contains the word hi"
        when: motd_contents.stdout.find('hi') != -1
```

<br>

ex) command 작업 결과에 따른 조건식 처리
```yaml
tasks:
  - name: Register a variable, ignore errors and continue
    ansible.builtin.command: /bin/false
    register: result
    ignore_errors: true

  - name: Run only if the task that registered the "result" variable fails
    ansible.builtin.command: /bin/something
    when: result is failed

  - name: Run only if the task that registered the "result" variable succeeds
    ansible.builtin.command: /bin/something_else
    when: result is succeeded

  - name: Run only if the task that registered the "result" variable is skipped
    ansible.builtin.command: /bin/still/something_else
    when: result is skipped
```

<br>

### **variables 기반 조건**

플레이북이나 인벤토리에 정의된 변수들을 기반으로 조건문을 작성할 수 있습니다.

>Boolean 값을 가지지 않는 변수를 조건문에서 사용하려면 `|bool` 필터를 사용해야합니다.

<br>

ex) epic , monumental 변수값에 따른 조건식 작성
```yaml
vars:
  epic: true
  monumental: "yes"

tasks:
    - name: Run the command if "epic" or "monumental" is true
      ansible.builtin.shell: echo "This certainly is epic!"
      when: epic or monumental | bool

    - name: Run the command if "epic" is false
      ansible.builtin.shell: echo "This certainly isn't epic!"
      when: not epic
```
<br>

### **반복문에서 조건문활용**

```yaml
## List
- name: Skip the whole task when a loop variable is undefined
  ansible.builtin.command: echo {{ item }}
  loop: "{{ mylist|default([]) }}"
  when: item > 5
  
## Dict
- name: The same as above using a dict
  ansible.builtin.command: echo {{ item.key }}
  loop: {{ query('dict', mydict|default({})) }}
  when: item.value > 5
```


### **일반적으로 사용되는 Facts**
<br>

**ansible_facts['distribution']**

    Alpine
    Altlinux
    Amazon
    Archlinux
    ClearLinux
    Coreos
    CentOS
    Debian
    Fedora
    Gentoo
    Mandriva
    NA
    OpenWrt
    OracleLinux
    RedHat
    Slackware
    SLES
    SMGL
    SUSE
    Ubuntu
    VMwareESX

**ansible_facts['os_family']**

    AIX
    Alpine
    Altlinux
    Archlinux
    Darwin
    Debian
    FreeBSD
    Gentoo
    HP-UX
    Mandrake
    RedHat
    SGML
    Slackware
    Solaris
    Suse
    Windows