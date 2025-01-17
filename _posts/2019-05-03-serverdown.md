---
layout:            post
title:             "서버 다운시 원인 파악하기(Tomcat)"
menutitle:         "서버 다운시 원인 파악하기(Tomcat)"
tags:              server
category:          Mistakes & Tips
author:            geunyoung
cover:             /assets/mountain-alternative-cover.jpg
published:         true
cover:             /assets/mountain-alternative-cover.jpg
language:          KO
comments:          true
---

## 에러 발생

잘 되고 있던 서비스가 갑작스럽게 죽었다는 문의가 왔다.

서버 환경은 Nginx/Tomcat이 었으며,
서비스의 인프라는 Web Server 2대, WAS 2대였다.

잘되던 서버가 갑자기 죽었다?

원인을 파악하기 시작했다.


## 문제 접근 1

우선 서비스 로그를 확인하였다. WAS한대에서는 로그가 있는 반면에 다른 WAS에서는 서비스 로그가 없는 것을 확인하였다.

거기서 파악할 수 있는 것은 우선 WebServer(Nginx)에서 문제가 있었다면 WAS 2대 다 서비스 로그가 안찍혀야하지만 한대에 찍혀있다는 것에서 WebServer에는 문제가 없다고 생각했으며, WAS가 제대로 종료되었으면 클러스터링으로 인해 문제가 되지 않았을텐데 계속해서 사용자가 불편함을 겪었다면 서버가 온전한 상태로 켜져 있지 않은 상태라고 생각하였다.


## 문제 접근 2

WAS 문제로 인식 후에 catalina의 로그를 확인.

<aside>
<figure>
<img src="{{ "/media/img/Mistakes/server_error01.png" | absolute_url }}" />>
</figure>
</aside>

로그를  살펴보면

Waiting for 1 instance to be deallocated ->서버가 재기동이 되었고

but failed to unregister it when the web application was stopped. To prevent a memory leak, the JDBC Driver has been forcibly unregistered -> JDBC Driver가 문제생겨 장제로 종료하려 했지만

but has failed to stop it. This is very likely to create a memory leak. -> 종료하지 못하여 memort leak이 발생하였다

라는 걸 볼 수 있다.

## 문제 접근 3

그럼 무엇 때문에 서버가 재기동 되었을까?

그전에 일어났던 내역을 제니퍼 툴을 활용하여 데이터 확인을 해보니 insert 및 update하는 데 10분 이상이  걸린 내역이 있으며, 트랜잭션이 꽉차서 처리하지 못햇다는 에러 내역들이 존재 하였다.

그것때문에 서버가 과부하 걸리다 재기동 되었는지 확답은 할 수 없지만 우선 해당 쿼리에 대해 수정을 하고 지켜보기로 결정하였다.
