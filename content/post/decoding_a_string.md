+++
date = "2016-03-04"
title = "Decoding a string"
tags = ["ctf", "series", "hugo"]
description = "decoding a string"
index = true
highlight = true
+++


So I was recently browsing Seek which is an Australian job searching website. I was looking to see where the Industry is headed and what skills I should be focusing on developing for a career in Information Security.

I came across a security analyst role that had the following:

For extra kudos, solve the following and include the solution in your cover letter:


>UEsDBAoACQAAAM1rV0xjL16LGAAAAAwAAAAPABw
Ad2hhdHNpbnNpZGUudHh0VVQJAAPBfI9a1HyPWnV4
CwABBAAAAAAEAAAAALvAF23HELDMWWvH/sMUFqMab
C8PE+ZY/lBLBwhjL16LGAAAAAwAAABQSwECHgMKAA
kAAADNa1dMYy9eixgAAAAMAAAADwAYAAAAAAABAAA
ApIEAAAAAd2hhdHNpbnNpZGUudHh0VVQFAAPBfI9a
dXgLAAEEAAAAAAQAAAAAUEsFBgAAAAABAAEAVQAAAHEAAAAAAA==


What is it? Can I work it out? I was curious.

Challenge Accepted!

If you want to give it a go yourself, pause here before reading on.
To Decode or not to Decode?

Looking at the string provided (a string is just a sequence of characters), it looks like it might have been encoded with Base64.
The string is made up of the characters: a – z, A – Z, 0 -9, + and / which is used by Base64.
There is also some padding on the end of the string represented by the == which fulfils Base64’s length requirements.
This [Wikipedia article] (https://en.wikipedia.org/wiki/Base64) explains Base64 in more depth.

So lets decode it!

I like to use a tool called [Cyber Chef] (https://gchq.github.io/CyberChef/), its provided by the [Government Communications Headquarters (GCHQ)] (https://www.gchq.gov.uk/) an intelligence and security organisation in the UK. Its a web app that has a number of different tools covering encryption, encoding, compressio, data analysis etc.

Decoding from Base64 resulted in:

PK..
.	...ÍkWLc/^.............whatsinside.txtUT	..Á|.ZÔ|.Zux.............»À.mÇ.°ÌYkÇþÃ..£.l/..æXþPK..c/^.........PK....
.	...ÍkWLc/^.....................¤.....whatsinside.txtUT...Á|.Zux.............PK..........U...q.....

Immediately whatsinside.txt, a text file stood out.
File Signatures

I tried to determine how to access the text file or what was in it, but kept running into dead ends. I tried using a function called Magic in CyberChef, that attempts to automatically detect the properties of the data, but that provided no leads.

A friend suggested that it was a zip file.

That makes sense but how did they know that? they said they just did…
Hmm… there must be a way to identify that this is a zip file without guessing or having come across something like this before right?

Right! I did some research on how you would be able to identify different file types and came across a Wikipedia article about [file signatures] (https://en.wikipedia.org/wiki/List_of_file_signatures) (also refered to as [magic numbers] (https://en.wikipedia.org/wiki/Magic_number_(programming)).

File formats when viewed as text are meaningless (most of the time), but on some occasions, the file signature has recognisable text which would allow you to discern what the file format is. The string starts with PK. PK as it turns out refers to Phil Katz, the developer of the ZIP file format.
PK can also refer to other file formats built on the ZIP format such as docx, jar and apk.

So I saved the string as a ZIP file, opened the archive and lo and behold there lied the whatsinside.txt text file.
!/static/img/2019/password_needed.png
This isn’t over.. yet
Whats the password?

Given that this came from a job listing I didn’t have much to go off in terms of identifying what the password was.
I went over the job listing again, thinking that a clue might have been left somewhere in the content.
Alas that search was in vain so I decided on a different approach.

If there are no hint to be found, then it has to be something simple or easily crack-able. I thought of compiling a list of the words inside the job description, then attempting to brute force the password using something like John the Ripper.

Instead i opened for a ZIP file password remover on passwordrecovery.io.

I uploaded the ZIP file and it turns out the password is… password

(insert video of me kicking down a door that has a sign saying pull)

Go figure.. I should have just guessed that to begin with.

I opened the text file and lo behold the mystery was unveiled.

So whats bobbytables? well it could quite literally mean a person called bobby and his tables?


Almost! its a reference to an xkcd comic called The Exploits of a Mom.
an xkcd comib about the exploits of a mom and little bobby tables

Brilliant! Curiosity satiated!

If you know of or come across similar challenges, let me know via twitter or send me an email, houman@whoishou.com

