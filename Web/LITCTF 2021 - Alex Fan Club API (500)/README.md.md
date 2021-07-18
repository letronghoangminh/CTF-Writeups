![[Pasted image 20210718142935.png]]

## This is the problem from LIT CTF 2021. At the time i wrote this writeup, it has only 6 solves (2 was mine lol xD)

### Here is the link of challenge: https://alex-fan-club-api.litctf.live/

![[Pasted image 20210718143143.png]]

#### We see one endpoint here */api*  and a template POST request to this endpoint to query for information in database
#### This request used xml so the first idea comes through me was a XXE, but let see

#### After downloading the challenge's file, we have source code of the challenge. I will focus on the main file *alexfanclubapi.py* where the bussiness happen. Make sure to always to try run these challs locally

![[Pasted image 20210718143708.png]]

#### The server will try to prepend and append xml document to our query, a basic query will be like:

```<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE foo [ <!ENTITY sendtime "xyzt"><!ENTITY nowtime "xyzt"> ]><req><stime>&sendtime;</stime><ntime>&nowtime;</ntime><search>cool</search></req>```

![[Pasted image 20210718144143.png]]

#### And this is the code for comparing the sendtime with the nowtime to determine if we have tampered the time or not
#### The server also take the search text to put into a safe query which prevent us from abusing error-based XXE or SQL Injection

![[Pasted image 20210718144351.png]]

## So now im pretty sure this is a XXE attack. But how??

#### After going around on Google i found this Github issue https://github.com/tylere/pykml/issues/37 which tell me that the fromstring was definitely a dangerous function cause it didn't parse the XML well

#### So i try to inject XXE into the template request
![[Pasted image 20210718145140.png]]

### AND HEY, IT WORK (I tried dumping the xml document after parsing it at local)
![[Pasted image 20210718144919.png]]

#### This is basically a blind XXE attack. So then i do what a normal person would do: Try to connect to the my server

![[Pasted image 20210718145215.png]]

#### Surely, that didn't work =))
#### The lxml module automatically block all outbound connections by default (:pain:)
![[Pasted image 20210718150501.png]]
#### Then i come up with a new idea, the only thing we can abuse was the feedback of the server 
## Pay attention to this code, the server try to turn the *sendtime* and *nowtime* we sent to it into 2 integers and them compare them to return a particular result. What if we can turn the flag into a integer and then compare it with our number? Then it would be easy to develop a binary search algorithm to find out the number
![[Pasted image 20210718145830.png]]

#### So, i inserted comments into the XML document to overwrite the nowtime entity, the flag then will be convert into a integer.
#### The *psycho* variable is a random number of my choice 
![[Pasted image 20210718145729.png]]

#### Then the algorithm part was easy 
![[Pasted image 20210718150035.png]]

#### Result: 
![[Pasted image 20210718150124.png]]

### After a few minutes, here is what we got:
![[Pasted image 20210718150521.png]]

### Then to decode it, you can reverse the algorithm, here i sent it to my local server. After parsing the XML, the flag appeared:
![[Pasted image 20210718150731.png]]

![[Pasted image 20210718150806.png]]

### Because of some problems in timing, the flag parsing is not completely successed
### So i guessed it =))
## Here is the flag: `flag{xxe_1s_k1nd4_co0l_als0_ins3rt_m1s5ing_br4ck3ts}`

## Thanks for reading, sorry for my English =))
## Cre: psycholog1st