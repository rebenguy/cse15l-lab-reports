# Welcome incoming 15L students! 
## Today you will be learning how to log into a course-specific acount on **ieng6**
---
## Installing Visual Studio Code

First, you must download [Visual Studio Code]( https://code.visualstudio.com/) (VScode).

After downloading VScode, open a window and you should get something similar to this picture:

![Image](VScode.png)

---

## **Remotely Connecting**

Now, you need to find your CSE15L course-specific account *[here](https://sdacs.ucsd.edu/~icc/index.php)*.

After you find your course-specific account, open a new terminal in VSCode (Ctrl + ` or Terminal -> New Terminal menu option) and type:

```
$ ssh cs15lsp22zz@ieng6.ucsd.edu
```

**Note:** replace `zz` with the letters in your course-specific account

You will get this message since it is your first time connecting to this server: 

```
⤇ ssh cs15lsp22zz@ieng6.ucsd.edu

The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.

RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.

Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type `yes` and press enter. It will prompt you for a password. Type in your password (it will not be shown in the terminal). 

You will get something like this:

![Image](login.png)

Your terminal is now connected to a computer in the CSE basement! Any commands you run will run on that computer. 

Your computer is the `client`, while the basement computer is the `server`.

---

## **Useful commands**

> Some commands to remember:

* cd ~
* cd
* ls -lat
* ls -a
* ls /home/linux/ieng6/cs15lsp22/cs15lsp22abc (replace abc with a username)
* cp /home/linux/ieng6/cs15lsp22/public/hello.txt ~/
* cat /home/linux/ieng6/cs15lsp22/public/hello.txt

Try these out on both the client and server and see what they do and how they are different. 

For example, *ls -a* organizes the home directory into alphabetical order.

![Image](lsa.png)

**Note:** To log out of the remote server in your terminal, use `Ctrl-D` or type and run `exit`

---

## **Moving Files into `scp`**

Now we will be learning how to copy files back and forth between your computer and the remote computer using the command `scp`, which will *always* be ran from the `client`.

First, create a file named **WhereAmI.java** with this code:

```
class WhereAmI {
  public static void main(String[] args) {
    System.out.println(System.getProperty("os.name"));
    System.out.println(System.getProperty("user.name"));
    System.out.println(System.getProperty("user.home"));
    System.out.println(System.getProperty("user.dir"));
  }
}
```

You can use *javac* and *java* to run it on the `client`.

Now, run this command (using your username):
```
scp WhereAmI.java cs15lsp22zz@ieng6.ucsd.edu:~/
```
Similar to when you log in with `ssh`, you will be asked to enter your password. 

Enter your password to log in and run `ls`. You will see the **WhereAmI.java** file in your home directory! 

Now you can use *javac* and *java* to run it on the `server`.

Here, on the right is the client and on the left is the server. 

![Image](scp.png)

After running *javac* and *java* on the client, I ran scp WhereAmI.java cs15lsp22zz@ieng6.ucsd.edu:~/ and entered my password. 

Then, I ran *javac* and *java* on the server, meaning I successfully copied the file from the client to the server.

---

## **Setting an SSH Key**

Everytime we log in or run scp, we have to put in our password. This is time consuming. To increase efficiency, we will us `ssh` keys, more specifically, a program call `ssh-keygen`.

On your `client`, enter `ssh-keygen` into the terminal, which should show this:
```
$ ssh-keygen

Generating public/private rsa key pair.

Enter file in which to save the key (/Users/<user-name>/.ssh/id_rsa): /Users/<user-name>/.ssh/id_rsa

Enter passphrase (empty for no passphrase):
```

Do **NOT** enter a passphrase, just click enter.
```
Enter same passphrase again: 

Your identification has been saved in /Users/<user-name>/.ssh/id_rsa.

Your public key has been saved in /Users/<user-name>/.ssh/id_rsa.pub.
The key fingerprint is:

SHA256:jZaZH6fI8E2I1D35hnvGeBePQ4ELOf2Ge+G0XknoXp0 <user-name>@<system>.local

The key's randomart image is:

+---[RSA 3072]----+
|                 |
|       . . + .   |
|      . . B o .  |
|     . . B * +.. |
|      o S = *.B. |
|       = = O.*.*+|
|        + * *.BE+|
|           +.+.o |
|             ..  |
+----[SHA256]-----+
```
Now, log into the server.
Once on, enter `mkdir .ssh` and logout of the server.

Once back on the client, type in 
```
scp /Users/<user-name>/.ssh/id_rsa.pub cs15lsp22zz@ieng6.ucsd.edu:~/.ssh/authorized_keys
``` 
(Using your username and path seen in the command above)

After this is completed, you should be able to use `ssh` or `scp` without having to type in your password.

You should see something like this:

![Image](nopswd.png)

---

## **Omptimizing Remote Running**

* **ssh** - Directly run a command on the remote server and then exit by writing a command in quotes at the end of an ssh command.

* **;** - Run multiple commands on the same line by separating them with ;

* **Up-arrow** - The up-arrow recalls the last command ran