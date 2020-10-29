# Wonderland

BOX: https://drive.google.com/drive/folders/1m7SArrM_1NX8Y-w7uPA6cdnw6ttMuWl-?usp=sharing

CREDENTIALS FOR FUTURE HACKATHONS:
cheshire: AliceTheQueen
root: Alice


FLAGS AND CLUES: https://drive.google.com/file/d/1RAWQpHb3VmkUtMCD6rVMtCmq_vWEuTK7/view?usp=sharing

This box is filled with “love letters” from a secret admirer to a person named “Alice”. Some will be published on simple web pages in full emo teenage glory, some will be text and ideas for pages kept in txt files on the box itself. They haven't yet published their "work of devotion" - they are still sorting their thoughts, working up the courage, making styling choices, finding a way to fit all of the pages together, etc. Should feel unfinished, moody, Alice in Wonderland themed, and suggest at the possibility that students might be going down a rabbit hole... Or are they? Privilege Escalation was kept intentionally simple, to balance out the “fake credentials” found at the end of the cookie puzzle

There will be 6 flags
 
3 from exploring web pages available through port 80
3 from actually breaking into the box

With each flag BUT the root flag, there will be a clue! included. The clue will be a few letters represented in binary. All of the clues will be combined into a word that must be unscrambled and then added into the “root” flag to complete it.



# Flags:

A dirb scan will show 4 directories of interest: portal, cat, letters, and phpmyadmin. Attempting to access the phpmyadmin directory will be a dead end. 3 flags can be found by exploring the rest of the website.

![](https://i.imgur.com/j1pf1I4.png)



(1) If you go to the URL, the landing page offers very little information, although if they haven't blocked sound in their browser, they will immediately hear Alice in Chains' 'Down In A Hole" which is appropriately goth and on theme

![](https://i.imgur.com/7UT8ZyZ.png)

A quick visit to humans.txt will direct you to /Muse/my_eyes.html. Look carefully at all images of the Muse - click on them - some of them will be captioned with a portion of a flag. Combine the flag bits into a complete flag - there will also be a clue! (The short-cut for this is simply to view the page source - you'll see the flag there. Am currently trying to find a way to prevent that from being so easy, but my front-end coding skills are fairly limited)

![](https://i.imgur.com/j2K7eaN.jpg)


(2) Navigating to the /portal/ directory will take you to a page with the infamous bottle that says drink me (and some text). This page will link to another directory - /pills/ - where you will have to click through to choose which side of the mushroom you would like to try - the red side, or the blue side. Each side will link to a page with some text that says "is this a flag? i dunno". The pages are identical. They each link to a page with a flag and a clue. Try both sides of the mushroom and input their flags to see which one works - that's the clue that you'll have to hang onto!

![](https://i.imgur.com/0YUiEDF.png)

![](https://i.imgur.com/jJFV6Yi.png)

![](https://i.imgur.com/V0VfFj9.png)





(3) Navigating to the /cat/ directory will bring you to a cheshire cat. 

![](https://i.imgur.com/1TQ7n8C.png)

If you explore the page with your mouse, you'll see that one of its teeth is linked to a page /Fortuna/Fortuna.php (viewing the page source will expose this information, and the link, as well). Clicking through to this page, "Alice" is informed that her fortune is about to change.

![](https://i.imgur.com/iY4UKr0.png)

Click the tarot card to go to a page where called "batch_made_in_haven" that features a welcome message and a picture of a fortune cookie *cracked open to show a message that you have to look closely at to read*. Below the fortune cookie pic are fields for username and passcode and a button for submitting that information. 

![](https://i.imgur.com/TdszPOL.png)

No matter what you input, you will get an "Incorrect login" message.

![](https://i.imgur.com/yfUwEXW.png)

However, if you LOOK AT YOUR COOKIES, you will see you have one cookie called "loggedin" and it is set to "no". Change the value to yes, resubmit your request to the browser, and get the following message: "well done, alice
your passcode is curiouser
and curiouser" 

![](https://i.imgur.com/JO71zNg.png)

This links to the flag and the clue!

![](https://i.imgur.com/9D6Vz1h.png)


(4) Anonymous FTP into the box through the high port into the user cheshire's home directory, and find the flag (and clue) in the list of files

(5) Get a low privilege reverse shell into the box at the root directory, look around you as daemon to get a flag and a clue

(6) Sudo su into root, and then check root home for a file with a flag (minus one word, which is compiled from all of the other clues!)
 

# Exploit Walkthrough

(1) Discover the IP of the new device on your network

(2) Nmap scan to see which ports are open. 

![](https://i.imgur.com/tC6HbKP.png)

(3) Nmap all-ports scan, using the -p- option

![](https://i.imgur.com/wou4pRU.png)

(4) Anonymously FTP into the box through the service running on port 40404. When you enter in the "ftp <ip> 40404" command, the banner will remind you that you might need to switch to passive mode after login with the command "passive". You may or may not need to do so. Will not be able to upload any files, only download. Most files are in the same format: AUTHOR__Name-Of-Work, with the exception of a handful. The files relevant to getting into the box are in that handful.

![](https://i.imgur.com/miBvW0P.png)

![](https://i.imgur.com/9N5qsnD.png)

(5) If student gets and then opens the file “thequeensstandard.txt”, they will get a flag - "paint_the_roses_red" encoded in braille - and a clue

![](https://i.imgur.com/8UvavK6.png)


(6) If student gets and then opens the file “breadcrumbs.txt”, they will find two strings encoded in base64. Decoded, the strings “cheshire:AliceTheQueen” and “daemon:AllMadHere” emerge

![](https://i.imgur.com/5WwD6CH.png)

(7) If student gets and then opens the file ”usefulcommand.txt”, they will find the following: 
python -c "import pty; pty.spawn('/bin/sh')" and then below, a reminder to try using passive mode if they run into issues logging into proftpd

(8) Using the “cheshire” credentials, log in to ProFtp on port 21. “Daemon” credentials will not work. You may or may not have to switch to passive mode. Also, there are sometimes issues with getting a shell while in passive mode. in which case, restart both boxes

![](https://i.imgur.com/O8GwwSG.png)

(9) Set up a listener

(10) “Put” a reverse shell file into the directory you find yourself in. This is not the web root, you'll have to use the other files in the folder to figure out where you are (/opt/lampp/htdocs/letters)

![](https://i.imgur.com/amsdTsT.png)

(11) Direct the browser to run the file you just uploaded

![](https://i.imgur.com/7GnESMM.png)

(12) Check your listener - you can see that you now have a shell as daemon

![](https://i.imgur.com/Xi9BqUW.png)

(13) You should immediately see that there is a file in the present directory called “another_clue.txt”. This includes a flag encoded in ROT(-9), which says {Be_what_you_would_seem_to_be}, and a clue

![](https://i.imgur.com/7ZB1bRl.png)

![](https://i.imgur.com/Qt9ESZC.png)


(14) Sudo su into root, perhaps after checking the /etc/passwd and /etc/group files to see that they *can* do so. They should already have the password from step (6)

(15) Respond to the error message by entering in the contents of usefulcommand.txt (below) and then try again to sudo su
python -c "import pty; pty.spawn('/bin/sh')" 

(16) You're now root!

![](https://i.imgur.com/zF4H637.png)

(17) You will find last flag in the root.txt file. This flag will include a word that is missing (represented by \*), with instructions to put together the clues to complete the flag. *When completed, the flag will be "she's_a_wildflower"

![](https://i.imgur.com/9LWB33z.png)
