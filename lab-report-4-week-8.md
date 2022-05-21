# CSE 15L Week 8 Lab Report

# Repo Links
[Our MarkdownParse Implementation](https://github.com/calistajlee/lab6-markdown-parser)

[Reviewed MarkdownParse Implementation](https://github.com/Badflar/markdown-parser)

# Expected Outputs
## Snippet 1
We expect `` `google.com``, `google.com`, `ucsd.edu`

## Snippet 2
We expect `a.com`, `b.com`, `a.com(())`, `example.com`

## Snippet 3
We expect `https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule`

# Test Code
```java
    @Test
    public void testReportSnippet1() {
        try {
            assertEquals(List.of("`google.com", 
                                 "google.com", 
                                 "ucsd.edu"), 
            MarkdownParse.getLinks(readString("reportSnippet1.md")));
        } catch (IOException e) {
            fail();
        }
        
    }

    @Test
    public void testReportSnippet2(){
        try {
            assertEquals(List.of("a.com",
                                 "b.com", 
                                 "a.com(())", 
                                 "example.com"), 
            MarkdownParse.getLinks(readString("reportSnippet2.md")));
        } catch (IOException e) {
            fail();
        }
    }


    @Test
    public void testReportSnippet3(){
        try {
            assertEquals(List.of("https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule"), 
            MarkdownParse.getLinks(readString("reportSnippet3.md")));
        } catch (IOException e) {
            fail();
        }
    }
```

# For our Implementation
## Snippet 1 Output
```
1) testsnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testReportSnippet1(MarkdownParseTest.java:207)
```

## Snippet 2 Output
```
2) testReportSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, b.com, a.com(()), example.com]> but was:<[a.com, a.com((]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testReportSnippet2(MarkdownParseTest.java:220)
```

## Snippet 3 Output
```
3) testReportSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule]> but was:<[
    https://www.twitter.com
,
https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testReportSnippet3(MarkdownParseTest.java:234)
```

# For the Reviewed Implementation
## Snippet 1 Output
```
1) testReportSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testReportSnippet1(MarkdownParseTest.java:60)
```
## Snippet 2 Output
```
2) testReportSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, b.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testReportSnippet2(MarkdownParseTest.java:73)
```

## Snippet 3 Output
```
3) testReportSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule]> but was:<[
    https://www.twitter.com
,
https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testReportSnippet3(MarkdownParseTest.java:87)
```

# Solutions
## Snippet 1
I think there would need to be an extensive code overhaul to handle inline backticks. If it were guaranteed that there is only one extra inline backtick and no escaped inline backticks, then the solution would be simple since it is essentially a different type of brackets. However, with nested inline backticks and methods to escape backticks, it quickly becomes a complex problem to solve.

## Snippet 2
I think this would also need a fairly extensive code overhaul to handle nested brackets/parenthesis. Tracking the extra nested parenthesis/brackets would be relatively simple as it doesn't really care whether the parenthesis are matched, it just needs to find the end of the parenthesis. The tricky part comes from escaped parenthesis with additional logic needed to determine when a parenthesis is escaped and whether it belongs as part of the link or if it is separate.

## Snippet 3
I think this one is a simpler change as we would just need to track the number of newlines between the ending bracket and the content and use that to reject or accept the link. We also would need a trim function to trim newlines that preceed or trail after the link.