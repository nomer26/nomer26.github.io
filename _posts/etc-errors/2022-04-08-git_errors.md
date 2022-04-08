---
layout: post
title: Git errors
categories: [error]
tags: [Git, error ]
fullview: true
comments: true
---


## **Build errror**
```
Liquid Warning: Liquid syntax error (line 213): Expected end_of_string but found string in "{{define "T"}}" in /github/workspace/_posts/GO/2022-04-04-GO_Feature.md
  Liquid Exception: Liquid syntax error (line 16): Unknown tag 'load' in /github/workspace/_posts/2022-04-07-django_erros.md
```
**상황 :** 평소와 같이 깃블로그에 포스트를 올리고있는데 아무리 기다려도 업로드가 안됩니다.

**원인 :** 글에  '{'% command %'}'  형식으로 작성된 것이 빌드과정에서 문제가 된 것 같습니다.

**해결 :**  저 명령문을 지우고 올렸더니 정상적으로 업로드가 되었습니다.



<br><br><br> 

--- 

<br><br><br><br>










