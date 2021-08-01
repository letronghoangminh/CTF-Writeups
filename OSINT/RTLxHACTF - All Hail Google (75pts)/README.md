
# All-Hail-Google
## OSINT Challenge for RTLxHACTF (Written by me in cooperation with Moriarty from Thehackerscrew)

![image](https://user-images.githubusercontent.com/50044415/127777568-788dd583-9f52-4c36-a83c-ae2d31036060.png)
## Imo it deserves more than 75pts, only 8 solves until the end of CTF 

## Start with a geoguesser image

![image](https://user-images.githubusercontent.com/88171165/127584064-dc054070-ddfa-468b-b97d-68f5ffbdd9f0.png)
## Description: A 'wonder' place


## With Google lens or something like that we find out that the place is Wonders Park, Da Nang, Viet Nam (The city which im living at xD)
https://www.google.com/maps/place/C%C3%B4ng+vi%C3%AAn+K%E1%BB%B3+Quan/@16.0988215,108.2222085,17z/data=!4m12!1m6!3m5!1s0x3142198d74e5a281:0x24e8ff134e9f0079!2zQ8O0bmcgdmnDqm4gS-G7syBRdWFu!8m2!3d16.0988164!4d108.2243972!3m4!1s0x3142198d74e5a281:0x24e8ff134e9f0079!8m2!3d16.0988164!4d108.2243972?hl=vi-VN

## Look at the review section, we see this comment with 'newest' filter that looks really suspicious ( I used one of my account to review because google prevent newly created account from spamming review)

![image](https://user-images.githubusercontent.com/88171165/127584221-9286afa8-3906-434a-9fdd-c2ce7a289a95.png)

## Follow the paste bin link we got this drive link
![image](https://user-images.githubusercontent.com/88171165/127584344-e10f5af6-fb5f-42fc-b8d5-971cae3b94f6.png)

## Follow it we get this image 
![image](https://user-images.githubusercontent.com/88171165/127584390-49878eb7-9255-45d8-b0ca-82239ddb213c.png)

## After downloading it and use 'exiftool' we got the email of Alexander
![image](https://user-images.githubusercontent.com/88171165/127584452-5004d27c-77b0-4532-82f5-bf8b5214880d.png)

## Now we got the email, if we mail it, there is an auto reply mail which lead us to Rick Astley song :)
## So now look at the review: `Such a great place, can't wait to visit this park with my wife next year.`
## That looks like an event in calendar
## But if we use Ghunt, we can't see the event (the calendar of alexixthomas is actually public, but somehow i fooled Ghunt =)))
![image](https://user-images.githubusercontent.com/88171165/127584647-593e283f-3f2a-4c88-8078-be7a7b82a6fa.png)

## So if players use Ghunt, they might follow the wrong path.
## Now we go to Google Calendar and add the gmail to search list
![image](https://user-images.githubusercontent.com/88171165/127584719-6522e41f-3e9e-4577-b076-19478c1d851b.png)

## Then scroll the calendar to next year, we see the event
![image](https://user-images.githubusercontent.com/88171165/127584796-c5f46d8d-8ff7-4794-a139-4c8f81a955f5.png)

## Here we got the email of his wife, the username lead to another Twitter account which is just an easter egg
## Then we mail her, we got auto reply

![image](https://user-images.githubusercontent.com/88171165/127584919-249332ad-22ec-4c78-8873-3e9abdae553b.png)

## I hide flag in the email with white text, so they must view the raw mail or hightlight the mail to see it 
![image](https://user-images.githubusercontent.com/88171165/127584976-79fa9f33-64f2-4247-b4e3-b9d676e65b27.png)

## This is the first chall i wrote, so I definitely messed up many things. If you have something for improving it, you can dm me psycholog1st#2222. Thanks for playing and reading =))
## Btw, give me a star if you find this writeup helpful xD

## Cre: psycholog1st

