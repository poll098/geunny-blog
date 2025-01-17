---
layout:            post
title:             "더 맵게"
menutitle:         "힙을 이용한 알고리즘 1"
tags:              Algorithm Heap
category:          Algorithm
author:            geunyoung
cover:             /assets/mountain-alternative-cover.jpg
published:         true
redirect_from:     "/algoith6/"
cover:             /assets/mountain-alternative-cover.jpg
language:          KO
comments:          true
math:		   false
---



## 문제설명

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

## 제한사항
 - scoville의 길이는 1 이상 1,000,000 이하입니다.
 - K는 0 이상 1,000,000,000 이하입니다.
 - scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
 - 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.


## 입출력 예
스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5
가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]

스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13
가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

## 본인답안

```java
    import java.util.Stack;

    public static int solution(String arrangement) {
    	char[] charArray = arrangement.toCharArray();
    	
    	int stickCount = 0;
    	char tempChar = 'x';
    	
    	Stack<Character> stack = new Stack<Character>();
    	for(char word : charArray ){
    		if(stack.empty()){
    			stack.push(word);
    		}else{    			
    			if(word == ')'){
    				stack.pop();
    				
    				if(tempChar == '('){
    					stickCount += stack.size();
    				}else{
    					stickCount += 1;
    				}
    			}else{
    				stack.push(word);
    			}
    		}
    		tempChar = word;
    	}
        return stickCount;
    }
```


## 점수가 높았던 답안

```java
import java.util.*;

class Solution {
    public static int solution(String arrangement) {
        int answer = 0;
        Stack<Integer> st = new Stack<>();
        for (int i = 0; i < arrangement.length(); i++) {
            if (arrangement.charAt(i) == '(') {
                st.push(i);
            } else if (arrangement.charAt(i) == ')') {
                if (st.peek() + 1 == i) {
                    st.pop();
                    answer += st.size();
                } else {
                    st.pop();
                    answer += 1;
                }
            }
        }
        return answer;
    }
}
```

## 알게된 점 및 아쉬운 점



