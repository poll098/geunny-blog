---
layout:            post
title:             "Apach/Tomcat - SSL 등록 및 교체"
menutitle:         "Apach/Tomcat - SSL 등록 및 교체"
tags:              Server
category:          Server
author:            geunyoung
cover:             /assets/mountain-alternative-cover.jpg
published:         true
redirect_from:     "/server2/"
cover:             /assets/mountain-alternative-cover.jpg
language:          KO
comments:          true
math:		   false
---

## 환경파일 찾기

우선 Apache 파일 위치부터 찾아보자.
필자의 해당 서비스 인프라는 OS가 Windows이며, Server는 Apache/Tomcat이다.
이다.

<aside>
<figure>
<img src="{{ "/media/img/Server/apache.png" | absolute_url }}" />
</figure>
</aside>

해당 위치에 설정파일이 있다.

## 환경파일 세팅하기

```xml

LoadModule ssl_module modules/mod_ssl.so

```

위의 내용이 주석처리가 되어 있을 텐데 주석을 풀어준다. 해당 내용은 아파치에서 제공해주는 SSL 모듈을 사용하겠다는 뜻이다.


```xml

<IfModule ssl_module>
SSLRandomSeed startup builtin
SSLRandomSeed connect builtin
</IfModule>

```

이후, 위의 내용도 주석처리나 안되어 있다면 추가해준다. ssl_module 사용시 세팅에 관한 태그인데 해당 내용은 서비스가 켜질때 SSL이 seedind이 되기 위한 규칙중 Pseudo Random Number Generator (PRNG)의 규칙을 활용한다는 뜻이다. 문서에 의하면 해당 규칙은 그다지 강력한 보안 및 정보를 담지못하며, 만약 다른 외부 방식으로 설정하고 싶다면 SSLRandomSeed startup file:/dev/ssl_num_random, SSLRandomSeed connect file:/dev/ssl_num_random 으로 정할 수 있다.


```xml

Include conf/SSL키매핑할경로및파일명.conf

```

으로 ssl매핑할 내용들만 파일로 따로 만들서 관리해준다. 


```xml

<VirtualHost _default_:443>
	
JkMount ssl 사용할 어플케이션 context 및 서블릿ServerName
	
ServerName 도메인명:포트
	
SSLProtocol all -SSLv2
#어떤 프로톨 콜 사용할지 해당 설정은 SSLv2 빼고(-) 다 사용하겠다는 의미. 이유는 1.6에서 SSLv2가 Handshake건헐적으로 발생하여 제거하였음

SSLCertificateChainFile "C:/server/Apache/conf/ssl/체인파일명.pem"
SSLCertificateKeyFile "C:/server/Apache/conf/ssl/키파일명.pem"
SSLCertificateFile "C:/server/Apache/conf/ssl/인증서파일.pem"

</VirtualHost>
		
```

SSL키매핑할경로및파일명.conf에서 위의 내용만 프로젝트에 맞게 설정라고 나머지는 샘플과 동일하게 하면된다.

Windows같은 경우 개인키의 패스워드를 제거하고 사용해야 하는데 방법은 openssl프로램을 설치하고 해당 위치에서 cmd를 열어

openssl rsa -in 기존개인키.pem -out 새로운개인키.pem

의 명령어를 실행해 그것을 적용시킨다.



## 확인하기

이 후 아파치를 재구동하여 확인하면 된다.
포트확인은 netstat -na | grep 설정포트
웹페이지로 확인은 https://URL:포트로 확인 가능하다.


