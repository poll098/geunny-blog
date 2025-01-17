---
layout:            post
title:             "SQL 트랜잭션 처리"
menutitle:         "SQL 트랜잭션 처리"
tags:              Jndi
category:          Spring
author:            geunyoung
cover:             /assets/mountain-alternative-cover.jpg
published:         true
redirect_from:     "/spring2/"
cover:             /assets/mountain-alternative-cover.jpg
language:          KO
comments:          true
math:		   false
---

이전에 시스템 관리를 하는 도중 자바 소스상으로 트랜잭션 처리를 분명 설정하였음에도 불구하고 트랜잭션이 되지 않아 몇시간을 애먹은 적이 있다. 나같은 상황을 겪지 않기를 바라며 내가 했던 실수에 대해 정리해본다. 

## Java소스

```java

@Autowired
private DbAdapter dbAdapter;
  
// 이후 로직 생략
  
SqlSession session = null;
session.openSession("데이터소스명", false);
int result = dbAdapter.insert(session, "매퍼명.쿼리ID",requestParam);
  
if(result != 1){
  session.rollback();
  session.close();  
}
  
```

문제될 것이 하나도 없는 소스인데 왜 롤백이 되지 않았을까?
정답은 dataSource설정에 있었다.

## xml설정

```xml

<environment id="환경명">
  <transactionManager type="MANAGED" />
  <dataSource type="JNDI">
	  <property name="data_source" value="스키마명" />
	</dataSource>
</environment>

```

## 경험
위의 xml이 문제의 설정이었다.
transactionManager의 type에는 MANAGED와 JDBC가 존재한다.
 - MANAGED : 자동적으로 열고 닫기가 자동으로 되며 그 외의 기능은 존재하지 않는다.
 - JDBC : commit, rollback 제공하며 특정 설정에 관해서는 open과 close도 해주어야 한다.
 
transactionManager을 JDBC로 바꾸니 정상 작동하였다.
