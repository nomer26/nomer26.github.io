---
layout: post
title: Module
categories: [Ansible]
tags: [module, ansible]
fullview: true
comments: true
---




## Module

> [Built-in Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html) : 내장 모듈 (바로가기)

default Module : `command`

***ad-hoc*** <br>
다른 모듈 사용시 `-m` 옵션으로 지정
```shell
$ ansible all -m [module] -a [module options]
```
<br>

**Command**
<br>
`command` : Execute commands on targets<br>
```yaml
- name: Run command if /path/to/database does not exist (with 'cmd' parameter)
  ansible.builtin.command:
    cmd: /usr/bin/make_database.sh db_user db_name
    creates: /path/to/database
```
<br>

`raw` : Executes a low-down and dirty command<br>
```yaml
- name: Bootstrap a host without python2 installed
  raw: dnf install -y python2 python2-dnf libselinux-python
```
<br>

`script` : Runs a local script on a remote node after transferring it<br>
```yaml
- name: Run a script using an executable in a system path
  ansible.builtin.script: /some/local/script.py
  args:
    executable: python3
```
<br><br>

`shell` : Execute shell commands on targets<br>
```yaml
- name: This command will change the working directory to somedir/
  ansible.builtin.shell:
    cmd: ls -l | grep log
    chdir: somedir/
```
<br>

**File Tool**
<br>
`archive` : 압축<br>
`unarchive` : 압축 풀기<br>
`blockinfile` : 텍스트 파일에 블록 CUD<br>
`copy` : 파일 복사<br>
`fetch` : 파일 가져오기<br>
`file` : 파일 및 속성 관리<br>
`lineinfile` : 텍스트 파일에 행 관리<br>
`replace` : 텍스트 파일 문자열 관리<br>
`synchronize` : rsync 동기화<br>
`template` : Jinja2 템플릿<br>
<br>
**Package Tool**
<br>
`gem` : Ruby Gems<br>
`npm` : Node.js <br>
`pip` : Python<br>
`apt` : Debian/Ubuntu<br>
`dnf` : CentOS 8 ~<br>
`yum` : CentOS <br>
`package` : General OS<br><br>

**System Tool**
<br>
`cron`<br>
`filesystem`<br>
`firewalld`<br>
`iptables`<br>
`lvg`   : LVM volume group<br>
`lvol`  : LVM logical volume<br>
`mount`<br>
`parted` : partition<br>
`ping`<br>
`reboot`<br>
`service`<br>
`ufw` : UFW firewall<br>

**Source(Version) Control**
<br>
`git`<br>
`github_*`<br>
`gitlab_*`<br>
`bitbucket_*`<br>


**Network Tool**

`get_url`  : HTTPS/S , FTP 파일 다운로드 (wget)<br>
`uri` : 웹 서비스 (curl)

---
<br>

***user*** <br>
```yaml
- name: Add the user 'johnd' with a specific uid and a primary group of 'admin'
  ansible.builtin.user:
    name: johnd
    comment: John Doe
    uid: 1040
    group: admin
```
>  `state`  : 계정 생성,삭제 여부 ( absent,present)<br>
  `append` : 그룹에 추가할것인지 여부   ( True)<br>
  `comment` : 사용자 코멘트             ( 'comment')<br>
  `create_home` : 홈 디렉토리 생성여부  ( True)<br>
  `force`   : 사용자 강제삭제 <br>           
  `name`    : 사용자 이름               ( 'myname')<br>
  `uid`     : 사용자 ID(UID)            ( 1001)<br>
  `group`   : 기본 그룹(GID)            ( 1001)<br>
  `groups`  : 추가 그룹                 ( wheel,admin...)<br>
  `home`    : 홈 디렉토리 경로          ( /home/user)<br>
  `remove`  : 계정 삭제 여부            (  True)