[<- Back](index.html)

# Lab Report 3
by Moshe Bookstein

Updated: Feb 11, 2022 18:14 UTC
## Table of Contents
---
1. [ To Copy a Whole Directory](#copying-a-whole-directory-to-ieng6)

2. [Log In -> Compile -> Test](#logging-in-compiling-and-testing)

3. [ All In One](#in-one-line)

### Copying a Whole Directory to ieng6 
---
To copy the whole directory to ieng6 we use the `scp` command with the `-r` modifier as seen below.
``` 
scp -r . cs15lwi22**@ieng6.ucsd.edu:~/markdown-parse
```
![Image](labreport3images\copyFile.png)

### Logging In, Compiling, and Testing
---
Now we log into the ieng6 server with `ssh` and the `cd` into the markdown-parse directory that was made when we copied the files over with `scp`. Then we compile the MarkdownParse.java and MarkdownParseTest.java files with `javac` and then run MarkdownParseTest with `java`.


![Image](labreport3images\loggingIn.png)

### In One Line
---
To combine all the commands to do this in one command I have adjusted the command a little due to a misconfiguration of the ieng6 server. *More info in [this](https://piazza.com/class/kxs0toocqhv4og?cid=354) piazza post.*

This is the one line command I ended up with.

```
ssh cs15lwi22ahy@ieng6.ucsd.edu "mkdir markdown-parse"; scp -r *.java *.md lib/ cs15lwi22ahy@ieng6.ucsd.edu:~/markdown-parse; ssh cs15lwi22ahy@ieng6.ucsd.edu "cd markdown-parse/; /software/CSE/oracle-java-se-14/jdk-14.0.2/bin/javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParse.java MarkdownParseTest.java; /software/CSE/oracle-java-se-14/jdk-14.0.2/bin/java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest;"
```

It looks compilcated but can be broken down into a few steps. 

First it runs `ssh` to get into ieng6 and uses `mkdir` to make a directory to copy the files into. `Scp` will only make a directory if it is copying a whole directory, but since I wanted to only copy the files we needed, I added this step. 

Next it runs `scp` to copy all the `.java` files  `.md` files and all files in `lib/` to ieng6. 

Finally it runs `ssh` again with some `javac` and `java` commands passed along with it to compile and run the tester on ieng6.

![Image](labreport3images\multicommandstack.png)