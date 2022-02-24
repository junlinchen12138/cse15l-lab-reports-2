[<- Back](index.html)

# Lab Report 4
by Moshe Bookstein

Updated: Feb 24, 2022 15:47 UTC
## Table of Contents
---


## Repos
[My Repo](https://github.com/mBookUCSD/markdown-parse)

[Original Repo Provided](https://github.com/ucsd-cse15l-w22/markdown-parse) 
#### *[Here is the copy I made changes to.](https://github.com/mBookUCSD/MDParseLab8Provided)*

---
## Snippet 1

### What output should be.
```[`google.com,google.com,ucsd.edu]```

### How it was made a test
```
    @Test
    public void testSnip1() throws IOException, NoSuchFileException {

        ArrayList<String> correctOutput = new ArrayList<>();
        correctOutput.addAll(Arrays.asList("`google.com","google.com","ucsd.edu"));
        Path fileName= Path.of("lab8-snip1.md");
        // read the file contents into a string
	    String contents = Files.readString(fileName);
        // run getLinks on the contents of the file
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        assertEquals(correctOutput,links);
    }
```

### Output When Running Given Implementation
Did not pass
```
JUnit version 4.13.2
...E...
Time: 0.014
There was 1 failure:
1) testSnip1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnip1(MarkdownParseTest.java:52)

```

### Output When Running My Implementation
Did not pass
```
JUnit version 4.13.2
..E..........
Time: 0.026
There was 1 failure:
1) testSnip1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnip1(MarkdownParseTest.java:200)

F```

---
## Snippet 2

### What output should be.
`[a.com(()),example.com]`

### How it was made a test
```
    @Test
    public void testSnip2() throws IOException, NoSuchFileException {

        ArrayList<String> correctOutput = new ArrayList<>();
        correctOutput.addAll(Arrays.asList("a.com(())","example.com"));
        Path fileName= Path.of("lab8-snip2.md");
        // read the file contents into a string
	    String contents = Files.readString(fileName);
        // run getLinks on the contents of the file
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        assertEquals(correctOutput,links);
    }
```

### Output When Running Given Implementation
Did not pass
```
JUnit version 4.13.2
...E...
Time: 0.018
There was 1 failure:
1) testSnip2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com(()), example.com]> but was:<[a.com, a.com(()), example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnip2(MarkdownParseTest.java:64)

```

### Output When Running My Implementation
Did not pass
```
JUnit version 4.13.2
..E..........
Time: 0.03
There was 1 failure:
1) testSnip2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnip2(MarkdownParseTest.java:212)

F```

## Snippet 3

### What output should be.
`[https://ucsd-cse15l-w22.github.io/]`

### How it was made a test
```
    @Test
    public void testSnip1() throws IOException, NoSuchFileException {

        ArrayList<String> correctOutput = new ArrayList<>();
        correctOutput.addAll(Arrays.asList("`google.com","google.com","ucsd.edu"));
        Path fileName= Path.of("lab8-snip1.md");
        // read the file contents into a string
	    String contents = Files.readString(fileName);
        // run getLinks on the contents of the file
        ArrayList<String> links = MarkdownParse.getLinks(contents);
        assertEquals(correctOutput,links);
    }
```

### Output When Running Given Implementation
Did not pass
```
JUnit version 4.13.2
...E...
Time: 0.015
There was 1 failure:
1) testSnip3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[https://www.twitter.com, https://ucsd-cse15l-w22.github.io/]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnip3(MarkdownParseTest.java:76)

```
### Output When Running My Implementation
Did not pass
```
JUnit version 4.13.2
..E..........
Time: 0.027
There was 1 failure:
1) testSnip3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnip3(MarkdownParseTest.java:224)

F```


## Questions

1. **Do you think there is a small (<10 lines) code change that will make your program work for snippet 1 and all related cases that use inline code with backticks? If yes, describe the code change. If not, describe why it would be a more involved change.**

I think there would be some small changes to make most of snippet 1 work. The issue with the first line of the snippet is that my code does not check for ` and adding that check to see if there is 1 before the [ and also within either the [] or the []. However, fixing the 7th line involves rethinking how I take care of tracking brackets which is much more involved.

2. **Do you think there is a small (<10 lines) code change that will make your program work for snippet 2 and all related cases that nest parentheses, brackets, and escaped brackets? If yes, describe the code change. If not, describe why it would be a more involved change.**

Yes, simply adding a small check for further brackets and then checking for no spaced iterating backwards to the currently found bracket should fix it.

3. **Do you think there is a small (<10 lines) code change that will make your program work for snippet 3 and all related cases that have newlines in brackets and parentheses? If yes, describe the code change. If not, describe why it would be a more involved change.**

No, fixing this would involve changing many things about tracking brackets and spaces and other things that are core to how my code functions


