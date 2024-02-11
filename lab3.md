# Lab 3 (Bugs and Commands)

## Bugs in Week 4 Lab

The bug in focus will be involving ```ArrayExample ReverseInPlace()```. The issue with ```ArrayExample ReverseInPlace()``` is that it was not utilizing a temporary variable, meaning that some elements are becoming overwritten. <br>
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
However, once we incorporate a more comprehensive test such as providing an Array with multiple elements, we can start to see bugs appearing in the program. The test implemented will be used to test for this issue.
```
@Test
public void testReverseInPlaceMoreElems() {
  int[] input1 = {1, 2, 3, 4};
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{4, 3, 2, 1}, input1);
}
```
When we run the tester file now, we result in a failure in one of our JUnit tests, specifically the one that was just implemented.
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
