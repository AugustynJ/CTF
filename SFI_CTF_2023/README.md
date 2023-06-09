# SFI CTF

Capture The Flag Competition organized on Students' IT Festival in Kraków.

## hello_world

You’ve got a message!
The flag is:

**sfi18_ctf{hello_world!}**

Nothing to do.

## cryptic_website

![cryptic_website](./images/cryptic_website.png)

You can jest copy text and paste somewhere. This gives you flag:

**sfi18_ctf{HelloThere!}**

## crawlers

Conctent gives us hint about robots many times:

> “Dear time traveler,
You HAVE TO destroy the time machine! It is dangerous for DeLorean to exist. Don’t try to find me in the past, or future, or any other dimension. As a scientist, I tried to explore time and space, but it only led to dangerous paradoxes. The future is not meant for people just yet. Please, don't let anything stop you. There may be some people or robots who would love to take over this technology. Doc Emmet Brown”
> 
> Try to find what robots Doc was talking about! Maybe they have a message for you that will clarify this further.

We can search in internet:

*A robots.txt file tells search engine crawlers which URLs the crawler can access on your site*

Therefore, we should find file *robots.txt*. It is on [ctf.sfi.pl/robots.txt](https://ctf.sfi.pl/robots.txt)

**sfi18_ctf{LQbvJJc1Ulj8}**

## la_bouche

We are starting with thic pic:

![la_bouche](./images/la_bouche.png)

After fast research we discover [Olivier Levasseur](https://en.wikipedia.org/wiki/Olivier_Levasseur) - pirate called "La Bouche"! And he invented his own cipher. Looks similar?
Decrypting ciphertext gives us: **QUAVOUSSERERLA**, which is obviously a flag.

## vpong_game

You can download file [here](./files/V-Pong.exe)

Looks like a game, but you mustn't run it. Just use one command:
```bash
strings V-Pong.exe | grep sfi18
```
that gives you flag immediately:

![vpong](./images/vpong.png)

**sfi18_ctf{LetMe(W)In}** Simple as that.

## wild_west

We started with cursed map:

![wild_west](./files/map.svg)

As we can see it's kinda adding:
```
OREGON = KANSAS + OHIO
```
It's popular puzzle. Every letter from above has own number:
```
0 = R
1 = G
2 = S
3 = E
4 = K
5 = O
6 = I
7 = N
8 = H
9 = A
```
Very importatnt were hints to this challenge:
> Hints:
>
>The value of every variable is unique.
>
>The flag is uppercase and sorted in ascending order.

Every letter has unique number - done

Ascending order means letters according to numbers in order:

0 1 2 3 4 5 6 7 8 9

R G S E K O I N H A

So the flag is: **sfi18_ctf{RGSEKOINHA}**

## images 

We have two identical images:

![a](./images/a.png)
![b](./images/b.png)

This challenge required advanced steganography tool: **[zsteg](https://github.com/zed-0xff/zsteg)**.

Using this command we get output:
```diff
$ zsteg -a b.png 

imagedata           .. file: Windows Precompiled iNF, version 0.1, InfStyle 1, flags 0x1ff0001, unicoded, at 0x1000100 "", OsLoaderPath "", LanguageID 0, at 0x1000100 InfName ""
-b1,rgba,lsb,xy      .. text: "SHOULD_YOU_SEE_ME?"
b2,r,lsb,xy         .. file: Novell LANalyzer capture file
b2,g,lsb,xy         .. text: "DUUTUTUUQ"
b2,b,lsb,xy         .. file: PEX Binary Archive
b2,a,msb,xy         .. text: ["U" repeated 41 times]
b2,bgr,lsb,xy       .. file: 0421 Alliant compact executable not stripped
b2,rgba,msb,xy      .. text: ["@" repeated 165 times]
```
And this is flag: **sfi18_ctf{SHOULD_YOU_SEE_ME?}**

Not obvious imho.

## post_office

This is page of post office organization. Every button displays a message about errors.

In the sourcepage there was a comment looks like base64 encoded text:
```
aW5kZXgucGhwCmxvZ2luOiBhZG1pbgpwYXNzd29yZDogc2Zpc2Zpc2Zp
```
After decoding we recieve passes to... what?
```
index.php
login: admin
password: sfisfisfi
```
Let's use BurpSuite and add a header to site:
```javascript
login=admin&passwword=sfisfisfi
```
We must to change method from GET to POST (like PoSt OfFiCe).

And that's the end. Page displays us a flag: **sfi18_ctf{idonthavetheflagandwebsiteisdonwwillfixinthefuture}**

## unknown_file

We started with a cursed (and damaged?) [file](./files/unknown_file) with no extension. Let's try to find something in bytes of that (I used `ghex`). ![unknown1](./images/unknown_file_1.png)

Looks like image. Maybe PNG? Let's try to repair this!

We can use PNG headers [specification](https://en.wikipedia.org/wiki/PNG#File_header) to do it.

First of all - 8-byte signature at the begining of file : `89 50 4E 47 0D 0A 1A 0A`

Progress! There is other error than earlier!

![2](./images/unknown_file_2.png)

The key is the last field of `IHDR` header - CRC sum. There are many implementations of this algorithm, but I used [PCRT](https://github.com/sherlly/PCRT) app to repair file.

And we got it!

![We got him!](./images/unknown_file_final.png)

REMEMBER:
>Hint: the flag is all lowercase.

So the flag is: **sfi18_ctf{corrupted_89504e47}**

## photo

![photo](./images/photo.jpg)

We must find where is this place. Just look at the left bottom corner:

![1](./images/photo_1.png)

Phone number? After fast research we got [owner](https://www.az-polska.com/firmy/opal_fphu_wioletta_michajow-morek_rzaska_mp_krakowska_45). Using Google Maps search the location - and place of the red arrow:
 ![2](./images/photo_2.png)

The flag is address on the fenence: **krakowska 50**

While submiting remember:
>Hint: the response is all lowercase with no spaces.

## losts_bits

We started with 12 groups of 7 bits:
```
1000110 1111100 1011010
1110110 0010101 1111001
1111010 1011111 0100000
0010000 0000100 0100111
```

After long, long (long...) research I found a solution: huffman(7, 4) coding. There was a trap: before and after decoding we must reverse each bits group. You can decrypt message e. g. [here](https://www.dcode.fr/hamming-error-correction).

After decrypt we got these bits:
```
1101 1110 1010 1101
1011 1110 1110 1111
0000 0000 0000 0001
```
which obviously are hex characters that you can decode [here](https://gchq.github.io/CyberChef/).

And we got him! Flag is **sfi18_ctf{deadbeef0001}** (remember lowercase!).

## camera_model 

File to this challenge: [click me!](./files/downloader.zip)

We got a few files from this zip: photo, note and... another zip.
So, what's in the note?
```
Pass: the model of the phone that took the picture
```
Lessgo! Exiftool says: `Camera Model Name : SM-S901B`, that is used in (fast research...) **SamsungS22**. 

And this is password to `deep.zip`, which contains... ANOTHER ZIP FILE!

![XD](./images/there_is_another.jpg)

And there's another note:
```
Pass: 440a2432cc29c4838d5574b0a38061ebcaa63f78

Buuuut, black -> emerald
```
Pass looks like kind of hash. 40 characters has only SHA-1. It's time to crack. Fortunately I tried to crack hash using [hashcat](https://github.com/hashcat/hashcat) with [rockyou](https://www.kaggle.com/datasets/wjburns/common-password-list-rockyoutxt) - common passwords in one file - as dictionary. And immediately found that - black_pearl261981 `Buuuut, black -> emerald`, so password for `deeper.zip` is emmerald_pearl261981.

After all we got text file with flag: **sfi18_ctf{C0ngR4tUl4t10n5}**

## binary_code
Have a look at the [document](./files/Binary%20Code.pdf). Of course it's encoded in ASCII. Fast decode and we got kinda assemly code (counting line starts from 1 not 0, cringe):
```
 1 | LOAD @ 12
 2 | STORE @ 14
 3 | LOAD @ 13
 4 | SUB $ 1
 5 | JZERO $ 11
 6 | STORE @ 13
 7 | LOAD @ 14
 8 | MULT @ 12
 9 | STORE @ 14
10 | JUMP $ 3
11 | END $ 0
12 | 5
13 | 3
14 | 0
```
Sooo there are instructions in lines 1-11 and values at 12-14. And there're special signs: 

`@` - means line number

`$` - means just number

So the program will load valure from liine 12 - `5` and store it in line 14. Next, decrease value in line 13 and (if not zero) - go on (lines 3-6). Following, multiply (line 8) value from 14 (actually it's `5`) five times and store result (`25`) in line 14. `JUMP $3` means that lines 3 - 10 are loop. It will execute 3 times and program stops. 

After all operations in line 14 there's `125` and this is flag: **sfi18_ctf{125}**.

Again - not the most obvious flag.
