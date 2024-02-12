# Lab 3 (Bugs and Commands)

## Bugs in Week 4 Lab

The bug in focus will be involving the ```ReverseInPlace()``` method located in ```.\lab3\ArrayExamples.java```. The issue the method is that it was not utilizing a temporary variable, meaning that some elements are becoming overwritten. <br>
<br>
It does not appear like buggy code in the case of an array with a single element.
```
public void testReverseInPlace() {
  int[] input1 = { 3 };
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{ 3 }, input1);
}
```
This test cases passes with no issues, which can be seen from the output of the JUnit tests here:
```
JUnit version 4.13.2
....
Time: 0.027

OK (4 tests)
```
However, once we incorporate a more comprehensive test such as providing an Array with multiple elements, we can start to see bugs appearing in the program. The following test implemented will be used to test for this issue. The test is named ```testReverseInPlaceMoreElems```.
```
@Test
public void testReverseInPlaceMoreElems() {
  int[] input1 = {1, 2, 3, 4};
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{4, 3, 2, 1}, input1);
}
```
The test is inputs an Array of four elements and expects the same Array to be editted in reverse order. When we run the tester file now, it results in a failure in one of the JUnit tests, specifically the one that was just implemented. The following output is given.
```
JUnit version 4.13.2
...E..
Time: 0.03
There was 1 failure:
1) testReverseInPlaceMoreElems(ArrayTests)
arrays first differed at element [2]; expected:<2> but was:<3>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReverseInPlaceMoreElems(ArrayTests.java:16)
        ... 30 trimmed
Caused by: java.lang.AssertionError: expected:<2> but was:<3>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 36 more

FAILURES!!!
Tests run: 5,  Failures: 1
```
By utilizing a debugging print statement: ```System.err.println(Arrays.toString(input1));```, we are able to identify the symptoms. As it stands, the current output of the ```ReverseInPlace()``` method with the input of ```[1, 2, 3 ,4]``` is ```[4, 3, 3, 4]```. This allows us to identify that the method is not reversing properly and is actually overwritting some of the elements within the Array.<br>
<br>
This happens because the original code does not make use of a temporary variable, meaning that while changing elements, some become overwritten as a result. The fixed code is as presented in the following block:
```
static void reverseInPlace(int[] arr) {
  for(int i = 0; i < arr.length / 2; i++) {
    int temp = arr[i];
    arr[i] = arr[arr.length - i - 1];
    arr[arr.length - i - 1] = temp;
  }
}
```
Instead of iteratoring over the entire array, we will only iterate half of it and every time, we are using a temporary variable to save the current element we are about to overwrite and swapping it with the element that is able to replace it. By implementing this change, we are able to successfully pass the tests written.

```
JUnit version 4.13.2
.....
Time: 0.021

OK (5 tests)
```

## Researching Commands
The ```find``` command is generally used to search out all files of a specific nature. By running ```man find```, we are given the following output that will help show different ways that ```find``` could be used.

```
FIND(1)                                                                               General Commands Manual                                                                               FIND(1)

NAME
       find - search for files in a directory hierarchy

SYNOPSIS
       find [-H] [-L] [-P] [-D debugopts] [-Olevel] [path...] [expression]

DESCRIPTION
       This manual page documents the GNU version of find.  GNU find searches the directory tree rooted at each given file name by evaluating the given expression from left to right, according to      
       the rules of precedence (see section OPERATORS), until the outcome is known (the left hand side is false for and operations, true for or), at which point find moves on  to  the  next  file      
       name.

       If  you are using find in an environment where security is important (for example if you are using it to search directories that are writable by other users), you should read the "Security
       Considerations" chapter of the findutils documentation, which is called Finding Files and comes with findutils.   That document also includes a lot more detail  and  discussion  than  this      
       manual page, so you may find it a more useful source of information.

OPTIONS
       The  -H,  -L  and  -P options control the treatment of symbolic links.  Command-line arguments following these are taken to be names of files or directories to be examined, up to the first      
       argument that begins with `-', or the argument `(' or `!'.  That argument and any following arguments are taken to be the expression describing what is to be searched for.  If no paths are      
       given, the current directory is used.  If no expression is given, the expression -print is used (but you should probably consider using -print0 instead, anyway).

       This  manual  page  talks  about `options' within the expression list.  These options control the behaviour of find but are specified immediately after the last path name.  The five `real'      
       options -H, -L, -P, -D and -O must appear before the first path name, if at all.  A double dash -- can also be used to signal that any remaining arguments are not options (though  ensuring
       that all start points begin with either `./' or `/' is generally safer if you use wildcards in the list of start points).
```
