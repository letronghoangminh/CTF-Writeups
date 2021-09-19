## SpiralCI (496 pts - 23 solves) write up

This was a good challenge, it uses a new attack technique called Dependency Confusing Attack.

Web interfaces: 
![image](https://user-images.githubusercontent.com/50044415/133912581-9e01e1c9-9d1e-49f2-b829-d049e3115995.png)

I looked at the cookie and got a jwt:
```
spiralCI=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyaWQiOi0xfQ.pCIT8y2m4E3nC-rpos3BvorxYZqIDpNNdZroYvpzMYI
```
After fuzzing and finding other interfaces to attack, i found nothing so i decided to attack on JWT.

Using jwt.io:
![image](https://user-images.githubusercontent.com/50044415/133912610-cf675816-0e1e-4699-b007-f0672c88cde7.png)

So i tried modify jwt's algorithm to none and userid to 1.

Then i got this:
![image](https://user-images.githubusercontent.com/50044415/133912632-14686032-ce4a-47ad-8ad6-1f049b53a587.png)

Seems like it's an admin account and it's disabled, so i changed userid to 2
```
spiralCI=eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VyaWQiOjJ9.
```
![image](https://user-images.githubusercontent.com/50044415/133912649-1f0d1195-a715-40ee-befd-bfcdeabb7c4d.png)

So it worked, so basically there are two Nodejs packages, and the chall let me run the CI test and view their logs. So i downloaded the zip files.

They are basically two simple Nodejs projects, and there are an important file package.json in 'calculator' package
![image](https://user-images.githubusercontent.com/50044415/133912690-6ecfc641-2304-4ede-a1ac-e3f4c436d65d.png)

Then i tried running the 'calculator' project and view it's log.
![image](https://user-images.githubusercontent.com/50044415/133912697-6f3b0015-d872-4451-b5de-c5d798c9f950.png)

First idea of mine was to overwrite the script in package.json to read the flag. But after finding for a while, i found nothing, i can't modify the project because they are admin's projects.

Admin account was disabled so i can't log in as admin either.

Then i decided to look at other chall and comeback with it later

After starting do this chall again, i found out that the package "calc_ph6avsgeed" (in this write up i will use the same package tho) was used in the last time now has a different name. So i think this package maybe my entry to find the flag.

The logs show that the project used private registry, so i was thinking of a way to takeover the package with npm public module.

And then i found this article: https://digital.nhs.uk/cyber-alerts/2021/cc-3751
And this about how to publish a npm module: https://zellwk.com/blog/publish-to-npm/

![image](https://user-images.githubusercontent.com/50044415/133912753-e24f55f7-298e-4376-a9d8-2423b307522c.png)

So i created a npm package with the name "calc_ph6avsgeed" and version: 1.0.1 and the "preinstall" script to list the file in current folder.

Then publishing it to npm publib registry.

![image](https://user-images.githubusercontent.com/50044415/133912783-0e5f3719-c32a-43e3-88b1-99e7a57b4b9a.png)

I got the following result:

![image](https://user-images.githubusercontent.com/50044415/133912795-625e0289-58b4-4618-aca4-7e66c4ba6fcc.png)

So we got RCE on server

Looks at the path to pwd, after listing for a while, i found the flag is located at /usr/src/app/flag.txt

![image](https://user-images.githubusercontent.com/50044415/133912813-84757510-1b98-431d-bf29-71c167b186bd.png)

And launch the test, then view the log:

![image](https://user-images.githubusercontent.com/50044415/133912843-0b88fc3a-02b4-423d-b66e-a0840326661e.png)


P/s: Dont't forget to give me a star if you find it's usefule xD










