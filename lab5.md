# Lab 5 (Putting It All Together)

# Debugging Scenario

## Student Post
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/3eb63f67-26d2-43b9-9226-4d898718ca81)


Student name: Cinnamaroll <br>
Post Subject: "HELP MY GRADING SCRIPT IS BROKEN :(" <br>

"I don't know what's wrong with my ```grade.sh``` script! It spit out a lot of output that I can barely make sense of. It seems like JUnit isn't being recognized or something, but I just used the same code as what the professor showed in class. I ran the command ```bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-lab3```" in the working directory ```./list-examples-grader```."


![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/5ba9a925-5b39-4dda-8271-0c171546ac59)
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/dc71f207-331a-4dab-9129-590d2a28bc74)
<br>

## TA Response

![download-photoaidcom-cropped](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/737b67d6-db1c-434e-8171-212fd5bda3b8) <br>
TA name: Detective Pikachu [STAFF] <br>
Post Subject: "RE:HELP MY GRADING SCRIPT IS BROKEN :(" <br>

"Thank you for providing screenshots of the symptoms, there seems to be a few things going on at once, so we can try to break them down one by one. First off, make sure your path to JUnit is cofrect. Are you on a Windows machine by any chance? Remember, there are two different ways to run JUnit depending on your operating system, refer to the course website on [Week 6](https://ucsd-cse15l-w24.github.io/week6/index.html) to double check if you're running the right one."
Another thing I am noticing in your output is that your ```git clone``` is failing. Look a little closely at the output message that ```git clone``` is giving. Why might it be failing?"
<br>

## Student Response
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/163e836e-78c0-49da-8cf4-ec2a91773075)


Student name: Cinnamaroll <br>
Post Subject: "RE:RE:HELP MY GRADING SCRIPT IS BROKEN :(" <br>

"That seems to have fixed a lot, thank you! I didn't realize that ```git clone``` would fail if the target directory was not empty, I thought it would just overwrite the existing files! I simply added ```rm -rf student-submission``` before the ```git clone``` to make sure it's empty. I've run into a different issue now though. When testing with ```https://github.com/ucsd-cse15l-f22/list-methods-compile-error```, which should be detected an a compile error, my script doesn't handle it properly and runs as if it had compiled normally. I ran the command ```bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-compile-error```" in the working directory ```./list-examples-grader```."

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/cf505492-4d43-467c-bf49-61e7adb1957e)

"The correct output should be:"
```
Compilation Error
Score:0
```

"This is what my conditional looks like:"
```
if [ $1 -ne 0 ]
then
    echo "Compilation Error"
    echo "Score:0"
    exit 1
fi

```

## TA Response

![download-photoaidcom-cropped](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/737b67d6-db1c-434e-8171-212fd5bda3b8) <br>
TA name: Detective Pikachu [STAFF] <br>
Post Subject: "RE:RE:RE:HELP MY GRADING SCRIPT IS BROKEN :(" <br>

"Thank you for providing the conditional! I see that you're using ```$1``` in your if statement. ```$1``` refers to the first argument that is passed in from the command line. Try looking into how we can grab exit codes from previously ran lines. Also it seems like your output isn't being redirected, make sure to double check your redirection syntax. Please refer to the course website on [Week 6](https://ucsd-cse15l-w24.github.io/week6/index.html) for this."

## Student Response
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/bb0b7b65-426c-44a8-9b52-7d563748a490)


Student name: Cinnamaroll <br>
Post Subject: "RE:RE:RE:RE:HELP MY GRADING SCRIPT IS BROKEN :(" <br>

"Oh oops, thank you. All I did to fix it was change ```$1``` to ```$?```"

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/6f18cb1f-2887-4039-a6bc-14998eb9dee9)

"There is one more problem, when I run ```bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-lab3``` in the working directory ```./list-examples-grader```, it takes a long time to run."

![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/5d6cb536-c6c8-4ca9-87fe-99b6bbe765b1)

"The JUnit output is the following:"
```
1) testMergeRightEnd(TestListExamples)
java.lang.OutOfMemoryError: Java heap space
	at java.base/java.util.Arrays.copyOf(Arrays.java:3513)
	at java.base/java.util.Arrays.copyOf(Arrays.java:3482)
	at java.base/java.util.ArrayList.grow(ArrayList.java:237)
	at java.base/java.util.ArrayList.grow(ArrayList.java:244)
	at java.base/java.util.ArrayList.add(ArrayList.java:483)
	at java.base/java.util.ArrayList.add(ArrayList.java:496)
	at ListExamples.merge(ListExamples.java:42)
	at TestListExamples.testMergeRightEnd(TestListExamples.java:18)
	at java.base/java.lang.invoke.LambdaForm$DMH/0x0000021f17012400.invokeVirtual(LambdaForm$DMH)
	at java.base/java.lang.invoke.LambdaForm$MH/0x0000021f17013000.invoke(LambdaForm$MH)
	at java.base/java.lang.invoke.Invokers$Holder.invokeExact_MT(Invokers$Holder)
```
"Is this normal? I don't remember the professor having this kind of output in lecture."

## TA Response

![download-photoaidcom-cropped](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/737b67d6-db1c-434e-8171-212fd5bda3b8) <br>
TA name: Detective Pikachu [STAFF] <br>
Post Subject: "RE:RE:RE:RE:RE:HELP MY GRADING SCRIPT IS BROKEN :(" <br>

"It's hard to say without being able to see your JUnit file, but the repository you are cloning is meant to have an infinite loop. JUnit has a way to limit this kind of behavior when it is expected, here is the documentation for [JUnit Timeouts](https://junit.org/junit5/docs/current/user-guide/#writing-tests-declarative-timeouts)."

## Student Response
![image](https://github.com/lov-daniel/cse15l-lab-reports/assets/83891229/bb0b7b65-426c-44a8-9b52-7d563748a490)

Student name: Cinnamaroll <br>
Post Subject: "RE:RE:RE:RE:RE:RE:HELP MY GRADING SCRIPT IS BROKEN :(" <br>

"Ahh, I see, okay I remember this from lecture! Everything works as expected now :) Thank you! I simply added ```(timeout = 500)``` to the end of every ```@Test``` in the ```TestListExamples.java```"
```
Time: 0.559
There were 2 failures:
1) testMergeRightEnd(TestListExamples)
org.junit.runners.model.TestTimedOutException: test timed out after 500 milliseconds
	at ListExamples.merge(ListExamples.java:43)
	at TestListExamples.testMergeRightEnd(TestListExamples.java:18)
```
"I ran ```bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-lab3``` in the working directory, ```./list-examples-grader``` and this looks just like what the professor had in lecture."

## File/Directory Setup
```
C:\USERS\DLOV\DESKTOP\CSE15L\LIST-EXAMPLES-GRADER
│   error-output.txt
│   grade.sh
│   grading-area
│   junit-output.txt
│   TestListExamples.java
│
├───lib
│       hamcrest-core-1.3.jar
│       junit-4.13.2.jar
│
└───student-submission
        ListExamples.java
```

## ```grade.sh``` Setup
```
CPATH='.:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java'
mkdir grading-area
git clone $1 student-submission
echo 'Finished cloning'
if [ -f student-submission/ListExamples.java ]
then
    cp student-submission/*.java grading-area
    cp TestListExamples.java grading-area
    cp -r lib grading-area
else
    echo "Missing student-submission/ListExamples.java, did you forget the file or misname it?"
    echo "Score: 0"
    exit 1
fi
cd grading-area
javac -cp $CPATH *.java >> error-output.txt

if [ $1 -ne 0 ]
then
    echo "Compilation Error"
    echo "Score:0"
    exit 1
fi

java -cp $CPATH org.junit.runner.JUnitCore TestListExamples > junit-output.txt

lastline=$(cat junit-output.txt | tail -n 2 | head -n 1)
check=$(echo $lastline | cut -c1-2)

if [ "$check" = "OK" ]
then
    tests=$(echo $lastline | awk -F'[( ]' '{print $3}')
    successes=$(echo $lastline | awk -F'[( ]' '{print $3}')
else
    tests=$(echo $lastline | awk -F'[, ]' '{print $3}')
    failures=$(echo $lastline | awk -F'[, ]' '{print $6}')
    successes=$((tests - failures))
fi
echo "Your score is $successes / $tests"
```

## ```TestListExamples.java``` setup
```
import static org.junit.Assert.*;
import org.junit.*;
import java.util.Arrays;
import java.util.List;
import java.util.ArrayList;

class IsMoon implements StringChecker {
  public boolean checkString(String s) {
    return s.equalsIgnoreCase("moon");
  }
}

public class TestListExamples {
  @Test
  public void testMergeRightEnd() {
    List<String> left = Arrays.asList("a", "b", "c");
    List<String> right = Arrays.asList("a", "d");
    List<String> merged = ListExamples.merge(left, right);
    List<String> expected = Arrays.asList("a", "a", "b", "c", "d");
    assertEquals(expected, merged);
  }

  @Test
  public void testMergeLeftEnd() {
    List<String> left = Arrays.asList("a", "b", "d");
    List<String> right = Arrays.asList("a", "c");
    List<String> merged = ListExamples.merge(left, right);
    List<String> expected = Arrays.asList("a", "a", "b", "c", "d");
    assertEquals(expected, merged);
  }

  @Test
  public void testFilter() {
    List<String> fruits = Arrays.asList("bananas", "apples");
    IsMoon check = new IsMoon();
    List<String> fruits_without_n = ListExamples.filter(fruits, check);
    assertEquals("apples", fruits_without_n.get(0));
  }
}
```

# Reflection

During this second half of the quarter, I think the most important thing that I have learned is how to edit files without having to use a mouse. At first it was extremely difficult, but as I used it more and more, I found it very convenient to be able to keep my hands on the keyboard at all times.
