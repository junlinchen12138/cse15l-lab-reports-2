# Lab Report 1
by Moshe Bookstein


## Installing VScode
---
The way that these instructions are written is using vs code. If you've never used vs code before, or you don't have it installed. Here is how you go about installing it.

Visit [https://code.visualstudio.com/](https://code.visualstudio.com/) and click download. After it downloads, click on it and go through the install proccess.


## Remote Connection

### Get OpenSSH
---
To start this assignment you will need to install OpenSSH, this allows you to use SSH, , to connect to remote locations that support this kind of connection.

Follow this link and then continue below when done.

**[Get OpenSSH Here](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)**


### Accessing Course-Specific Account
---
After you install OpenSSH you will need to get your course specifc accpount.

Go to this link and follow the steps below.
**[https://sdacs.ucsd.edu/~icc/index.php](https://sdacs.ucsd.edu/~icc/index.php)**


Log into your account 

Under 'Additional Accounts' look for the button labled `cs15lwi22***`

![Image](courseaccount.png)

Your course specific account is your username listed on the page which is identified by the letters shown at the end of your username.

### Using SSH
---
We will be using Visual Studio Code to connect to the remote servers. Open VSCode and navigate to Terminal -> New Terminal

![Image](newterminal.png)

Enter this into the terminal with the letters `zz` replaced with the letters from the end of your username.
```
$ ssh cs15lwi22zz@ieng6.ucsd.edu
```
The first time you connect to the server your computer may give you this warning message. 
```
$ ssh cs15lwi22***@ieng6.ucsd.edu
The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:kshsjr6nYH+sySHnhwerjsu7grPEyZTDl/1xgsyd4cec.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```
If this is the first time you've connected to the server, you should click yes. If you keep connecting to the server and keep getting the message then it may mean there's an issue with your connection or someone's trying to interrupt your connection. 

Type ```yes``` and then after hitting enter, type in your password. As you type in your password, nothing should show up on the terminal, but once you put your password in fully, hit enter. if what you get looks like this, congratulations you have successfully logged in. 

```
Last login: Wed Jan 5 14:49:30 2022 from 68-21-***-**.lightspeed.irvnca.sbcglobal.net

quota: No filesystem specified.
Hello cs15lwi22***, you are currently logged into ieng6-203.ucsd.edu

You are using 0% CPU on this system

Cluster Status
Hostname     Time    #Users  Load  Averages
ieng6-201   21:30:01   15  2.11,  2.24,  2.29
ieng6-202   21:30:01   11  1.64,  1.73,  1.77
ieng6-203   21:30:01   19  0.22,  0.37,  0.40


Wed Jan 12, 2022  9:30pm - Prepping cs15lwi22
```


## Trying Some Commands
Below are some commands that you might want to try and run.

|Commands |
|-----|
|cd ~|
|cd|
|ls -lat|
|ls -a|
|ls `<directory>` where `<directory>` is /home/linux/ieng6/cs15lwi22/cs15lwi22abc, where the abc is one of the other group members’ username|
|cp /home/linux/ieng6/cs15lwi22/public/hello.txt ~/|
|cat /home/linux/ieng6/cs15lwi22/public/hello.txt|


After you use some of these commands you should log out of the server using either:

* CTRL-D
* Type `exit`


## Moving Files with scp
---
As of now we have seen that we can login to a remote computer and run commands on it. Something that is extremely useful when working on remote computers is be the ability to copy files from your computer to another computer. While there are many ways to do this, the way that we will use in this assignment is called `scp`. SCP (secure copy) is a command that you run from the terminal on the device that has a file you wish to copy to another device. The command copies that file onto the server you that you tell it to. Since you run this from your own terminal, you should not be logged into `ieng6` when running `scp` command.

To start, we need to file to copy over. On your computer create a file called `WhereAmI.java` and place this code within it. 

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

Run it using `javac` and `java` on your computer and see what it prints out. 

Now, using the `SCP` command we are going to copy it to the server we were logged into before. In the terminal from the directory where the file we made is located, run the command below replacing `zz` with the letters from your course-specific account. 
```
$ scp WhereAmI.java cs15lwi22zz@ieng6.ucsd.edu:~/
```
Just like with SSH, it will ask you for a password which you should type in and then hit `enter`. 

Now login to `ieng6` again using SSH, and use the `ls` command. you should see the files in that directory.

While you are still logged into the server, try running the file with `javac` and `java` and see how the output of the file differs from when you ran it locally on your own computer. 

# **Important!!!** 
**Something to be aware of is that *SCP DOES NOT CHECK* if that file already exists in the directory. If it finds a file with the same name, it will just overwrite that file. You should try this, make a change to the file you have saved locally, use SCP to copy it over to the server again, and then run it again and see if the output has changed.** 
---

---
## Setting an SSH Key
While typing your password in once or twice doesn't seem like it takes much time, over the course of even a quarter, you might type it in a hundred or two hundred times. This is a significant amount of time to be spending typing in your password, and people that figured out better ways of confirming that the person running the command is authorized to do so. we are going to be using something called an SSH key to pre-approve our computer to log into the server without having to type in the password every time. 

The main idea behind an SSH key is that on our computer, we create a public and private key using the command `ssh-keygen` . we give the public key to the server and keep the private key on our machine, and then when we try to log into the server the server will check with our computer that have the private key. If we do it will just log us in. This is an extremely common setup in any work environment that requires using code on a server. 

Below is what you should run on your computer(the client)
*Instead of the file xxxxxxxxxxxxx, enter a path to the new file location on your computer such as C:\Users\Username/.ssh/id_rsa*
```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\Moshe/.ssh/id_rsa): xxxxxxxxxxxxx
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in example.
Your public key has been saved in example.pub.
The key fingerprint is:
SHA256:mwE2E0rC/MU2dzBiSuXub7qcfr6wYx5LIK5J/DF6tIQ moshe@DESKTOP
The key's randomart image is:
+---[RSA 3072]----+
| o..o +          |
|  *o==..         |
|   .  . S        |
|.   .o o +       |
| o.Eoo+ o        |
| . =.BB          |
|    +o.=         |
|.o.=o+.*.        |
|o..o*=Ooo.       |
+----[SHA256]-----+
```
If you’re on Windows, follow the extra `ssh-add` steps here: [https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#user-key-generation](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#user-key-generation)

This process created two new files on your computer, a **public** key and a *private* key. The private key in the example above is labeled `id_rsa`. The **public** key is labeled very similarly, `id_rsa.pub`. Both are stored in the `.ssh` directory on your computer, which is where they should be located. 


## Optimizing Remote Running


![Image](vsstartup.png)
