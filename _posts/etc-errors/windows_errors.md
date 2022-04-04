---
layout: post
title: Windows errors
categories: [error]
tags: [Windows, error ]
fullview: true
comments: true
---


## Powershell
<pre>
. : 이 시스템에서 스크립트를 실행할 수 없으므로 C:\Users\xxx\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 파일을 로드할 수 없습니다. 자세한 내용은 about_Execution_Policies(https://go.microsoft.com/fwlink/?L inkID=135170)를 참조하십시오.
위치 줄:1 문자:3
+ . 'C:\Users\xxx\Documents\WindowsPowerShell\Microsoft.PowerShell_pr ...
+   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : 보안 오류: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
</pre>

**상황 :** VSCode 단축키로 shell을 통해 Go 언어를 실행 시켰습니다.

**원인 :** Powershell 이 스크립트 실행을 막아놓은 상태였습니다.

**해결 :**
<pre>
명령어 다음과 같이 입력

<b>ExecutionPolicy</b>      <-- 현재상태확인
>> Restricted              <-- 모든 스크립트 막은 상태

 
<b>Set-ExecutionPolicy Unrestricted</b>  <-- 모든 스크립트 허용
<b>ExecutionPolicy</b>      <-- 현재상태확인
>> Unrestricted           <-- 모든 스크립트 허용 상태.
</pre>

>`참조`<br>
> PowerShell 명령어 : https://samsons.tistory.com/16










