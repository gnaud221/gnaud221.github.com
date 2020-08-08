---
layout: post
title:  "PLAYSOUND AND KEYBOARD"
date:   2020-08-08 04:27
categories: Coding
permalink: /archivers/playsound
---

# PLAYSOUND AND KEYBOARD

### 목차 
1. PLAYSOUND
2. KEYBOARD

## 1 PLAYSOUND

소리를 재생한다

PLAYSOUND를 이용한 작은별 코드
~~~py
from playsound import playsound
a=["do","do","sol","sol","ra","ra","sol","pa","pa","me","me","re","re","do",]  
b=0  
while len(a)>b:  
    playsound(a[b]+'.mp3')  
    b=b+1
~~~

## 2 KEYBOARD
파이썬에서 키보드의 역할을 함


~~~py
while True:  
    if keyboard.is_pressed('q'):  
        playsound("cat.wav")  
        print('You Pressed A Key!')  
        break  
 else:  
        pass
~~~





























































































