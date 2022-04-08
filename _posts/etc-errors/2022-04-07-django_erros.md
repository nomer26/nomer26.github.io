---
layout: post
title: Django errors
categories: [error]
tags: [Django, error ]
fullview: true
comments: true
---


## **Staticfiles**

장애 발생 시점에 오류메세지를 캡쳐하지 못했습니다.

**상황 :** 정적 파일 (이미지 파일)을 폴더에서 불러오는 중 찾지못함.

**원인 :** 정적 파일 경로 미지정

**해결 :** 'settings.py' 파일에 아래 코드 추가

```python
STATIC_URL = '/static/'

STATICFILES_DIRS = [ os.path.join(BASE_DIR,'이미지 보관 폴더'),]
```
이미지가 필요한 html 파일에  load static   명령을 추가해주었더니 정상적으로 이미지를 불러 올 수 있었습니다.

<br><br><br> 

--- 

<br><br><br><br>

## **502 Gateway**

**상황 :** AWS EC2에서 이미지로 인스턴스 시작 시  502 Bad Gateway 에러 발생

**원인 :** 502 Bad Gateway 원인은 매우 다양하고 많았습니다.

    - 로드 밸런서 서브넷에서 대상 포트의 대상으로 트래픽이 허용되는지 확인
    - 대상응답의 형식이 잘못되었거나 유효하지 않은 HTTP 헤더가 포함
    - 대상의 연결 유지 기간이 로드 밸런서 유휴시간 초과 값보다 짧은지 확인
    - 로드밸런서가 대상에 연결할 때 SSL H.S 시간 초과
    - 대상이 Lambda 일때

제 경우에는 uwsgi 문제였습니다.<br>
추측하건데 nginx 보다 django 가 우선적으로 로딩? 이 되어야 정상적으로 작동한다고 하는데 아마도 이 문제가 아닐까 생각합니다.<br>
그것도 아니라면 uwsgi 연결 문제일 것 같습니다.

**해결 :** 
```python
uwsgi --plugin python3 --ini uwsgi.ini  # 코드 실행 
```
또는 uwsgi.ini 구성파일에 플러그인을 설정해주어도 된다고 하지만
```python
[uwsgi]
plugins=python3 
...
```
저는 명령어로만 해결이 되었습니다.










