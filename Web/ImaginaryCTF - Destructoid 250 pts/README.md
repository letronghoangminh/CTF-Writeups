
## This is my writeup of the 250 points challenge 'Destuctoid' in Imaginary CTF 2021. 

### This is the challenge's description

![[Pasted image 20210725162523.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725162523.png)

### There are a youtube link link to the 'We Are Destroyer' song by Anberlin (it was good :D) although we really dont need this hint.

#### Here is the front page of the site
![[Pasted image 20210725162917.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725162917.png)

#### Reverse the text and i got **Can you find my ?source** so i requested to 'https://destructoid.chal.imaginaryctf.org/?source'

#### Here is what i got
![[Pasted image 20210725163320.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725163320.png)

### So basically we must exploit the unserialize vulnerability from the `$_SERVER['HTTP_X_PADYLOAD]'`
#### We can set it with our request header 'X-PAYLOAD'
#### For who doesn't know:
- `__construct` function is automatically called when we create an object
- `__wakeup` function is automatically called when we unserialize a serialized object
- `__toString` function is automatically called when we `echo` an object

#### Look at the code, we see that `$printflag` is set to false and only change to true if the `__toString` function of class Y invoked
![[Pasted image 20210725163657.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725163657.png)

#### To invoke the `__toString` we must `echo` a Y object, here we only see one `echo`
![[Pasted image 20210725163741.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725163741.png)

#### What does that mean? So basically the `echo` will be called when the `__wakeup` is called (mean that a Y object is unserialized)
#### So we can construct our Y class like this:
![[Pasted image 20210725164224.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725164224.png)

#### Then after we serialize this `exploit` object and send to server, when it's unserialized the Y object inside will be echoed and set the $printFlag to true
![[Pasted image 20210725164452.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725164452.png)

#### Cool, so we got in the `__destruct` function of X class (pay attention to the No! with 'o' lowercase)
#### Now return to class X
![[Pasted image 20210725164552.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725164552.png)
#### We see from class Y that a Y object's secret will be passed in to class X as `$cleanup` and create a new X object. If `$cleanup` is 'flag' it will be blocked by the `__construct` function of X class. So we have to reach the `__destruct` function without creating a new X object
### Here is what i did:
![[Pasted image 20210725165005.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725165005.png)

#### So when the inside Y object is echoed, it will create a new X object with the `$cleanup`  equal to a X object with its own `$cleanup` is 'flag' (confusing right? try to imagine it as X inside X)

#### So then we can pass the `__contruct` function and when the `__destruct` of the inside X object is called, we got the flag
![[Pasted image 20210725165252.png]](https://github.com/letronghoangminh/CTF-Writeups/blob/master/Web/ImaginaryCTF%20-%20Destructoid%20250%20pts/Images/Pasted%20image%2020210725165252.png)

### I know it's really confusing, just take time to think and you will understand 

### Star me if you found this useful
## Credit: psycholog1st


