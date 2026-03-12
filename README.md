# DosCrack

## What is it about?
A task about cracking a simple DOS program. I receive a .com file with a program
which check the written password and prints either the 'access granted' string
or the 'access denied' one. The goal is to get the access message.

The programm contains two vulnerabilities which let me get pass the check without
knowing the password. Turbo debagger is used for exploring the code and finding
the vulnerabilities.

## Finding the first one
This is the first thing I see when the program starts:
<img width="1033" height="288" alt="image" src="https://github.com/user-attachments/assets/bf4ef53c-1bd7-40fe-ba5b-5e23f3c6a7be" />

And after trying the most obvious choice...

<img width="1793" height="538" alt="image" src="https://github.com/user-attachments/assets/b81be36d-0406-422f-83e3-8c090fd33665" />

After going through the code in TD, I notice two important things. Firstly, there is obviosly a hash function which used for the password:

<img width="1127" height="519" alt="image" src="https://github.com/user-attachments/assets/e842cfb6-500a-46e5-9656-330c20c66405" />


Secondly, my answer is placed exactly where the password is. This is what I got after typing 'aaaaa' string:

<img width="901" height="142" alt="image" src="https://github.com/user-attachments/assets/0baf0c7c-3322-48fd-b5da-1148849c6f7e" />

So any symbol I put there changes the password. I went to the beginning to check the conditions of finishing the input.
As I expected, the programm needs me to press 'Enter'...

<img width="906" height="150" alt="image" src="https://github.com/user-attachments/assets/8b529959-088f-4650-a864-3743c82e5a90" />

...but the most important thing here is that it won't change the password at all if I just press 'Enter' in the beginning of the
programm. So I did it:

<img width="1185" height="279" alt="image" src="https://github.com/user-attachments/assets/8ca8b160-cdae-4a63-a031-ff5bf8eb8ede" />

The first vulnerability is found!

## Finding the second one

I also notice that the program doesn't control how many symbols I write. That allows me to overflow the buffer.
The hash function takes the info from the space in ds between the password and displayed strings...

<img width="1009" height="306" alt="image" src="https://github.com/user-attachments/assets/123a33dc-9a2f-4cc7-8c68-d72c3e3d616c" />

... and just puts it to ax every cycle.

<img width="992" height="271" alt="image" src="https://github.com/user-attachments/assets/06499051-ca2b-495e-82cb-375356c6777b" />

These are the only changes for ax. So the next thing I tried is to replace all the info after the password with random but similar symbols.
They will be put to ax, and the same ones will be put in bx(I replace not only the hash function random data but the hash itself too).
<img width="936" height="98" alt="image" src="https://github.com/user-attachments/assets/a7a5fbfc-a4a8-401a-a5ba-26f0a4ad5790" />

So the comparation will lead to the access.

<img width="1630" height="715" alt="image" src="https://github.com/user-attachments/assets/1c7f7bbb-9682-4da3-b803-da5cb3c68d5a" />

## In conclusion...

I guess there are more ways to crack the programm, but the idea of buffer overflow is one of the most obvious, so I
was seeking for something that would let me erase the important data or maybe even place my own code inside.




