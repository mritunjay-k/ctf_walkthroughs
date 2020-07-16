# [Pwned:1](https://www.vulnhub.com/entry/pwned-1,507/)

* Step 1: Start with scanning the machine with nmap. 
  ```bash
  nmap -A -p- <box-ip> -oN <output-file>
  ```
  ![save demo](https://i.ibb.co/v30gjKb/nmap.png)
  <br><br>
* Step 2: Checkout the web application on port 80
  ![save demo](https://i.ibb.co/rcfTgXW/pwned-200.png)
  <br><br>
* Step 3: There's nothing to see, lets directory brute force. I am going to use Dirbuster's directory-list-2.3-big.txt<br><br>
  ![save demo](https://i.ibb.co/dmvDZ9Z/initial-enum.png)
  <br><br>
  Here we can see a directory named "hidden_text" which looks interesting. Lets check it out.<br><br>
  ![save demo](https://i.ibb.co/qDT29zD/hidden-text.png)
  <br><br>
  The secret.dic file is a dictionary with potential file names.
  <br><br>
  ![save demo](https://i.ibb.co/kGP5fV5/secret-dic.png)
  <br><br>
  Lets save this dictionary locally and fuzz the web application.
  <br><br>
  ![save demo](https://i.ibb.co/dbmGP7M/later-enum.png)
  <br><br>
  Looks like we found something. Lets check that out.
  <br><br>
  ![save demo](https://i.ibb.co/5MC44YG/login-page.png)
  <br><br>
  Its a login page. But we don't have a credential. Lets check the source code.
  <br><br>
  ![save demo](https://i.ibb.co/mHND5kk/source-of-the-login-page.png)
  <br><br>
  **WOW!!**
  <br><br>
* Step 4: Intial Foothold. We have an SSH service running on the server. Remember? Lets try the credentials into it.
  <br><br>
  ![save demo](https://i.ibb.co/YtbKbwZ/ssh.png)
  <br><br> **NICE**
  <br><br>
  Upon enumeration I found an SSH private key in the share folder. Bring it into your local environment using the http.server module of python3.
  <br><br>
  ![save demo](https://i.ibb.co/BnC9GtB/id-rsa.png)
  <br><br>
  Now login to the server with the user Ariana through ssh using this key.
  <br><br>
  ![save demo](https://i.ibb.co/Mh458jf/got-ariana.png)
  <br><br>
* Step 5: Privilege Escalation.
  <br><br>
  Upon enumeration i saw that a file named "messenger.sh" can be ran using sudo by another user Selena. This file takes a user's name and a random string as input. The random string is executed in the shell without any restriction. So, this file can be used for priv-esc.
  <br><br>
  ![save demo](https://i.ibb.co/PDWM3tR/messenger-sh-view.png)
  <br><br>
  So, I ran the file by the user **selena** and got a shell.
  <br><br>
  ![save demo](https://i.ibb.co/Phm6DYn/pwn-selena.png)
  <br>
  ![save demo](https://i.ibb.co/Yh4m62y/pwned-selena.png)
  <br><br>
* Step 6: Privilege Escalation to root. Upon running the **id** command, the user looks to be present in a very interesting group named **docker**. Lets leverage Docker to attain a root shell.
  <br><br>
  ![save demo](https://i.ibb.co/qx4VdXQ/priv-esc-to-root.png)
  <br>
  ![save demo](https://i.ibb.co/mC7DZDX/root-txt.png)
  <br><br>
  **BOOM! WE HAVE A ROOT SHELL**

**So, this completes the box. Hope you all enjoyed it.**<br>
**Thank You for reading.**
