# **NahamCon CTF 2020**

<p align="center">
  <img width=170 src="assets//images//logo.png"
</p>

This is my writeup for the challenges in NahamCon CTF, I mainly focused on cryptography, steganography and OSINT.
***
# Table of Contents

* [Warmup](#warmup)
  - [Read The Rules](#read-the-rules)
  - [CLIsay](#clisay)
  - [Metameme](#metameme)
  - [Mr.robot](#mr-robot)
  - [UGGC](#uggc)
  - [Easy Keesy](#easy-keesy)
  - [Peter Rabbit](#peter-rabbit)
  - [Pang](#pang)
* [OSINT](#osint)
  - [Time Keeper](#time-keeper)
  - [New Years Resolution](#new-years-resolution)
  - [Finsta](#finsta)
  - [Tron](#tron)
* [Steganography](#steganography)
  - [Ksteg](#ksteg)
  - [Doh](#doh)
  - [Beep Boop](#beep-boop)
  - [Snowflake](#snowflake)
  - [My Apologies](#my-apologies)
  - [Dead Swap](#dead-swap)
  - [Walkman](#walkman)
  - [Old School](#old-school)
* [Cryptography](#cryptography)
  - [Docxor](#docxor)
  - [Homecooked](#homecooked)
  - [Twinning](#twinning)
  - [Ooo-la-la](#ooo-la-la)
  - [Unvreakable Vase](#unvreakable-vase)
  - [December](#december)
  - [Raspberry](#raspberry)
* [Forensics](#forensics)
  - [Microsooft](#microsooft)
  - [Cow Pie](#cow-pie)
* [Mobile](#mobile)
  - [Candroid](#candroid)
  - [Simple App](#simple-app)
  - [Ends Meet](#ends-meet)
* [Miscellaneous](#miscellaneous)
  - [Vortex](#vortex)
  - [Fake file](#fake-file)
  - [Alkatraz](#alkatraz)
  - [Trapped](#trapped)
  - [Awkward](#awkward)
* [Scripting](#scripting)
  - [Dina](#dina)
  - [Rotten](#rotten)
  - [Really powerful Gnomes](#really-powerful-gnomes)
* [Web](#web)
  - [Agent 95](#agent-95)
  - [Localghost](#localghost)
  - [Phphonebook](#phphonebook)
***
# Warmup

## Read The Rules
Please follow the rules for this CTF!

Connect here:
https://ctf.nahamcon.com/rules

**flag{we_hope_you_enjoy_the_game}**

**Solution:** The flag is commented close to the end of the source code for the rules pages, right after the elements for the prizes:

![](assets//images//read_the_rules.png)

## CLIsay
cowsay is hiding something from us!

Download the file below.

[clisay](assets//files/clisay)

**flag{Y0u_c4n_r3Ad_M1nd5}**

**Solution:**  With the challenge we are given an ELF file (a type of Unix executable), by running it we get:

![](assets//images//clisay_1.png)

well that didn't give us much, we can check if there are printable strings in the file by using the strings command on it, doing that gives us the flag:

![](assets//images//clisay_2.png)

notice that you need to append the two parts of the flag together (the strings after and before the ascii art).

**Resources:**
* strings man page: https://linux.die.net/man/1/strings
* ELF file: https://en.wikipedia.org/wiki/Executable_and_Linkable_Format

## Metameme
Hacker memes. So meta.

Download the file below.

[hackermeme.jpg](assets//images//hackermeme.jpg)

**flag{N0t_7h3_4cTuaL_Cr3At0r}**

**Solution:** With the challenge we get this image:

![hackermeme.jpg](assets//images//hackermeme.jpg)

We can guess by the name of the challenge and its description that there is something in the metadata of the image, so we can use exiftool on it, exiftool allows you to see the metadata of an image, and by using it we get the flag:

![](assets//images//metameme.png)

**Resources:**
* Exif: https://en.wikipedia.org/wiki/Exif
* exiftool: https://linux.die.net/man/1/exiftool

## Mr. Robot
Elliot needs your help. You know what to do.

Connect here:\
http://jh2i.com:50032

**flag{welcome_to_robots.txt}**

**Solution:** With the challenge we get a url to a website:

![](assets//images//mr_robot.png)

There doesn't seem to be much in the index page, but we can guess by the name of the challenge that there is something in the robots.txt file for the website, robots.txt is a file which helps search engines (crawlers in general) to index the site correctly, in most sites nowadays there is a robots.txt file, if we look at the file ( the link is http://jh2i.com:50032/robots.txt ) we get the flag:

![](assets//images//mr_robot_2.png)

**Resources:**
* Introduction to robots.txt: https://support.google.com/webmasters/answer/6062608?hl=en

## UGGC
Become the admin!

Connect here:\
http://jh2i.com:50018

**flag{H4cK_aLL_7H3_C0okI3s}**

**Solution:** With the challenge we get a url to a website and it seems that we can login to the it using the index page:

![](assets//images//uggc.png)

By the description we know that we need to login as admin, but if we try using admin as our username we get the following:

![](assets//images//uggc_2.png)

But we can login with any other username:

![](assets//images//uggc_3.png)

If we try to refresh the page or open it in another tab it seems that the login is saved, which means that the site is using cookies, because HTTP connection is stateless (doesn't save the state of the connection server-side) and because sometimes the server needs to know who is the user in a session it saves cookies on the computer of the user, cookies are data which is most of the time encrypted and sent with HTTP requests to helps the server recognize the user, we can see the cookies of the site by using the inspector tool in the browser:

![](assets//images//uggc_4.png)

we can see that the cookie for the site bares a strange similarity to the username I used, that is because the cookie is encrypted using ceaser cipher, a type of substitution cipher where each letter is replaced by the letter with a specific offset from it, in our case with the offset of 13, so a becomes n, b becomes o and so on, a ceaser cipher with offset of 13 is also called a ROT13 cipher, now that we know the cipher used on the cookie we can change our cookie to being that of the admin, we can use cyberchef to do that:

![](assets//images//uggc_5.png)

now we only need to change the value of the cookie to the ciphertext corresponding to admin (we can use the browser inspector tool for that) and we get the flag:

![](assets//images//uggc_6.png)

**Resources:**
* HTTP cookie: https://en.wikipedia.org/wiki/HTTP_cookie
* Ceaser cipher: https://en.wikipedia.org/wiki/Caesar_cipher
* Cyberchef: https://gchq.github.io/CyberChef/


## Easy Keesy
Dang it, not again...

Download the file below.

[easy_keesy](assets//files//easy_keesy)

**flag{jtr_found_the_keys_to_kingdom}**

**Solution:** With the challenge we get a file with an unknown format, we can use the file command to see that the file is a KeePass database:

![](assets//images//easy_keesy.png)

This type of files are databases used to keep passwords on the computer 'safely', there are many password managers to view this kind of files but I used KeeWeb for this challenge mostly because it is a web tool, if we try to open the file with it we can quickly notice that we don't have the password for doing that, furthermore there aren't any mentions of a password in the file or in the description of the challenge, so it seems we need to bruteforce for the password.\
Passwords are commonly saved as hashes, hashes are data created using cryptographic hash functions which are one way functions (easy to find an hash for a password, hard to find a password for the hash) who are also able to return a value with a fixed length to any file with any size, a simple example for an hash function is the algorithm shown in the December challenge with the slight modification that only the last block of the cipher is returned, hashes are great because it is easy to validate a value using them as you can just as hash the value using the hash function and compare the hashes, but, it is hard to get the value from an hash.\
In the case of a KeePass database file, the password for the database, which is called a master password, is saved as an hash in the file in order for a password manager to verify it, this is not a smart idea to save the password locally like that but it's good for us.\
To find the password I used a dictionary attack, this type of attack uses a known database in order to find the right data, in the case of password cracking we use a database of passwords, preferably ordered by most frequently used to least frequently used, we will hash each password and compare it to the hash we have until we'll find a password with the same one, this does not guarantee that we found the correct password (an hash collision can occur) but most probably it will find the correct one, the dictionary I used is called rockyou.txt which lists common passwords. for executing the attack I used John the Ripper, a great tool for cracking hashes using a dictionary, I first converted the file to something john can use and then used john with rockyou.txt to crack the password by executing the following commands:

```bash
keepass2john easy_keesy > kp
john --wordlist=/usr/share/wordlists/rockyou.txt -format:keepass kp
```
by doing that we get that the password for the file is monkeys, if we try using it in KeeWeb we are given access to the database and we get the flag:

![](assets//images//easy_keesy_1.png)

**Resources:**
* file man page: https://linux.die.net/man/1/file
* KeePass: https://en.wikipedia.org/wiki/KeePass
* KeeWeb: https://keeweb.info/
* rockyou.txt: https://wiki.skullsecurity.org/Passwords
* John the Ripper: https://tools.kali.org/password-attacks/john
* cryptographic hash function (CHF): https://en.wikipedia.org/wiki/Cryptographic_hash_function



## Peter Rabbit
Little Peter Rabbit had a fly upon his nose, and he flipped it and he flapped it and it flew away!

Download the file below.\
[peter.png](assets//images//peter.png)

**Post CTF Writeup**

**flag{ohhhpietwastherabbit}**

**Solution:** With the challenge we are given the following PNG image:

![](assets//images//peter.png)

this is actually an esoteric programming language called piet, named after the artist Piet Mondrian, we can use an interpreter to execute the script (I linked the one I used in the resources), by doing so we get the flag:

![](assets//images//peter_2.png)


**Resources:**
* Piet: https://www.dangermouse.net/esoteric/piet.html
* Esoteric Programming Language: https://en.wikipedia.org/wiki/Esoteric_programming_language
* Piet online interpreter: https://www.bertnase.de/npiet/npiet-execute.php



## Pang
This file does not open!

Download the file below.

[pang](assets//files//pang)

**flag{wham_bam_thank_you_for_the_flag_maam}**

**Solution:** With the challenge we get a unknown file, we can use the file command to see that this is a PNG image, but it seems we can't open the image in an image viewer, so we can guess that the image is corrupted, we can verify that by using a tool called pngcheck:

![](assets//images//pang_1.png)

The tool tells us that there is an CRC error in the IHDR chunk, the IHDR is the first chunk in a PNG image and the CRC value is a value stored for every chunk in the image to verify the authenticity of the data (I explained more about CRC and IHDR in my writeup for the challenges in RACTF 2020 listed in the resources).\
We can fix the image by changing value of the CRC, I prefer to do it using an hex viewer so we can have a clear understanding of the data, the changes are marked in red:

![](assets//images//pang_2.png)
![](assets//images//pang_3.png)

and by saving the modified file and viewing it again we get the flag:

![](assets//images//pang.png)

**Resources:**
* pngcheck man page: https://man.cx/pngcheck(1)
* PNG file format specification: http://www.libpng.org/pub/png/spec/1.2/PNG-Contents.html
* RACTF 2020 writeup for stego challenges: https://github.com/W3rni0/RACTF_2020#steg--forensics
* HxD: https://mh-nexus.de/en/hxd/

***
# OSINT

## Time Keeper
There is some interesting stuff on this website. Or at least, I thought there was...

Connect here:\
https://apporima.com/

**JCTF{the_wayback_machine}**

**Solution:** We are given a url of a site with the challenge, as the challenge suggests we need to look at older versions of the site, the current version is:

![](assets//images//time_keeper.png)

we can use a site called Wayback Machine (linked in resources) to view older versions of sites, it seems that there is only one older version of the site from the 18th of april, and there is a snapshot of the index page:

![](assets//images//time_keeper_2.png)

link to the snapshot:
`https://web.archive.org/web/20200418214642/https://apporima.com/`

You can see that the first blog post from the older version can't be found in the current version, furthermore it suggests that the flag is in the web server of the site under /flag.txt, trying to view the file in the current version gives us 404 error, but if we try to view older version of it in the the wayback machine we get the flag:

![](assets//images//time_keeper_3.png)

**Resources:**
* Wayback Machine: https://archive.org/web/

## New Years Resolution
This year, I resolve to not use old and deprecated nameserver technologies!

Connect here: jh2i.com

**flag{next_year_i_wont_use_spf}**

**Solution:** We can infer from the name of the challenge and the description that it has something to do with nameservers, nameserver are servers which handle resolving human-readable identifiers to numberical identifiers, in the case of web server, nameserver handle providing responses to queries on domain names, usually converting urls to IP addresses but not always, we can view this responses using the dig command, in our case we want to view all the type of responses availiable (the more the merrier), we can do this by writing ANY after the command, the full command is:

`dig jh2i.com ANY`

and we have our flag in the output of the command:

![](assets//images//new_year_resolution.png)

**Resources:**
* Name server: https://en.wikipedia.org/wiki/Name_server
* DNS protocol: https://tools.ietf.org/html/rfc1034
* dig man tool: https://linux.die.net/man/1/dig

## Finsta

This time we have a username. Can you track down `NahamConTron`?

**flag{i_feel_like_that_was_too_easy}**

**Solution:** In this challenge we need to track down a username, luckily there is a tool called Sherlock that does just that, it searches popular sites such as GitHub, Twitter, Instagram and etc. for an account with the given username, and returns a list to the profiles, we can run it using the following command:

`python3 sherlock NahamConTron`

and the commands returns the following list of accounts:

```
https://www.github.com/NahamConTron
https://www.instagram.com/NahamConTron
https://www.liveleak.com/c/NahamConTron
https://www.meetme.com/NahamConTron
https://forum.redsun.tf/members/?username=NahamConTron
https://www.twitter.com/NahamConTron
Total Websites Username Detected On : 6
```

by looking at the instegram account we can find our flag at the accout description:

![](assets//images//finsta.png)

**Resources:**
* Sherlock: https://github.com/sherlock-project/sherlock


## Tron

NahamConTron is up to more shenanigans. Find his server.

**flag{nahamcontron_is_on_the_grid}**

**Solution:** Taking a look back at the list Sherlock returned in the previous challenge we can see that there is an account in github with this username, let's take a look at it:

![](assets//images//tron_1.png)

there are 2 repositories for the user:

![](assets//images//tron_2.png)

the second one is not very helpful:

![](assets//images//tron_3.png)

but the first one has some interesting files:

![](assets//images//tron_4.png)

the first file to pop into view is the .bash_history file, it contains the command history of a user and can reveal sensitive information about the user activity, in our case it contains the following line:
```bash
ssh -i config/id_rsa nahamcontron@jh2i.com -p 50033
```
so we now know the user has connected to a server using the SSH protocol (Secure Shell protocol) with an SSH private key, and we also know that the key is in a config folder .... interesting, maybe it is the same folder as the one in the repo?

![](assets//images//tron_5.png)

yeah it is!, the private key is:

```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
NhAAAAAwEAAQAAAYEAxHTNmVG6NLapytFkSDvLytH6aiE5GJRgkCV3mdxr3vLv+jSVs/73
WtCDuHLn56nTrQK4q5EL0hxPLN68ftJmIoUdSvv2xbd8Jq/mw69lnTmqbJSK0gc6MTghMm
3m3FvOoc/Unap6y5CkeqtY844yHsgeXqjVgOaUDsUqMjFAP+SIoQ+3o3aZEweUT4WarHG9
a487W1vxIXz7SZW6TsRPsROWGh3KTWE01zYkHMeO0vHcVBKXVOX+j6+VkydkXnwgc1k6BX
UTh9MOHxAxMK1nV6uC6JQijmUdW9q9YpMF/1VJRVwmzfdZTMTdrGFa7jJl+TxTAiViiBSn
o+IAWdB0Bo5QEoWy+/zzBlpBE9IdBldpH7gj7aKV6ORsD2pJHhbenszS+jp8g8bg8xCwKm
Jm8xNRN5wbdCJXAga5M5ujdXJgihnWtVlodRaZS2ukE+6NWcPx6JdKUpFodLtwO8bBaPFv
mjW9J7hW44TEjcfU2fNNZweL3h+/02TxqxHqRcP/AAAFgNfG1XLXxtVyAAAAB3NzaC1yc2
EAAAGBAMR0zZlRujS2qcrRZEg7y8rR+mohORiUYJAld5nca97y7/o0lbP+91rQg7hy5+ep
060CuKuRC9IcTyzevH7SZiKFHUr79sW3fCav5sOvZZ05qmyUitIHOjE4ITJt5txbzqHP1J
2qesuQpHqrWPOOMh7IHl6o1YDmlA7FKjIxQD/kiKEPt6N2mRMHlE+FmqxxvWuPO1tb8SF8
+0mVuk7ET7ETlhodyk1hNNc2JBzHjtLx3FQSl1Tl/o+vlZMnZF58IHNZOgV1E4fTDh8QMT
CtZ1erguiUIo5lHVvavWKTBf9VSUVcJs33WUzE3axhWu4yZfk8UwIlYogUp6PiAFnQdAaO
UBKFsvv88wZaQRPSHQZXaR+4I+2ilejkbA9qSR4W3p7M0vo6fIPG4PMQsCpiZvMTUTecG3
QiVwIGuTObo3VyYIoZ1rVZaHUWmUtrpBPujVnD8eiXSlKRaHS7cDvGwWjxb5o1vSe4VuOE
xI3H1NnzTWcHi94fv9Nk8asR6kXD/wAAAAMBAAEAAAGANjG+keAAzQ/i0QdocaDFPEMmoG
Zf2M79wGYFk1VCELPVzaD59ziLxeqlm5lfLgIkWaLZjMKrjx+uG8OqHhYuhLFR/mB5l9th
DU8TCsJ09qV0xRVJIl1KCU/hoIa+2+UboHmzvnbL/yH8rbZdCHseim1MK3LJyxBQoa50UH
pTrgx+QGgUkaxi1+QMXs+Ndqq9xVEy36YCY+mVbJw4VAhFr6SmkLfNGgGJ0SCnX6URWlHM
JQkn5Ay6Z6rZSUnhn0sAMNhgBzFGhY3VhpeP5jPYBIbtJUgZ51vDlCQoCBYqXQXOCuLQMB
Efy1uKW+aH0e0Gh07NZyy5AyxHWEtq/zWUJpDrXsmdqbyOW/WX/lAusGkSNj1TPGRcqUl1
4CPJugXgMWWuUuQoRChtKFObCCl7CpjdUdvbKyWDy+Uie/xGZ+dOrU/u4WrwZkkqGKvA6g
SAd6v/RxAdVhaL0xjnPXCgM8e4p9B7EuW3Jy9d15eaGtNp9fpY+SpH4KbHoRom9tXxAAAA
wC2p2qsvXEbiriXaX0WdGa6OYcbr9z5DnG6Kkpwf3K0fb4sm3qvcCrt7owHwiSB1Uy1hng
hLUmUlEgMvVzO0gi/YFCatryIeT9oyQP4wUOLLSSUc4KYg9KuX5crS1Qfo2crAPhkm1n+l
LdiqjAYUB8kL+vU9EuHt0mUA6yrWaVAl4zNP3DOlpB54/v/0yKBEPyHBalU/jv2++NlTRa
FsmU7PV8GD0YuvuHJAVfpnBb8/u4ugpBXciQOS/s734h087QAAAMEA6k6WMSNAmM6SAI2X
5HqwHa19V2AvUUIS0pKbx8Gx3htKq4kHi4Q+tYYAdPFInFO5yauD3/Iv95PakOpiBwTXb1
KK7pzgayc/1ZUN/gHbOgY8WghRY4mnxUg1jQWprlv+Zpk/Il6BdW5db/PmcdQ47yf9IxBA
zcBSCECB1KKFXGUuM3hLowyY77IxQZkZo3VHkkoKhbewQVA6iZacfBlXmEPo9yBNznPG2G
KsjrIILz2ax44dJNeB2AJOvI8i+3vXAAAAwQDWpRmP9vLaVrm1oA8ZQPjITUQjO3duRux2
K16lOPlYzW2mCGCKCd4/dmdpowYCG7ly9oLIZR+QKL8TaNo5zw/H6jHdj/nP//AoEAIFmQ
S+4fBN5i0cfWxscqo7LDJg0zbGtdNp8SXUQ/aGFuRuG85SBw4XRtZm4SKe/rlJuOVl/L+i
DZiW4iU285oReJLTSn62415qOytcbp7LJVxGe7PPWQ4OcYiefDmnftsjEuMFAE9pcwTI9C
xTSB/z4XAJNBkAAAAKam9obkB4cHMxNQE=
-----END OPENSSH PRIVATE KEY-----
```

and we can connect to the server, using the same command, and in the server we find our flag:

![](assets//images//tron_6.png)

***
# Steganography

## Ksteg
This must be a typo.... it was kust one letter away!

Download the file below.

[luke.jpg](assets//images//luke.jpg)

**flag{yeast_bit_steganography_oops_another_typo}**

**Solution:** With the challenge we get the following JPEG image:


<p align="center">
  <img src="assets//images//luke.jpg">
</p>

We can infer by the challenge name and the challenge description that we need to use Jsteg (link in the resources), this is a type of tool for hiding data in the least significant bit (LSB) of the bytes in the image, this image is actually an image of the creator of the tool (whose name is luke), I only succeeded in using the tool by running the main.go script that's in jsteg/cmd/jsteg using the following command:

![](assets//images//luke_1.png)

**Resources:**
* Jsteg: https://github.com/lukechampine/jsteg

## Doh
Doh! Stupid steganography...

**Note, this flag is not in the usual format.**

Download the file below.

[doh.jpg](assets//images//doh.jpg)

**JCTF{an_annoyed_grunt}**

**Solution:** With the challenge we get the following JPEG image:

<p align="center">
  <img src="assets//images//doh.jpg">
</p>

because this is a stego challenge one of the first thing I do is to check if there are files embedded in the image using binwalk and steghide, luckily steghide comes to use and finds a text file in the image which actually contains the flag:

![](assets//images//doh_1.png)

**Resources:**
* Steghide: http://steghide.sourceforge.net/
* binwalk man page: https://manpages.debian.org/stretch/binwalk/binwalk.1.en.html


## Beep Boop
That must be a really long phone number... right?

Download the file below.

[flag.wav](assets//files//flag.wav)

**flag{do_you_speak_the_beep_boop}**

**Solution:** Now we are given for a change a WAV file (Wave audio file), in it we can hear key presses of a phone, this is actually DTMF (dual tone multi frequency) which were used to signal to the phone company that a specific key was pressed and they have quite a lot of history with respect to hacking, we can actually decipher this tones using a tool called multimon-ng or using the web tool listed below, this will give us the following code:

```
46327402297754110981468069185383422945309689772058551073955248013949155635325

```
we can execute the following command to extract the number:

`multimon-ng -t wav -a DTMF flag.wav | grep -o "[0-9]+" | tr -d "\n"`

I tried a lot of ways to get the flag from this number and eventually figured out that you need to convert the numbers from decimal format to hex and then from hex to ascii, or alternatively use long_to_bytes from the pycryptodome module, by doing so we get the flag:

![](assets//images//beep_boop.png)

**Resources:**
* DTMF: https://en.wikipedia.org/wiki/Dual-tone_multi-frequency_signaling
* multimon-ng: https://tools.kali.org/wireless-attacks/multimon-ng
* Web DTMF decoder: http://dialabc.com/sound/detect/index.html
* long_to_bytes: https://pycryptodome.readthedocs.io/en/latest/src/util/util.html#Crypto.Util.number.long_to_bytes

## Snowflake
Frosty the Snowman is just made up of a lot of snowflakes. Which is the right one?

Note, this flag is not in the usual format.

Download the file below.\
[frostythesnowman.txt](assets//files//frostythesnowman.txt)

**JCTF{gemmy_spinning_snowflake}**

**Solution:** We are given a text file with the challenge, if we look at the file we can't see anything weird:
```
Frosty the snowman was a jolly happy soul
With a corncob pipe and a button nose
And two eyes made out of coal
Frosty the snowman is a fairy tale, they say
He was made of snow but the children know
How he came to life one day
There must have been some magic in
That old silk hat they found
For when they placed it on his head
He began to dance around
Oh, Frosty the snowman

```

but if we open the file in Notepad++ and turn on the option the show special symbols we can now see that something is off with the file:

![](assets//images//snowman_1.png)

these are tabs and spaces, and this type of steganography is actually SNOW (Steganographic Nature Of Whitespace), it is a type of whitespace steganography which uses Huffman encoding to compress a message and hide it in the whitespaces, we can use stegsnow tools to reveal the message but it seems that it doesn't work:

![](assets//images//snowman_2.png)

After a bit of trial and error I discovered that it is password protected, so I wrote a simple bash script which reads the passwords from rockyou.txt line by line and try to decrypt the data, this is a dictionary attack, and a simple one at that (I explained more about this type of attacks in the writeup for Easy Keesy):

```bash
file=rockyou.txt
while read -r line
do
        printf "\n$line "
        stegsnow -C -Q -p "$line" frostythesnowman.txt
done < $file
```

by using this simple bruteforce script we get that the password is ilovejohn (don't we all) and we get the flag (I redirected the script output to a file and then grepped for braces pattern):

![](assets//images//snowman_3.png)

**Resources:**
* SNOW: http://www.darkside.com.au/snow/
* stegsnow man page: http://manpages.ubuntu.com/manpages/bionic/man1/stegsnow.1.html

## My Apologies
Nothing witty to say here... just that I am sorry.

Note, this flag is not in the usual format.

Download the file below.\
[apologies.txt](assets//files//apologies.txt)

**flag_i_am_so_sorry_steg_sucks**

**Solution:** We again get a txt file with the challenge, now we can easily notice that something off with the message:

```
Tuｒｎs out the sｔｅganｏgrａｐhⅰc tｅcｈｎｉｑuｅ ｗe wｅｒｅ ｕsiｎg dⅰｄｎ'ｔ rｅalｌy mａｋe mｕcｈ ｓense.．. ｂｕｔ we ｋｅpｔ it aｎyｗａy． Oh well!
```
This is actually an Homoglyphs Steganography, a type of steganography which uses unicode encoding to hide a message, we can use the link in the resources to reveal the flag:

![](assets//images//apologies.png)

**Resources:**
 * Twitter Secret Messages: http://holloway.co.nz/steg/

## Dead Swap
There is a flag in my swap!

Download the file below.\
[deadswap](assets//files//deadswap)

**Post CTF Writeup**

**flag{what_are_you_doing_in_my_swap}**

**Solution:** With the challenge we are given a file from an unknown type, with can infer from the challenge title and description that this is a swap file, without getting into details, swap files are files saved in the hard drives to be used as an extension to the memory, when a computer needs to save some data on the memory for quick access and doesnt have a place for it the computer moves a chunk of the data stored on the memory to the hard drive (usually the least used chunk) and overwrites this chunk in the memory with the data, this is actually really not important for this challenge, by using xxd on the file it seems that we only have \xff bytes:

![](assets//images//deadswap_1.png)

but by grepping for everything but \xff bytes we can see thet there are \xfe bytes too:

![](assets//images//deadswap_2.png)

during the CTF I tried using binary by mapping 1 and 0 to f and e but the solution is actually to map 1 and 0 to fe and ff, this will give us a binary string, encoding the string to ascii we get the flag in reverse, so I wrote a one-liner to do all that (and it's amazing):

`xxd deadswap | grep -v "ffff ffff ffff ffff ffff ffff ffff ffff" | cut -d " " -f 2-10 | sed "s/ff/0/g" | sed "s/fe/1/g" | tr -d " \n" | python3 -c "import binascii; print(binascii.unhexlify('%x' % int(input(),2)));" | rev`

the one-liner prints the hexdump of the file, greps for lines which contains interesting data, cut only the columns with the hex data, replaces ff and fe with 0 and 1 (using sed), removes new-lines, convert the data from binary to ascii using python and reverses the string, and in action:

![](assets//images//deadswap_3.png)

**Resources:**
* Swap file: https://www.computerhope.com/jargon/s/swapfile.htm

## Walkman
Do you hear the flag? Maybe if you walk through it one step at a time.

Download the file below.\
[wazaa.wav](assets//files//wazaa.wav)

**Post CTF Writeup**

**flag{do_that_bit_again}**

**Solution:** We are given a WAV audio file with the challenge, I personally hate steganography that is related to audio, if it is not spectogram and wavsteg can't find something I just quit...but I'm a completionist, so I'll cover that as well, we need to use a tool called wav-steg-py which is listed in the resources using this command to extract the flag:

`python3 wav-steg.py -r -s wazaaa.wav -o a -n 1 -b 1000`

in action:

![](assets//images//walkman.png)

this tool uses the least significant bit to hide data and extract hidden data (which wavsteg also do so i'm not sure why it didn't work with it), it's quite common to hide data in the LSB of the file so this type of tools are really handy.

**Resources:**
* wav-steg-py: https://github.com/pavanchhatpar/wav-steg-py

## Old School
Did Dade Murphy do this?

Note, this flag is not in the usual format

Download the file below.\
[hackers.bmp](assets//images//hackers.bmp)

**Post CTF Writeup**

**JCTF{at_least_the_movie_is_older_than_this_software}**

**Solution:** With the challenge we are given a bitmap (bmp) file:

![](assets//images//hackers.bmp)

bmp format is a quite old file format and rarely used today as its compression algorithm is really not good and rarely supported so it's not very efficient in space to save images as bmp files, especially if you consider the image quality nowadays, the flag is again hidden in the least significant bits of the image and again I tried checking that during the CTF and got nothing, a smarter approach is to use zsteg, which checks all the available channels and even checks the most significant bit for hidden data, we can get the flag using the following command:

`zsteg -a hackers.bmp`

and in action:

![](assets//images//oldschool.png)

***
# Cryptography

## Docxor
My friend gave me a copy of his homework the other day... I think he encrypted it or something?? My computer won't open it, but he said the password is only four characters long...

Download the file below.\
[homework](assets//files//homework)

**flag{xor_is_not_for_security}**

**Solution:** We get an unknown file with the challenge, obviously from the challenge description and challenge name we know that the file is xored and that the key is of length 4, if we look at the hex dump of the file we can notice this reoccurring pattern of bytes `\x5a\x41\x99\xbb` :

![](assets//images/docxor_1.png)

furthermore if we analyze the frequency of the bytes in the file we get the following graph where the peaks are in \x5a, \x41, \x99 and \xbb:

![](assets//images/docxor_2.png)

but if we look at a regular PNG file or Zip file we get the following bytes frequency:

![](assets//images/docxor_4.png)

we can notice that regularly the \x00 byte is the most frequent, so if the key is xorred with the data all of the \x00 bytes will be mapped to the bytes of the key.

so we can infer that key is `\x5a\x41\x99\xbb`, plugging the file into cyberchef in xorring the data with the key gives us the following zip file:

[xorred_homework](assets//files//xorred_homework)

this is actually not a zip file but a docx file by the folders in it (there are a lot of file types which are actually zip) if we open the file using Microsoft word or Libreoffice we get the flag:

![](assets//images/docxor_3.png)

**Resources:**
* Xor: https://en.wikipedia.org/wiki/Exclusive_or
* Frequency analysis: https://en.wikipedia.org/wiki/Frequency_analysis
* An example to some of the file types which are actually zip: https://www.quora.com/Which-file-types-are-really-ZIP-or-other-compressed-packages


## Homecooked
I cannot get this to decrypt!

Download the file below.\
[decrypt.py](assets//files//decrypt.py)

**flag{pR1m3s_4re_co0ler_Wh3n_pal1nDr0miC}**

**Solution:** Now we get with the challenge a python script:
```python 3
import base64
num = 0
count = 0
cipher_b64 = b"MTAwLDExMSwxMDAsOTYsMTEyLDIxLDIwOSwxNjYsMjE2LDE0MCwzMzAsMzE4LDMyMSw3MDIyMSw3MDQxNCw3MDU0NCw3MTQxNCw3MTgxMCw3MjIxMSw3MjgyNyw3MzAwMCw3MzMxOSw3MzcyMiw3NDA4OCw3NDY0Myw3NTU0MiwxMDAyOTAzLDEwMDgwOTQsMTAyMjA4OSwxMDI4MTA0LDEwMzUzMzcsMTA0MzQ0OCwxMDU1NTg3LDEwNjI1NDEsMTA2NTcxNSwxMDc0NzQ5LDEwODI4NDQsMTA4NTY5NiwxMDkyOTY2LDEwOTQwMDA="

def a(num):
    if (num > 1):
        for i in range(2,num):
            if (num % i) == 0:
                return False
                break
        return True
    else:
        return False

def b(num):
    my_str = str(num)
    rev_str = reversed(my_str)
    if list(my_str) == list(rev_str):
       return True
    else:
       return False


cipher = base64.b64decode(cipher_b64).decode().split(",")

while(count < len(cipher)):
    if (a(num)):
        if (b(num)):
            print(chr(int(cipher[count]) ^ num), end='', flush=True)
            count += 1
            if (count == 13):
                num = 50000
            if (count == 26):
                num = 500000
    else:
        pass
    num+=1

print()
```

this script is used for decrypting the cipher but it doesn't seem to work well:

![](assets//files//homecooked.gif)

it somewhat stops printing at this point but still runs, we can guess by that the code is inefficient, we can try to understand what the script does to figure out how to make it more efficient, we can see that the script decode the ciphertext from base64 to bytes, then for each byte in the ciphertext it tries to find a value of next value for num such that both functions a and b returns a boolean value of True, then xors that value with the value of the byte and prints the result, it continues likes that for the succeeding bytes while continuously increasing the value of num by one, but, by the 13th byte the value of num is jumped to 50000 and by the 26th byte the value of num is jumped to 500000.

Now let's look at the functions, a checks if there are no numbers bigger than 2 and smaller than the input that can divide it without a remainder, so a checks if the input is prime.
The function b checks if the input is equal to itself in reverse so b checks if the input is a palindrome.
a return True if the number is prime and b checks if the number is a palindrome, so the values that are xorred with the bytes of the cipher are palindromic primes

if we take a second look at the function a we can see that it is very inefficient as it checks for all the numbers that are smaller than the input if they can divide it without a remainder, we can replace it with the primality test in the sympy module, which uses an efficient method (Rabin-Miller Strong Pseudoprime Test), in the end we get the less obfuscated following script:

```python 3
import base64
import sympy

num = 0
count = 0
cipher_b64 = b"MTAwLDExMSwxMDAsOTYsMTEyLDIxLDIwOSwxNjYsMjE2LDE0MCwzMzAsMzE4LDMyMSw3MDIyMSw3MDQxNCw3MDU0NCw3MTQxNCw3MTgxMCw3MjIxMSw3MjgyNyw3MzAwMCw3MzMxOSw3MzcyMiw3NDA4OCw3NDY0Myw3NTU0MiwxMDAyOTAzLDEwMDgwOTQsMTAyMjA4OSwxMDI4MTA0LDEwMzUzMzcsMTA0MzQ0OCwxMDU1NTg3LDEwNjI1NDEsMTA2NTcxNSwxMDc0NzQ5LDEwODI4NDQsMTA4NTY5NiwxMDkyOTY2LDEwOTQwMDA="

def prime(num):
    return sympy.isprime(num)

def palindrome(num):
    my_str = str(num)
    rev_str = reversed(my_str)
    if list(my_str) == list(rev_str):
       return True
    else:
       return False


cipher = base64.b64decode(cipher_b64).decode().split(",")

while(count < len(cipher)):
    if (prime(num)):
        if (palindrome(num)):
            print(chr(int(cipher[count]) ^ num), end='', flush=True)
            count += 1
            if (count == 13):
                num = 50000
            if (count == 26):
                num = 500000
    else:
        pass
    num+=1

print()
```
and by running this more efficient script we get the flag in a reasonable time:

![](assets//files//homecooked_2.gif)


## Twinning
These numbers wore the same shirt! LOL, #TWINNING!

Connect with:\
`nc jh2i.com 50013`

**flag{thats_the_twinning_pin_to_win}**

**Solution:** When we connect to server given we the challenge we are greeted with the following:

![](assets//images//twinning_1.png)

we can guess that this is an RSA encryption, I explained more about how RSA works in my writeup for RACTF 2020:

> ... RSA is a public key cipher, which means that there are two keys, one that is public which is used to encrypt data, and one that is private which is used to decrypt data, obviously there is some sort of connection between the keys but it is hard to reveal the private key from the public keys (and in this case vice versa), specifically in RSA in order to find the private key we need to solve the integer factorization problem, which is thought to be in NP/P (this is not important for the challenge), we will call our public key e and our private key d, they posses the following attribute - d multiply by e modulo the value of (p-1) * (q-1) which we will name from now phi, is equal to 1, we will call d the modular multiplicative inverse of e and e the modular multiplicative inverse of d, furthermore if we take a plaintext message pt and raise it to the power of d and then to the power of e modulo the value of p * q, which we will name n and will be commonly given to us instead of q and p, we will get pt again (to understand why it is needed to delve into modern algebra, if n is smaller than pt then obviously we will not get pt), now with that in mind we can talk about the cipher, encryption in this cipher is raising the plaintext pt to the power of the public key e mod the value of n, similarly, decryption is raising the ciphertext to the power of d mod n...

and I explained why it works and how we can break the cipher:

>...for that we need to talk about factors, factors are numbers which can be divided only by 1 and himself (we are only talking about whole numbers), we have discovered that there are infinitely many factors and that we can represent any number as the multiplication of factors, but, we haven't discovered an efficient way to find out which factors make up a number, and some will even argue that there isn't an efficient way to do that (P vs. NP and all that), which means that if we take a big number, it will take days, months and even years to find out the factors which makes it, but, we have discovered efficient ways to find factors, so if I find 2 factors, which are favorably big, I multiply them and post the result on my feed to the public, it will take a lot of time for people to discover the factors that make up my number. But, and a big but, if they have a database of numbers and the factors that make them up they can easily find the factors for each numbers I will post, and as I explained before, if we can the factors we can easily calculate phi and consequently calculate d, the private key of RSA, and break the cipher, right now there are databases (listed below) with have the factors to all the numbers up to 60 digits (if I remember correctly), which is a lot but not enough to break modern RSA encryptions, but if we look at the challenge's parameters, we can see that n is awefully small, small enough that it most be in some databases...

if we search for the value of n in factorDB, a database for the factors of numbers, we can find factors for the value of n given to us:

![](assets//images//twinning_2.png)

now we can write a small script which calculates phi, finds d the modular inverse for e modulo phi and raise the ciphertext to the power of d (or be a script kiddie and use the RSA module):

```python 3
from Crypto.Util.number import inverse


p = 1222229
q = 1222231
e = 65537
ct = 348041806368
n = 1493846172899

phi = (p - 1) * (q - 1)
d = inverse(e,phi)
plain = pow(ct,d,n)
print(plain)

```
and by running this script we get that the PIN is 3274 and by giving the PIN to the server we get the flag:

![](assets//images//twinning_3.png)

I guess that the challenge name and description is joking about the proximity of the primes...

## Ooo-la-la
Uwu, wow! Those numbers are fine!

Download the file below.\
[prompt.txt](assets//files//ooolala.txt)

**flag{ooo_la_la_those_are_sexy_primes}**

**Solution:** With the challenge we are given a text file, the text file contains the following:

```
N = 3349683240683303752040100187123245076775802838668125325785318315004398778586538866210198083573169673444543518654385038484177110828274648967185831623610409867689938609495858551308025785883804091
e = 65537
c = 87760575554266991015431110922576261532159376718765701749513766666239189012106797683148334771446801021047078003121816710825033894805743112580942399985961509685534309879621205633997976721084983
```

So this is another RSA challenge, we can again try to find the factors that make up the value of N, we can use factorDB again:

![](assets//images//ooolala_1.png)

and we have the factors, now let's recycle the script from the last challenge now with the new parameters,also now we need to convert the plaintext to ascii encoded characters, we can use the function long_to_bytes from pycryptodome for that:

```python 3
from Crypto.Util.number import inverse, long_to_bytes


p = 1830213987675567884451892843232991595746198390911664175679946063194531096037459873211879206428207
q = 1830213987675567884451892843232991595746198390911664175679946063194531096037459873211879206428213
e = 65537
ct = 87760575554266991015431110922576261532159376718765701749513766666239189012106797683148334771446801021047078003121816710825033894805743112580942399985961509685534309879621205633997976721084983
n = 3349683240683303752040100187123245076775802838668125325785318315004398778586538866210198083573169673444543518654385038484177110828274648967185831623610409867689938609495858551308025785883804091

phi = (p - 1) * (q - 1)
d = inverse(e,phi)
plain = pow(ct,d,n)
print(long_to_bytes(plain))
```
and by running the script we get the flag:

![](assets//images//ooolala_2.png)

## Unvreakable Vase
Ah shoot, I dropped this data and now it's all squished and flat. Can you make any sense of this?

Download the file below.\
[prompt.txt](assets//files//vase.txt)

**Post CTF Writeup**

**flag{does_this_even_count_as_cryptooo}**

**Solution:** We this challenge we are given a text file with the following content:

```
zmxhz3tkb2vzx3roaxnfzxzlbl9jb3vudf9hc19jcnlwdg9vb30=
```

this seems to be base64 encoding, but if you try to decode it from base64 to ascii you don't get much:
```
ÎlaÏ{dokóÇzèk.ßÏ.ån_co{îuÿas_crypv.oo}
```

I didn't actually managed to solve this challenge by myself during the CTF thinking it is a combination between rail cipher and base64 but actually that is just a base64 encoding where all the upper cased letters were lowered, we can try going over all combination of lower case and upper case for all the characters in the string but it will take two to the power of the length of the string, which is 2 to the power of 52 at most and at least 2 to the power of 40 if we exclude numbers and symbol, which is still a lot.\
But, we can do something else, base64 is using characters to represents the numbers from 0 to 63, if we'll encode one letter from base64 to binary we get a binary string of length 6 bits, but each ascii character take 8 bits to encode, so if we want to find the smallest ascii substring that decodes to a base64 string without padding we'll need to find a lowest common multiple (LCM) value, for those numbers the LCM is 24, and s0 every 24/8 = 3 ascii characters are encoded to 24/6 = 4 base64 characters without padding and if we will split our ciphertext to blocks of 4 characters and try every possible combination of upper case and lower case on every character in each block until we get a readable substring (preferably of the flag which very likely though not guaranteed) we'll need to try at most 2 to the power of 4 multiplied by the number of blocks for every block, in out case `(2 ** 4) * (52 / 4) = (2 ** 4) * 12` which is a lot less then what we had before, for that I wrote the following script which goes through every block in the ciphertext and tries all the possible combinations until the ascii strings decoded from the block are printable (in the range from space \x20 to tilde \x7e):

```python 3
import base64
from string import printable

cipher = list('zmxhz3tkb2vzx3roaxnfzxzlbl9jb3vudf9hc19jcnlwdg9vb30=')

for i in range(0,len(cipher),4):
	for j in range(2 ** 4):
		curr_byte = cipher[i:i+4].copy()
		string_index = int(i/4*3)
		for k in range(len(curr_byte)):
			if j % (2 ** (k + 1)) >= 2 ** k:
				curr_byte[k] = curr_byte[k].upper()
		new_cipher = cipher[:i] + curr_byte + cipher[i+4:]
		max_char = chr(max(base64.b64decode(''.join(new_cipher))[string_index: string_index+3]))
		min_char = chr(min(base64.b64decode(''.join(new_cipher))[string_index: string_index+3]))
		if min_char in printable and max_char in printable:
			cipher[i:i+4] = curr_byte
			break
	print(base64.b64decode(''.join(cipher)))
```

and by running this script we get the flag:

![](assets//images//unvreakable_vase.png)

**Resources:**
* I used this writeup just to discover the cipher although it seems that he solved just it like me with a way better script: https://deut-erium.github.io/WriteUps/nahamconCTF/crypto/Unvreakable%20Vase/
* Least common multiple: https://en.wikipedia.org/wiki/Least_common_multiple

## December
This is my December...

Download the file below.\
[source.py](assets//files//source.py)     [ciphertext](assets//files//ciphertext)


**flag{this_is_all_i_need}**

**Solution:** With the challenge we get the following python 3 script:

```python 3
#!/usr/bin/env python

from Crypto.Cipher import DES

with open('flag.txt', 'rb') as handle:
	flag = handle.read()

padding_size = len(flag) + (8 - ( len(flag) % 8 ))
flag = flag.ljust(padding_size, b'\x00')

with open('key', 'rb') as handle:
	key = handle.read().strip()

iv = "13371337"
des = DES.new(key, DES.MODE_OFB, iv)
ct = des.encrypt(flag)

with open('ciphertext','wb') as handle:
	handle.write(ct)
```

and the ciphertext:

```
Ö¢oåÇ\"àT^N@]XõêiùÔ1÷UWETR^DˆžbÿÑ\*á^VAAVCç¤nÿÌIô]RTLE[ZDÝ£yÉÃ/ÍXl]RTWN7
```

We can see from the script that it uses DES, DES (Data Encryption Standard) is a type of symmetric cipher that was used in the 80s and the 90s as the standard cipher replaced by AES in the following years, it was invented by IBM with the help of the NSA (yeah that NSA) and in the 90s   people have discovered ways to crack the cipher in a matter of hours (22 hours and 15 minutes to be precise).\
This cipher also has a lot of weaknesses, one of those are the existence of weak keys, decryption and encryption with this keys have the same effect and so encrypting some data twice with the same weak key is equivalent to decrypting the encryption and the ciphertext is equal to the original plaintext.

we can also notice that the cipher uses OFB mode of operation, in this mode the plaintext is split to blocks of 8 bytes and for each block of plaintext the mode encrypts the encryption of the previous block (in the case of the first block this mode encrypts IV) and xors the new encryption with the plaintext, in a visual representation:

<p align="center">
  <img src="assets//images//december_1.png" style="background-color:white;" />
</p>

and in a formal representation:

<p align="center">
  <img src="assets//images//december_2.png" style="background-color:white;" />
</p>

we can now notice the following attribute of using weak keys in this mode of operation:

<p align="center">
  <img src="assets//images//december_3.png" style="background-color:white;" />
</p>

in other words, for every block in an even position we get that the encryption with a weak key is equal to xorring IV with the plaintext, so the plaintext for block in an even position is equal to the ciphertext xorred with IV, let's try that on our ciphertext, we can do that using the following code:

```python 3
from Crypto.Util.strxor import strxor

data = open("ciphertext",'rb').read()
IV = "13371337"

print(strxor(data,(IV * len(data))[0:len(data)].encode("utf-8")))
```
and we get:

![](assets//images//december_4.png)

it worked!, now we know that our key is a weak key, we can find a list of weak keys to DES on google and bruteforce them until we get a complete text (there are less than 100 weak and semi-weak keys), I listed all the weak keys in the following file:

[weak DES keys](assets//files//keys)

and wrote this script to crack the ciphertext:

```python 3
#!/usr/bin/env python

from Crypto.Cipher import DES

with open('ciphertext','rb') as handle:
	ct = handle.read()

with open('keys', 'r') as handle:
	keys = handle.read().replace("\n"," ").split()
	keys = [ bytes(bytearray.fromhex(key.strip())) for key in keys]

iv = "13371337"
for key in keys:
	des = DES.new(key, DES.MODE_OFB, iv.encode('utf-8'))
	pt = des.decrypt(ct)
	if b'flag' in pt:
		print(pt)
		print(key)
```

and we get the flag:

![](assets//images//december_5.png)


## Raspberry
Raspberries are so tasty. I have to have more than just one!

Download the file below.\
[prompt.txt](assets//files//raspberry.txt)

**flag{there_are_a_few_extra_berries_in_this_one}**

**Solution:**: With the challenge we are get a text file, the content of the text file is:

```
n = 7735208939848985079680614633581782274371148157293352904905313315409418467322726702848189532721490121708517697848255948254656192793679424796954743649810878292688507385952920229483776389922650388739975072587660866986603080986980359219525111589659191172937047869008331982383695605801970189336227832715706317
e = 65537
c = 5300731709583714451062905238531972160518525080858095184581839366680022995297863013911612079520115435945472004626222058696229239285358638047675780769773922795279074074633888720787195549544835291528116093909456225670152733191556650639553906195856979794273349598903501654956482056938935258794217285615471681
```
This is again an RSA cipher, if we try plugging the value of n to a factor database we get the following output:

![](assets//images//respberry.png)

this is a big amount of factors, this amount is actually okay as RSA is not limited to only 2 factors (but it is really bad practice to use a lot of factors), phi is actually the value of Euler's totient function for n, this value is the number of values smaller than n which don't have common factors with n, and this value is actually equal to multiplication of all the factors reduced by one each (the proof for that is actually very easy and logical), so for decrypting the message I used the following script which is the same as the previous script with a more general phi calculation:

```python 3
from Crypto.Util.number import inverse, long_to_bytes


primes = ['2208664111', '2214452749', '2259012491', '2265830453', '2372942981', '2393757139', '2465499073', '2508863309', '2543358889', '2589229021', '2642723827', '2758626487', '2850808189', '2947867051', '2982067987', '3130932919', '3290718047', '3510442297', '3600488797', '3644712913', '3650456981', '3726115171', '3750978137', '3789130951', '3810149963', '3979951739', '4033877203', '4128271747', '4162800959', '4205130337', '4221911101', '4268160257']

e = 65537
ct = 5300731709583714451062905238531972160518525080858095184581839366680022995297863013911612079520115435945472004626222058696229239285358638047675780769773922795279074074633888720787195549544835291528116093909456225670152733191556650639553906195856979794273349598903501654956482056938935258794217285615471681
n = 7735208939848985079680614633581782274371148157293352904905313315409418467322726702848189532721490121708517697848255948254656192793679424796954743649810878292688507385952920229483776389922650388739975072587660866986603080986980359219525111589659191172937047869008331982383695605801970189336227832715706317

phi = 1
for p in primes:
  phi *= (int(p) - 1)
d = inverse(e,phi)
plain = pow(ct,d,n)
print(long_to_bytes(plain))
```
By running this script we get the flag:

![](assets//images//respberry_1.png)

**Resources:**
* Euler's totient function: https://en.wikipedia.org/wiki/Euler%27s_totient_function

***

# Forensics

## Microsooft
We have to use Microsoft Word at the office!? Oof...

Download the file below.\
[microsooft.docx](assets//files//microsooft.docx)

**flag{oof_is_right_why_gfxdata_though}**

**Solution:** With the challenge we get a docx file, but if we try opening it with Microsoft Word or Libreoffice we get noting interesting:

![](assets//images//microsooft.png)

so we need to inspect the file more, docx files are actually a bunch of xml files contained in a zip file, so if we open the file as a zip file we can look at the content without relying on a document editor:

![](assets//images//microsooft_1.png)

after a brief inspection I found that there is a filed called foo.txt in the src directory in the zip file:

![](assets//images//microsooft_2.png)

and the file contains our flag:

![](assets//images//microsooft_3.png)

## Cow Pie
Ew. Some cow left this for us. It's gross... but something doesn't seem right...

Download the file below.\
[manure](assets//files//manure)

**flag{this_flag_says_mooo_what_say_you}**

**Solution:** run strings on manure and grep for the flag

***

# Mobile

## Candroid
I think I can, I think I can!

Download the file below.\
[candroid.apk](assets//files//candroid.apk)

**flag{4ndr0id_1s_3asy}**

**Solution:** With the challenge we get an apk file, as the previous challenge an apk file is actually a zip file, we can unzip the file and grep for the flag format to get the flag:

![](assets//images//candroid.png)

## Simple App
Here's a simple Android app. Can you get the flag?


Download the file below.\
[candroid.apk](assets//files//simple-app.apk)

**flag{3asY_4ndr0id_r3vers1ng}**

**Solution:** same as previous challenge:

![](assets//images//simple_app.png)

## Ends Meet
Are you a true mobile hacker?

Download the file below.

**flag{rev3rsIng_ApKs_l1k3_A_Pr0}**

**Solution:** open the apk file in jadx-gui and get a base64 encoded url in the `MainActivity` and visit the page with useragent `volley/0`

***
# Miscellaneous

## Vortex
Will you find the flag, or get lost in the vortex?

Connect here:\
`nc jh2i.com 50017`

**flag{more_text_in_the_vortex}**

**Solution:** With the challenge we are given a server to connect to, if we try connecting we get...

![](assets//images//vortex.png)

...that...we can redirect the output of the server to a file and view the file, by doing so for a minute more or less and grepping for the flag format we get the flag:

![](assets//images//vortex_1.png)

## Fake file
Wait... where is the flag?

Connect here:\
`nc jh2i.com 50026`

**flag{we_should_have_been_worried_about_u2k_not_y2k}**

**Solution:** We are given a server to connect to with the challenge, when we connect to the server we seemingly have a shell:

![](assets//images//fake_file.png)

seems that there are two files with the name '..' but using regular command like cat on the file won't work I eventually tried to use `grep -r .` to recursively grep the file in the directory, and we get the flag (or we could just get all the flags like this guy https://tildeho.me/leaking-all-flags-in-a-ctf/):

![](assets//images//fake_file_2.png)

## Alkatraz
We are so restricted here in Alkatraz. Can you help us break out?

Connect here:\
`nc jh2i.com 50024`


**flag{congrats_you_just_escaped_alkatraz}**

**Solution:** We are given again a server to connect to, it seems that we have a shell again and that the flag is in our working directory, but we can't use cat or grep to read it:

![](assets//images//alkatraz.png)

I eventually got to output the file content to stdout using printf as it is not restricted and using the following command `printf '%s' "$(<flag.txt)"` we get the flag:

![](assets//images//alkatraz_2.png)


## Trapped
Help! I'm trapped!

Connect here:\
nc jh2i.com 50019

**flag{you_activated_my_trap_card}**

**Solution:** Trap command is catching every command except trap, set the trap to something else with `trap '<command>' debug`.
like `trap 'cat flag.txt' debug` to get flag
## Awkward
No output..? Awk-o-taco.

Connect here:
`nc jh2i.com 50025`

**flag{okay_well_this_is_even_more_awkward}**

**Solution:** use grep to find cat all files and grep only to flag format

```python 3
import re
from pwn import *
from string import printable
host, port = 'jh2i.com', 50025

s = remote(host,port)

name = "flag{"
while True:
	for c in "_" + printable:
		command = """grep -ro "{}.*"\n """.format(name + c)
		s.send(command)
		response = s.recv()
		return_code = re.findall("[0-9]+",str(response))[0]
		if int(return_code) == 0:
			if c != '*'
				name += c
				print(name)
				break
			else:
				printf(name + "}")
				break
s.close()
```

***
# Scripting

## Dina
Help! I can't make any sense out of what Dina is saying, can you??

Connect with:
`nc jh2i.com 50035`

**Post CTF Writeup**
**flag{dina_speaks_in_dna_and_you_do_too}**

**Disclaimer:** I'll start of with a quick disclaimer, even though I ended solving it using frequency analysis and some common sense, I don't think what I did was the intention of the author, and I used the mapping john showed three or four times to validate a character and escape rabbit holes, though I think it can be done without using it at all if you have  time, and after validating the first few characters I could easily complete the rest myself.\
oh and another thing, the script I wrote is really ugly but it works well, you can use an empty mapping and remove the question types and it will still work, and I strongly recommend trying this yourself.

**Solution:** With the challenge we are given an host and a port to connect to, when we connect the server sends a string which comprises only of A,C,G,T and waits for input, if we give some random input it will respond with another string like the previous:

![](assets//images//dina_1.png)

It seems to be a DNA sequence and if you have basic knowledge in biology you'll know that DNA sequences are interpreted by the ribosomes to proteins, and every sequence of 3 nucleic acids in the DNA or equivalently every 3 letters in the DNA which is called a codon is interpreted to one amino acid in the protein, so we can trying using this type of encoding to get a strings consisting of the letters of each matching amino acid, but it will not work.\
we can next try using binary notations for each nucleic acid and decode the binary to ascii characters but this will also not work, after a google search about DNA encoding to English I stumbled upon this writeup https://github.com/ChapeauR0uge/write-ups/tree/master/b00t2root/crypto/genetics where the author uses a mapping from codons to English in order to decode the string, but the mapping didn't seem to help, at this point in the CTF I stopped and moved on to other challenges.\
After the CTF and during the debrief that john did I discovered that the last thing I tried was kinda right but the mapping was incorrect, so I tried to find the right mapping on the internet, but I couldn't find it, and so I resorted to the last thing I could think of, to use frequency analysis.

In every alphabet there are characters who appear more than others, for example the letter a appears more than z and e appears more than a, also if we look at all the printable symbols we will also see that the space symbol appears more than e or any other characters, we can also look at the frequency of one word or two words and so on and see the same trend.\
We can use that to our advantage, so I started out by finding a list of frequencies for all the printable symbols (https://www.wired.com/2013/08/the-rarity-of-the-ampersand/) and then wrote a small script which connect to the server a number of times (I used 50) and count the occurrences of every codon in the string and in the end lists them in descending order of occurrences (you can see parts of it in the final script), also, in every session the script would try to guess the plaintext using the already mapped codons or the total number of occurrences of the each codons against the list of frequencies of symbols if it was not mapped, I used it to map codons to the space symbol and e, then map to a and t by looking at the ciphertexts and recognizing words (we can easily guess the mapping for a from a one-letter word and the mapping for t and h from a very frequent three letter-word), admittedly I used john script in the debrief to figure out that the first word in every ciphertext, which is a two-letter word without i, n or t, is a number and and colon (I though initially that it was yo or mr), by using letter frequency and word frequency and most importantly common sense I mapped around ten to fifteen codons to characters, but than I hit a roadblock, even though most of the encoded sentence is a readable string like "enter the string" the string in question is a combination of random letters, for example (actually taken from a run):

`send back yirkbmusswnqmhq as a string`

and for that no frequency analysis would have helped, so I turned to what I do best, bruteforcing everything I can.

At this point when I had around ten to fifteen characters mapped so sometimes when the sun and the moon align the random string would be comprised by only mapped letters, and more commonly but still rare enough I'll get that the string comprises only of mapped codons except one, and we can use the response of the server for this kind of strings to eliminate mappings or discover the actual mapping, and so I upped the number of retries of the script would do to 200, and added that in every session the script would try to recognize the question sent by the server and send back the matching string *upper-cased* (yeah that throw me off for a while), I then manually went through the output and tried to eliminate wrong mapping or to see if the script discovered any correct one (actually when I think of it I could have written a script which automatically does that):

```python 3
import re
from pwn import remote
from random import choice

#  The predicted mapping of the codons
mapping =   {"AAA":'y',
             "AAC":'q',
             "AAG":'k',
             "AAT":'',
             "ACA":'q',
             "ACC":'1',
             "ACG":'s',
             "ACT":'0',
             "AGA":'~',
             "AGC":'',
             "AGG":'=',
             "AGT":'j',
             "ATA":' ',
             "ATC":'',
             "ATG":'i',
             "ATT":'',
             "CAA":'',
             "CAC":'',
             "CAG":'',
             "CAT":'',
             "CCA":'b',
             "CCC":'',
             "CCG":'w',
             "CCT":'v',
             "CGA":'a',
             "CGC":'h',
             "CGG":'',
             "CGT":'',
             "CTA":'x',
             "CTC":'',
             "CTG":'u',
             "CTT":'z',
             "GAA":'',
             "GAC":'',
             "GAG":'4',
             "GAT":'.',
             "GCA":'3',
             "GCC":'',
             "GCG":'/',
             "GCT":':',
             "GGA":'o',
             "GGC":'e',
             "GGG":'',
             "GGT":'f',
             "GTA":'',
             "GTC":'',
             "GTG":'p',
             "GTT":'c',
             "TAA":'',
             "TAC":'',
             "TAG":'2',
             "TAT":'',
             "TCA":'r',
             "TCC":'m',
             "TCG":',',
             "TCT":'n',
             "TGA":'',
             "TGC":'l',
             "TGG":'',
             "TGT":'',
             "TTA":'<',
             "TTC":'t',
             "TTG":'d',
             "TTT":'g'}


# The type of questions dina asks and the position of the string in them
question_types = {'send the string': 4,
                  'send the message': 5,
                  'send this back': 4,
                  'go ahead and send': 5,
                  'back to me': 2,
                  'enter this back': 7,
                  'please respond with': 4,
                  'respond with': 3,
                  'enter the string': 4,
                  'please send back': 4,
                  'send back': 3,
                  }


def beautify_data(msg):
      return str(msg)[2:-3].replace("\\n","")

# frequency analysis stuff
frequency = { a: 0 for a in mapping }
letter_frequency = list(' etaoinsrhldcumfgpywb,.vk-\"_\'x)(;0j1q=2:z/*!?$35>\{\}49[]867\\+|&<%@#^`~.,')
for i in range(len(letter_frequency)):
      if letter_frequency[i] in mapping.values():
            letter_frequency[i] = choice(letter_frequency)
letter_frequency = ''.join(letter_frequency)


host, port  = 'jh2i.com', 50035
for _ in range(200):
      s = remote(host,port)
      index = 0
      while 1:

            # Recieves until a message is sent
            ciphertext = ''
            try:
                  while ciphertext == '':
                        ciphertext = beautify_data(s.recv())
            except EOFError:
                  s.close()
                  break
            # Checks if the flag is given
            if 'flag' in ciphertext:
                  print(ciphertext)
                  exit(0)

            # Find the frequency of each codon for frequency analysis
            for i in range(0,len(ciphertext),3):
                  frequency[ciphertext[i:i+3]] += 1
            frequency =  {k:frequency[k] for k in sorted(frequency, key= frequency.get, reverse=True)}

            # The mapping letters from frequency analysis
            frequency_letters = []
            # The whole plaintext
            plaintext = []
            for i in range(0,len(ciphertext),3):
                  # Checks if the mapping for the codon is known, if not predict a letter otherwise uses the mapping
                  if mapping[ciphertext[i:i+3]] == '':
                         plaintext.append(letter_frequency[list(frequency.keys()).index(ciphertext[i:i+3])])
                         frequency_letters.append((ciphertext[i:i+3],letter_frequency[list(frequency.keys()).index(ciphertext[i:i+3])]))
                  else:
                        plaintext.append(mapping[ciphertext[i:i+3]])

            plaintext = ''.join(plaintext)
            if 'nope' in plaintext:
                  break

            print(ciphertext)
            print(plaintext)
            print(str(index) + ": " + str(frequency_letters))

            response = 'random'
            for q in question_types.keys():
                  if q in plaintext:
                        response = plaintext.split(" ")[question_types[q]]
                        break

            print(response)
            s.send(response.upper())
            index += 1

print(frequency)
```
and oh boy did it worked, after a little more than an hour I succeeded in finding out a mapping for every used codon and get the flag:

![](assets//images//dina_2.png)

actually the mapping is not spot-on for numbers but because the string only comprises of  letters this is not that crucial.

***

# Web

## Agent 95
They've given you a number, and taken away your name~

Connect here:\
http://jh2i.com:50000

**flag{user_agents_undercover}**

**Solution:** Change User agent to Windows 95

## Localghost
BooOooOooOOoo! This spooOoOooky client-side cooOoOode sure is scary! What spoOoOoOoky secrets does he have in stooOoOoOore??

Connect here:\
http://jh2i.com:50003

**JCTF{spoooooky_ghosts_in_storage}**

**Solution:** In JavaScript code for jquerty.jscroll2 after beautifying, flag variable contains flag in bytes

## Phphonebook
Ring ring! Need to look up a number? This phonebook has got you covered! But you will only get a flag if it is an emergency!

Connect here:\
http://jh2i.com:50002

**flag{phon3_numb3r_3xtr4ct3d}**

**Solution:** With the challenge we are given a website with the following index page:

![](assets//images//phphonebook_1.png)

so the site tells us that the php source for the page accepts parameters...oooh we might have LFI (local file inclusion), php allows the inclusion of files from the server as parameters in order to extend the functionality of the script or to use code from other files, but if the script is not sanitizing the input as needed (filtering out sensitive files) an attacker can include any arbitrary file on the web server or at least use what's not filtered to his advantage, php scripts stay in the server-side and should not be revealed to the client-side as it may contain sensitive data (and there is no use to it in the client-side), if we want to leak the php files we'll need to encode or encrypt the data as the LFI vulnerable php code will read the file as code and execute it, we can do such things using the php://filter wrapper, using this wrapper in the following way will leak the php source code for index page:

`http://jh2i.com:50002/index.php?file=php://filter/convert.base64-encode/resource=index.php`

and by going to this url we get the file base64 encoded:

![](assets//images//phphonebook_2.png)

```
PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KICA8aGVhZD4KICAgIDxtZXRhIGNoYXJzZXQ9InV0Zi04Ij4KICAgIDx0aXRsZT5QaHBob25lYm9vazwvdGl0bGU+CiAgICA8bGluayBocmVmPSJtYWluLmNzcyIgcmVsPSJzdHlsZXNoZWV0Ij4KICA8L2hlYWQ+CiAgPGJvZHk+Cgk8P3BocAoJCSRmaWxlPSRfR0VUWydmaWxlJ107CgkJaWYoIWlzc2V0KCRmaWxlKSkKCQl7CgkJCWVjaG8gIlNvcnJ5ISBZb3UgYXJlIGluIC9pbmRleC5waHAvP2ZpbGU9IjsKCQl9IGVsc2UKCQl7CgkJCWluY2x1ZGUoc3RyX3JlcGxhY2UoJy5waHAnLCcnLCRfR0VUWydmaWxlJ10pLiIucGhwIik7CgkJCWRpZSgpOwoJCX0KCT8+CgkgIAk8cD5UaGUgcGhvbmVib29rIGlzIGxvY2F0ZWQgYXQgPGNvZGU+cGhwaG9uZWJvb2sucGhwPC9jb2RlPjwvcD4KCjxkaXYgc3R5bGU9InBvc2l0aW9uOmZpeGVkOyBib3R0b206MSU7IGxlZnQ6MSU7Ij4KPGJyPjxicj48YnI+PGJyPgo8Yj4gTk9UIENIQUxMRU5HRSBSRUxBVEVEOjwvYj48YnI+VEhBTksgWU9VIHRvIElOVElHUklUSSBmb3Igc3VwcG9ydGluZyBOYWhhbUNvbiBhbmQgTmFoYW1Db24gQ1RGIQo8cD4KPGltZyB3aWR0aD02MDBweCBzcmM9Imh0dHBzOi8vZDI0d3VxNm85NTFpMmcuY2xvdWRmcm9udC5uZXQvaW1nL2V2ZW50cy9pZC80NTcvNDU3NzQ4MTIxL2Fzc2V0cy9mN2RhMGQ3MThlYjc3YzgzZjVjYjYyMjFhMDZhMmY0NS5pbnRpLnBuZyI+CjwvcD4KPC9kaXY+CgogIDwvYm9keT4KIDwvaHRtbD4=
```
and decoding this from base64 gives us the code:

```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Phphonebook</title>
    <link href="main.css" rel="stylesheet">
  </head>
  <body>
	<?php
		$file=$_GET['file'];
		if(!isset($file))
		{
			echo "Sorry! You are in /index.php/?file=";
		} else
		{
			include(str_replace('.php','',$_GET['file']).".php");
			die();
		}
	?>
	  	<p>The phonebook is located at <code>phphonebook.php</code></p>

<div style="position:fixed; bottom:1%; left:1%;">
<br><br><br><br>
<b> NOT CHALLENGE RELATED:</b><br>THANK YOU to INTIGRITI for supporting NahamCon and NahamCon CTF!
<p>
<img width=600px src="https://d24wuq6o951i2g.cloudfront.net/img/events/id/457/457748121/assets/f7da0d718eb77c83f5cb6221a06a2f45.inti.png">
</p>
</div>

  </body>
</html>
```

we can now tell by this line `include(str_replace('.php','',$_GET['file']).".php");` that we only leak php files as the code removes the php extension from the file given as input (if there is one) and then appends a php extension to it, I think that there are ways to go around this but it is not important for this challenge, doing the same for phphonebook.php gives us this code:

```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Phphonebook</title>
    <link href="main.css" rel="stylesheet">
  </head>

  <body class="bg">
    <h1 id="header"> Welcome to the Phphonebook </h1>

    <div id="im_container">

      <img src="book.jpg" width="50%" height="30%"/>

      <p class="desc">
      This phphonebook was made to look up all sorts of numbers! Have fun...
      </p>

    </div>
<br>
<br>
    <div>
      <form method="POST" action="#">
        <label id="form_label">Enter number: </label>
        <input type="text" name="number">
        <input type="submit" value="Submit">
      </form>
    </div>

    <div id="php_container">
    <?php
      extract($_POST);

    	if (isset($emergency)){
    		echo(file_get_contents("/flag.txt"));
    	}
    ?>
  </div>
  </br>
  </br>
  </br>


<div style="position:fixed; bottom:1%; left:1%;">
<br><br><br><br>
<b> NOT CHALLENGE RELATED:</b><br>THANK YOU to INTIGRITI for supporting NahamCon and NahamCon CTF!
<p>
<img width=600px src="https://d24wuq6o951i2g.cloudfront.net/img/events/id/457/457748121/assets/f7da0d718eb77c83f5cb6221a06a2f45.inti.png">
</p>
</div>

  </body>
</html>
```
by the end of the code there is a php segment where the method extract is used on $\_POST and then there is a check if $emergency is set, if so the code echoes the content of a file called flag.txt.\
this is really interesting, I'll explain the gist of it mainly because I'm not so strong at php, the $_POST is the array of variables passed to the script when an HTTP POST request is sent to it (we commonly use 2 type of HTTP request, POST and GET, where GET asks from the server to return some resource for example the source for a page, and POST also sends variables to a file in the server in the request body), the extract method extracts the variables from the array, the extracted variable name is set to the name passed in the HTTP request and the value is set to the corresponding value, the isset method just checks if there is a variable with this name, by all this we can infer that we need to send a POST request to the server with a variable named emergency which has some arbitrary value in order to get the server to print the flag, we can do so using curl or using a proxy like burp suite like I will show first, we need to set burp suite as our proxy and send a post request to the server, we can do such that using the submit button in the phphonebook page:

![](assets//images//phphonebook_3.png)

burp suite catches the request and allows us to edit it:

![](assets//images//phphonebook_4.png)

we now only need to add an emergency variable and we get the flag:

![](assets//images//phphonebook_5.png)

![](assets//images//phphonebook_6.png)

with curl we can simply use the following command `curl -X POST --data "emergency=1" http://jh2i.com:50002/phphonebook.php` and by using that and grepping for the flag format we can easily get the flag:

![](assets//images//phphonebook_7.png)


**Resources:**
* File Inclusion Vulnerabilities: https://www.offensive-security.com/metasploit-unleashed/file-inclusion-vulnerabilities/
* HTTP request methods: https://www.w3schools.com/tags/ref_httpmethods.asp
* Payload all the things - File Inclusion: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion
