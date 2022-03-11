[<- Back](index.html)

# Lab Report 5
by Moshe Bookstein

Updated: Mar 11, 2022 20:25 UTC
## Table of Contents
---

- [How I found the differences in output]()

- [Test 496](#test-496)

- [Test 498](#test-498)


## How I found the differences in output
I generated result files using 

`bash script.sh > givenImplresults.txt` for other implementation in its directory.

and

`bash script.sh > myImplresults.txt` for my implementation in my directory.

Then I used

`diff ~/LabReport5/myImpl/myImplresults.txt ~/LabReport5/givenImpl/givenImplresults.txt > mydiffsgiven.txt`

to create a diffs file and looked for differences in output and then checked the corresponding files to see what the issues may have been.


## Test 496
---
Expected Output: `[]`

The other implementaion gives the correct result for this test. 

My Output: `[foo(and(bar]`

Other Output: `[]`

The bug in mine is the fact that I don't check for brackets within the link, and overall my bracket finder need work because even if I just wrote a line to check if there were brackets within it, that would not be a corrected solution. Links can have brackets within them as long as if they are opened they must be closed withing the link. This means that this output isn't wrong becuse it did not detect the brakcets within, but *becuse it did not detect that they had not been closed.*


Code in my implementation to be fixed (*and honestly, **fully** overhauled*):
```
// if we are potentially looking at a link after finding []
            if (findLink) {
                // if there arent any other brackets on the bracket tracker we 
                // should check what we are looking at and if it is an open 
                // braket and mark the current index as the start of a link
                if (bracketTracker.isEmpty()) {
                    if (curr == '(') {
                        bracketTracker.push(curr);
                        start = currentIndex;
                    } 
                    // something else came after the ] that wasn't (
                    else { 
                        findLink = false;
                    }
                } else {
                    // if ) then we should note the end of the link down
                    if (curr == ')') {
                        end = currentIndex;
                        //create substring with only the text of the link
                        String link = markdown.substring(start + 1, end).trim();
                        //check the link for any spaces or newlines
                        if (!containsSpace(link)) {
                            toReturn.add(link);
                        }
                        // take the ( off the bracket tracker
                        bracketTracker.pop();
                        findLink = false;
                    }
                }
            }
```

## Test 498
---
Expected Output: `[foo(and(bar]`

Both implementaions output the incorrect result.

My Output `[<foo(and(bar]`

Other Output `[]`

The link should be included becuse it is within `<...>`. For the other implementation the bracket tracker gets rid of the link when it should not. To fix this would involve rewriting several key parts of the way that brackets are detected and writing a way to find and check if offending brackets are within `<...>`. My solution is closer but only by accident, since mine doesnt detect bracket issues like this, as seen in the first test above, it doesnt throw it away becuse of the brackets, but becuse it doesn't have any checks for `<...>`, it just includes it in the link even when it should not. To fix mine I would need to overhaul the bracket tracking, and implement a check for `<...>`.

Code in my implementation to be fixed is in a similar place to the one above, but with the added issue of managing finding and tracking the effects of `<...>`.

Code in the other implementation to be fixed is the close bracket finder as included below, with the added issue of managing finding and tracking the effects of `<...>`.

```
static int findCloseParen(String markdown, int openParen) {
        int closeParen = openParen + 1;
        int openParenCount = 1;
        while (openParenCount > 0 && closeParen < markdown.length()) {
            if (markdown.charAt(closeParen) == '(') {
                openParenCount++;
            } else if (markdown.charAt(closeParen) == ')') {
                openParenCount--;
            }
            closeParen++;
        }
        if(openParenCount == 0) {
          return closeParen - 1;
        }
        else {
          return -1;
        }

    }
```
