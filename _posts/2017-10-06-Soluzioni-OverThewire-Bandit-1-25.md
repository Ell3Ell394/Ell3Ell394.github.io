---
layout: post
title: "Soluzioni OverThewire Bandit 1-25"
date: 2017-10-06
---
Bandit - 
[http://overthewire.org/wargames/bandit/](http://overthewire.org/wargames/bandit/ "Bandit ")

## Livello 0
Ci colleghiamo come ci viene detto a  
`ssh bandit0@bandit.labs.overthewire.org -p 2220`  
inseriamo la password **bandit0**

## Bandit Level 0 → Level 1
Ci viene detto che la password è all'interno di un file chiamato **readme**.
```
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme 
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

## Bandit Level 1 → Level 2  
`ssh bandit1@bandit.labs.overthewire.org -p 2220`  
Abbiamo un file denominato "-", per poterlo leggere possiamo fare cosi:

```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```
o anche
```
bandit1@bandit:~$ cat < -
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```


## Bandit Level 2 → Level 3

`ssh bandit2@bandit.labs.overthewire.org -p 2220`   
In questo livello abbiamo il nome del file che presenta degli spazi, per poterlo leggere possiamo fare cosi:

```
cat "spaces in this filename"
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

## Bandit Level 3 → Level 4

`ssh bandit3@bandit.labs.overthewire.org -p 2220` 

In questo livello dobbiamo fare in modo di visualizzare un file nascosto; utilizzeremo **ls** insieme all'opzione **-a**.

```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden
bandit3@bandit:~/inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

## Bandit Level 4 → Level 5


`ssh bandit4@bandit.labs.overthewire.org -p 2220` 

In questo livello dobbiamo trovare la password che è all'interno dell'unico file "human-readable" della cartella.
Usiamo il comando **file** che serve per stabilire di che tipo è un file.


```
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data

bandit4@bandit:~/inhere$ cat ./-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```


## Bandit Level 5 → Level 6


`ssh bandit5@bandit.labs.overthewire.org -p 2220`

In questo livello la password è all'interno di uno dei tanti file presenti tra le varie directory; ci vengono fornite delle proprietà che possono aiutarci nella ricerca se utilizzate insieme al comando **find**.

```
bandit5@bandit:~$ ls
inhere
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere04  maybehere08  maybehere12  maybehere16
maybehere01  maybehere05  maybehere09  maybehere13  maybehere17
maybehere02  maybehere06  maybehere10  maybehere14  maybehere18
maybehere03  maybehere07  maybehere11  maybehere15  maybehere19

bandit5@bandit:~/inhere$ find -type f -size 1033c
./maybehere07/.file2

bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

## Bandit Level 6 → Level 7

`ssh bandit6@bandit.labs.overthewire.org -p 2220`	

In questo livello ci viene detto che il file è da "qualche parte nel server" e ha "n" proprietà; per poter fare una ricerca all'interno dell'intero sistema con il comando **find** dobbiamo utilizzare il prefisso **/**, diciamo cosi al comando find di ricercare l'elemento all'interno di tutte le directory presenti nel sistema, partendo da quella pricipale.  
Se proviamo a lanciare il seguente comando:  
`find / -user bandit7 -group bandit6 -size 33c`  
Notiamo che abbiamo in output una notevole quantità di risultati inutili per la nostra ricerca, per pulire l'output possiamo aggiungere al comando appena utilizzato **2>/dev/null**: stiamo in pratica dicendo di ridirigere i messaggi di errore ottenuti in output verso **/dev/null**.

```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null  
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```


## Bandit Level 7 → Level 8
`ssh bandit7@bandit.labs.overthewire.org -p 2220`   
In questo livello ci viene detto che la password è vicina alla parola **millionth** all'interno del file **data.txt**, utilizziamo **grep** per effettuare la ricerca della parola all'interno del file.
```
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ grep 'millionth' data.txt
millionth	cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

## Bandit Level 8 → Level 9

`ssh bandit8@bandit.labs.overthewire.org -p 2220` 

In questo livello dobbiamo trovare la linea di testo presente in **data.txt** che si ripete solamente una volta.
Combiniamo il comando **sort** e **uniq** per ottenere la password.

**uniq** fa un check delle linee presenti in un file o input e rimuove le linee ripetute, dato che il confronto viene effettuato solo tra linee adiacenti è necessario usare uniq insieme al comando **sort**, cosi che il file prima venga ordinato.

Abbiamo due modi per ottenere la password:

Ordinare il file e stampare la riga presente solo una volta  
```
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```


Ordinare il file, fare un conteggio delle righe presenti e stampare quella presente una volta
```
bandit8@bandit:~$ sort data.txt | uniq --count | grep "1 "
      1 UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

## Bandit Level 9 → Level 10

`ssh bandit9@bandit.labs.overthewire.org -p 2220`


In questo livello dobbiamo fare in modo di recuperare dal file **data.txt** una delle poche parole **human readable** che iniziano con diversi **==**.  
Utilizziamo **strings** per recuperare i soli caratteri stampabili e lo concateniamo a **grep** per avere in output le sole parole che iniziano con =.
```
bandit9@bandit:~$ strings data.txt | grep ==
J========== the
========== password
========== is
W========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

## Bandit Level 10 → Level 11

`ssh bandit10@bandit.labs.overthewire.org -p 2220`	

In questo livello dobbiamo decifrare una stringa cifrata in **base64**.
```
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt 
VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
bandit10@bandit:~$ echo VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg== | base64 --decode
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
bandit10@bandit:~$ 
```
oppure 
```
bandit10@bandit:~$ base64 -d data.txt 
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```
## Bandit Level 11 → Level 12

`ssh bandit11@bandit.labs.overthewire.org -p 2220`	

In questo livello abbiamo del testo in **data.txt** dove ogni lettera è stata sostituita con quella posta 13 posizioni più avanti nell'alfabeto[[ROT13](https://it.wikipedia.org/wiki/ROT13)].  
Utilizziamo il comando **tr** per shiftare la posizione dei caratteri.

```
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ tr A-Za-z N-ZA-Mn-za-m < data.text
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```


## Bandit Level 12 → Level 13

`ssh bandit12@bandit.labs.overthewire.org -p 2220`	

Abbiamo un file data.txt contenente un dump esadecimale, compresso più volte.
Ci viene consigliato di creare una cartella temporanea dove poi procedere con il livello.
```
bandit12@bandit:~$ mkdir /tmp/ell3ell394
bandit12@bandit:~$ cp data.txt /tmp/ell3ell394
bandit12@bandit:~$ cd /tmp/ell3ell394
```

Per ripristinare il dump e farlo tornare alla forma originale

`bandit12@bandit:/tmp/ell3ell394$ xxd -r data.txt data.txt`

Risulta però ancora illeggibile, da qui in avanti utilizzeremo i comandi **find**, **gzip** e **bzip2** per arrivare alla password.
```
bandit12@bandit:/tmp/ell3ell394$ file data
data: gzip compressed data, was "data2.bin", from Unix, last modified: Thu Sep 28 14:04:06 2017, max compression

bandit12@bandit:/tmp/ell3ell394$ mv data1.txt data2.bin.gz
bandit12@bandit:/tmp/ell3ell394$ gzip -d data2.bin.gz

bandit12@bandit:/tmp/ell3ell394$ file data2.bin
data2: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/ell3ell394$ bzip2 -d data2.bin
bzip2: Can't guess original name for data2.bin -- using data2.bin.out

bandit12@bandit:/tmp/ell3ell394$ file data2.bin.out data
data2.bin.out: gzip compressed data, was "data4.bin", from Unix, last modified: Thu Sep 28 14:04:06 2017, max compression

bandit12@bandit:/tmp/ell3ell394$  mv data2.bin.out data4.bin.gz
bandit12@bandit:/tmp/ell3ell394$ gzip -d data4.bin.gz

bandit12@bandit:/tmp/ell3ell394$ file data4.bin 
data4.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/ell3ell394$ tar -xf data4.bin
bandit12@bandit:/tmp/ell3ell394$ ls 
data.txt  data4.bin  data5.bin

bandit12@bandit:/tmp/ell3ell394$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/ell3ell394$ tar -xf data5.bin

bandit12@bandit:/tmp/ell3ell394$ ls
data.txt  data4.bin  data5.bin  data6.bin

bandit12@bandit:/tmp/ell3ell394$ file data6.bin 
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/ell3ell394$ bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out

bandit12@bandit:/tmp/ell3ell394$ file data6.bin.out 
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/ell3ell394$ tar -xf data6.bin.out

bandit12@bandit:/tmp/ell3ell394$ ls
data.txt  data4.bin  data5.bin  data6.bin.out  data8.bin

bandit12@bandit:/tmp/ell3ell394$ file data8.bin 
data8.bin: gzip compressed data, was "data9.bin", from Unix, last modified: Thu Sep 28 14:04:06 2017, max compression
bandit12@bandit:/tmp/ell3ell394$ mv data8.bin data9.bin.gz
bandit12@bandit:/tmp/ell3ell394$ gzip -d data9.bin.gz

bandit12@bandit:/tmp/ell3ell394$ ls
data.txt  data4.bin  data5.bin  data6.bin.out  data9.bin
bandit12@bandit:/tmp/ell3ell394$ file data9.bin 
data9.bin: ASCII text
bandit12@bandit:/tmp/ell3ell394$ cat data9.bin 
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```

## Bandit Level 13 → Level 14
`ssh bandit13@bandit.labs.overthewire.org -p 2220`	

Ci colleghiamo al livello 14 attraverso la chiave privata che ci viene fornita  
`ssh -i sshkey.private bandit14@localhost`  
Una volta connessi troviamo la password al percorso **/etc/bandit_pass/bandit14**.
```
cat /etc/bandit_pass/bandit14
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

## Bandit Level 14 → Level 15

`ssh bandit14@bandit.labs.overthewire.org -p 2220`  
Per questo livello utilizziamo **netcat**.  
Ci colleghiamo alla porta 30000 di localhost e inviamo la password del livello corrente.
```
bandit14@bandit:~$ nc localhost 30000 
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr
```

## Bandit Level 15 → Level 16

`ssh bandit15@bandit.labs.overthewire.org -p 2220`

Dobbiamo connetterci alla porta 30001 usando **SSL**  
`openssl s_client -connect localhost:30001`  
se inseriamo la password abbiamo in output **HEARTBEATING** e **Read R BLOCK** come ci era stato segnalato nella descrizione del livello; usiamo l'opzione **-ign_eof** come ci viene suggerito per non far presentare il problema.

Leggendo la manpage di **s_client** l'errore si presentava perchè la password che andiamo a inserire inizia con la lettera B.

```
openssl s_client -ign_eof -connect localhost:30001
....
....
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd
```

##  Bandit Level 16 → Level 17

`ssh bandit16@bandit.labs.overthewire.org -p 2220`	

Iniziamo facendo una scansione delle porte nel range che ci viene detto per vedere quali sono aperte e i servizi che sono attivi.

`bandit16@bandit:~$ nmap 127.0.0.1 -p 31000-32000`
```
Starting Nmap 6.40 ( http://nmap.org ) at 2017-09-17 10:14 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00053s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```
testiamo quali porte accettino **SSL** con `echo hello | nc localhost PORT`, la porta **31518**  e la **31790** ritornano un errore ssl.  
Se proviamo a connetterci alla porta 31518 con `openssl s_client -connect localhost:31518` vediamo che ci viene ritornato quello che scriviamo.

Testando invece la porta **31790** con `openssl s_client -connect localhost:31790` e inviando la password `cluFn7wTiGryunymYOu4RcffSxQluehd` otteniamo in risposta una chiave privata RSA.

Salvo la chiave  
```
cat > sshkey.private
incollo la chiave
ctrl + D per salvare e chiudere
```
se provo a connettermi al prossimo livello usando la chiave
`ssh -i sshkey.private bandit17@localhost`	
 ottengo questo messaggio:

```
Permissions 0664 for 'sshkey.private' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```
cambio i permessi in modo che solo l'user attuale possa leggere e scrivere il file `chmod 600 sshkey.private` e riprovo a connettermi: `ssh -i sshkey.private bandit17@localhost`

## Bandit Level 17 → Level 18

Una volta che siamo in bandit 17 sappiamo che la password è la differenza di parole tra i due file presenti nella homedirectory.
```
diff passwords.new passwords.old
42c42
< kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd				
---
> SRjASnaoZMUCa8kgGMfGCD08SGfQA3xN
```
La riga sopra è stata modifica con la riga sotto quindi la password è **kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd**

## Bandit Level 18 → Level 19

Se proviamo a connetterci utilizzando li solito comando 
`ssh bandit18@bandit.labs.overthewire.org -p 2220 `
otteniamo in output **Byebye!**; qualcuno come ci viene detto ha modificato il file **.bashrc**. 	

Per superare il livello sapendo che la password è all'interno di un file chiamato **readme** nella homedirectory possiamo usare il comando **cat** insieme a **ssh**
`ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme`  
oppure possiamo richiamare una shell differente e usare il comando **-t** per creare uno pseudo terminale `ssh -t bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh` e una volta connessi usare il comando **cat**.

**IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x**

## Bandit Level 19 → Level 20

`ssh bandit19@bandit.labs.overthewire.org -p 2220`  
Per superare questo livello dobbiamo usare il file **setuid** presente nella homedirectory.
```
bandit19@bandit:~$ ls
bandit20-do

bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id

bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```
## Bandit Level 20 → Level 21

`ssh bandit20@bandit.labs.overthewire.org -p 2220`  
Sappiamo che c'è un file setuid nella homedirectory.
```
bandit20@bandit:~$ ls
suconnect
```
suconnect crea una connessione con la porta da noi specificata e poi effettua un confronto tra linee di testo per verificare che la password inviata corisponda a quella del livello precedente.

Mettiamo **netcat** in ascolto passando la password del livello precedente, utilizziamo **&** per mettere il comando in background.   
`echo "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | nc -l 1337 &`   
sullo stesso terminale poi usiamo suconnect con la porta 1337
```
bandit20@bandit:~$ ./suconnect 1337
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
[1]+  Done                    echo "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | nc -l 1337
```

## Bandit Level 21 → Level 22

`ssh bandit21@bandit.labs.overthewire.org -p 2220`  
Abbiamo un comando che viene ripetuto a intervalli regolari grazie a **cron**.  
Ci spostiamo nella cartella **/etc/cron.d** per vedere quale comando viene eseguito.
```
bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls
cron-apt  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  php5
```
Andiamo a vedere cosa contiene cronjob_bandit22
```
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
e facciamo lo stesso anche con lo script cronjob_bandit22.sh 
```
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
vediamo che la password del livello 22 viene copiata nel percorso **/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv**
```
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```
## Bandit Level 22 → Level 23

`ssh bandit22@bandit.labs.overthewire.org -p 2220`

Ci spostiamo nella directory **/etc/cron.d**
```
cd /etc/cron.d

cat cronjob_bandit23
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null

cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget

```
dallo script vedo che la password dell'utente corrente viene copiata in una cartella **mytarget**; se eseguiamo lo script **/usr/bin/cronjob_bandit23.sh** però whoami riporta l'user "bandit22", a noi interessa il livello 23.  
Possiamo allora sostituire la variable **$myname** con bandit23 e avremo la cartella che ci interessa con la relativa password.
```
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349

cat /tmp/8ca319486bfbbc3663ea0fbe81326349
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```

## Bandit Level 23 → Level 24

`ssh bandit23@bandit.labs.overthewire.org -p 2220`

Facciamo gli stessi passaggi del livello precedente
```
cd /etc/cron.d
cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
cat /usr/bin/cronjob_bandit24.sh
```
```
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
	echo "Handling $i"
	timeout -s 9 60 ./$i
	rm -f ./$i
    fi
done
```
Lo script **cronjob_bandit24.sh** esegue tutti i programmi presenti in **/var/spool/$myname**

creiamo una cartella
```
mkdir /tmp/elle
cd /tmp/elle
```
scriviamo lo script che va a copiare la password per il livello 24 in un file a nostro piacimento
```
nano bandit24.sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 >> /tmp/elle/level24.txt
```
settiamo i permessi dello script
```
chmod 777 bandit24.sh
```
copiamo lo script nella cartella dove vengono eseguiti tutti i programmi
```
cp bandit24.sh /var/spool/bandit24/
```
setto i permessi della nostra cartella
```
chmod 777 /tmp/elle
```
aspettiamo qualche attimo
```
ls
bandit24.sh  level24.txt
cat level24.txt
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```
## Bandit Level 24 → Level 25
`ssh bandit24@bandit.labs.overthewire.org -p 2220`

Abbiamo un demone in ascolto sulla porta 30002 che una volta ricevuta la password attuale più la giusta combinazione di 4 numeri restituirà la password per il livello successivo.  
Creo una cartella
```
mkdir /tmp/elle
cd /tmp/elle
```
creiamo un dizionario con tutte le possibili combinazioni di pin
```
nano generaprobe.sh

for i in {0000..9999}
do
  echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i" >> ./out
done
```
settiamo i permessi per poter eseguire lo script e lo eseguiamo
```
chmod 700 generaprobe.sh
./generaprobe.sh
```
creiamo una connessione con **netcat** sulla porta 30002 passando il dizionario creato in precedenza e usiamo **grep** per non avere in output tutti i tentativi errati sapendo che il messaggio di errore è il seguente: "Wrong! Please enter the correct pincode. Try again."
```
cat out | nc localhost 30002 | grep -v "Wrong!"
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
Exiting.
```


## Bandit Level 25 → Level 26

`ssh bandit25@bandit.labs.overthewire.org -p 2220`

La descrizione del livello 25 dice:
```
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
```

Come faccio a vedere quale shell sta usando?
```
cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext

cat /usr/bin/showtext
#!/bin/sh

more ~/text.txt
exit 0
```
usando il comando **ls** nella homedirectory vedo che abbiamo a disposizione una chiave privata 
```
ls
bandit26.sshkey
```
Se provo a connettermi a **bandit26** con il comando 
`ssh -i bandit26.sshkey bandit26@localhost -p2220`
la connessione viene chiusa.

Dobbiamo fare in modo di "attivare" il comando **more** quando proviamo a connetterci, riduco la finestra cosi che il testo non venga stampato a schermo completamente quando effettuiamo la connessione, una volta collegati digitiamo **v** per far partire un editor e digitiamo **:r /etc/bandit_pass/bandit26** ottenedno la password a schermo 5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z.