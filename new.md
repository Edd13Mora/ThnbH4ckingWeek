# ms17-010
# Eternalblue

hello ! 
so i was scrolling on facebook and i sow a post talcking about th3 Hacking Week so i joind the Discord .
i joind the meeting with Th3 Hacker News B'darija Discord members .
i undrstood that i should work on a hacking project and write a write-up
about it . i wanna mention that im a beginner i got in cybersecurity at January 2022.
i got in this challeng while im in (LBLAD)so i have access anly to my kali machin and my sister laptop , so i tried to hack it.![Screenshot_2022-07-25_08-10-33](https://user-images.githubusercontent.com/109980731/180873833-3fd98582-01c0-4330-9749-b73471767e1a.png)
so as nmap scan shows m target running windows7 as OS , and port 445 is open witch means that its running smb (Server Message Block) so i thought about a famous vulnrability  immediately witch is eternalblue AKA ms17-010
now i need to make sure that the target is vulnrable so i checked with metasploit ![Screenshot_2022-07-25_08-14-49](https://user-images.githubusercontent.com/109980731/180874845-a27aa6c4-72eb-4452-a978-716970a39ceb.png)
as metasploit scan showed the target is vulnrable to eternal blue 
i tried to use metasploit to exploit the vulnrability  but it didnt work i dont exactely  way so i looked for a exploit i found serval in exploit-db but no one of them worked 
i searched on github and i found an exploit hear is the link : https://github.com/3ndG4me/AutoBlue-MS17-010
it did work , 
but the shell i've got is wierd so many powershell commands doesnt worck , i started googling and i found that my shell is not stable so i need to stabilize it i found those commands :

-------------------------------------------------

$ python -c "import pty; pty.spawn('/bin/bash')"
$ ruby -e "exec '/bin/bash'"
$ perl -e "exec '/bin/bash';"

$ Ctrl+Z

$ stty raw -echo && fg

$ export TERM=xterm-256-color
-------------------------------------------------


and i've got my stibilized shell 
as i mentioned brfore im still a beginner so im suck at Privilege escalation so i'll stop here ...

