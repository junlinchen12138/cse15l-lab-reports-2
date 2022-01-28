[<- Back](index.html)

# Lab Report 2
by Moshe Bookstein

Updated: Jan 28, 2022 07:52 UTC
## Table of Contents
---
1. [Infinite Loop Bug](#code-change-1)

2. [Non-Link Bracket Bug](#code-change-2)

3. [Images as Links Bug](#code-change-3)



## Code Change 1:
#### The Infinite Loop Bug
---

![testfile2commit](labreport2images\testfile2commit.png)

[test-file2.md](https://github.com/mBookUCSD/markdownParser/blob/3d4bff6967e6eb0bd7818ab4a2882b9a62ea72ea/test-file2.md)

```
$ java MarkdownParse.java test-file2.md
```
No command line output as it causes an infinite loop as seen in the screenshot below.

![Infinite Loop](labreport2images\testfile2error.png)

The symptom was the program running well beyond what time it should take and making the computer slow down immensely. The bug itself was the fact that the loop would expect that if there was anything after a link, it must be looked at, but it would loop forever if no link was found as it kept checking the same characters over and over as it tried to find the next link that was not there. The failure inducing input was a file that had text after the last link in the file, it caused the loop to keep checking the same part of the file over and over for a non-existent next link.

## Code Change 2:
#### The Non-Link Bracket Bug
---

![testfile3commit](labreport2images\testfile3commit.png)

[test-file3.md](https://github.com/mBookUCSD/markdownParser/blob/3d4bff6967e6eb0bd7818ab4a2882b9a62ea72ea/test-file3.md)

```
$ java MarkdownParse.java test-file3.md
Exception in thread "main" java.lang.StringIndexOutOfBoundsException: begin 0, end -1, length 53
at java.base/java.lang.String.checkBoundsBeginEnd(String.java:4601)
at java.base/java.lang.String.substring(String.java:2704)
at MarkdownParse.getLinks(MarkdownParse.java:30)
at MarkdownParse.main(MarkdownParse.java:43)

```
The symptom was that there was an index out of bounds exceptions thrown. The bug was that the code that checked for the next part of the link never took into account that brackets could be intentionally put in the text of the file without them implying a link, so when the body of the link did not follow, it went out of bounds when trying to get a substring of something that did not exist. The failure inducing input was a file with brackets within its text that were not meant to be links.

## Code Change 3:
#### The Images as Links Bug
---

![testfile6commit](labreport2images\testfile6commit.png)

[test-file6.md](https://github.com/mBookUCSD/markdownParser/blob/3d4bff6967e6eb0bd7818ab4a2882b9a62ea72ea/test-file6.md)

```
java MarkdownParse.java test-file6.md
[page.com]
```

The symptom was the URL of an image on the page being printed out when it was not supposed to. The bug was that the code that checked for where links are assumed that everything that followed the template of []() was a link, however, images are also templated the same way while just including an ! directly before the []() and so were included. The failure inducing input was a file with an image on it with the URL “page.com”.
