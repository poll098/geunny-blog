---
layout:            post
title:             "전화번호 목록"
menutitle:         "해시를 이용한 알고리즘 2"
tags:              Algorithm Hash
category:          Algorithm
author:            geunyoung
cover:             /assets/mountain-alternative-cover.jpg
published:         true
redirect_from:     "/algoith2/"
cover:             /assets/mountain-alternative-cover.jpg
language:          KO
comments:          true
math:		   false
---



## 문제설명

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

구조대 : 119
박준영 : 97 674 223
지영석 : 11 9552 4421
전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.


## 제한사항
 - phone_book의 길이는 1 이상 1,000,000 이하입니다.
 - 각 전화번호의 길이는 1 이상 20 이하입니다.


## 입출력 예
입출력 예 #1 앞에서 설명한 예와 같습니다.

입출력 예 #2 한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3 첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.



## 본인답안

```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Set;

class Solution {
    public boolean solution(String[] phone_book) {
        HashMap<String, Integer> temMap = new HashMap<>();
        for(String phone_number : phone_book){
        	Set set = temMap.keySet();
        	Iterator iterator = set.iterator();
        	while(iterator.hasNext()){
        		Object object = iterator.next();
        		int result = phone_number.indexOf(object.toString());
        		if(result == 0){
        			return false;
        		}
        		
        		result = object.toString().indexOf(phone_number);
        		if(!(result == 0)){
        			return false;
        		}
        	}
        	temMap.put(phone_number, 0);
        }
        return true;
    }
}
```


## 점수가 높았던 답안

```java
import java.util.HashMap;

class Solution {
    public boolean solution(String[] phoneBook) {
       for(int i=0; i<phoneBook.length-1; i++) {
            for(int j=i+1; j<phoneBook.length; j++) {
                if(phoneBook[i].startsWith(phoneBook[j])) {return false;}
                if(phoneBook[j].startsWith(phoneBook[i])) {return false;}
            }
        }
        return true;
    }
}
```

## 알게된 점 및 아쉬운 점

 - 문제를 제대로 읽지 않아 시간 낭비가 길었습니다.(접두어인 경우인데 존재하는 지 여부로 생각하고 풀었습니다. )
 - 해시맵이라 해서 억지로 해시맵을 만든 느낌이 강하네요...
 - 배열 함수중 startsWith라는 게 있네요
