# Lab Report 2

## Part 1
Writing a web server that handles a URL request requires multiple steps. First, it is important to check the URL path for specific key words so that the program understands when to complete a certain function. In this example, the StringServer program looks for the specific keyword “/add-message”, and using .split, then stores the String that the user types in after the equals sign. It then adds that String to the stored String that includes all the previous Strings, and as a result is able to concatenate them and print them subsequently, as seen below. 

*Code:*

```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String stored = "";
    
    public String handleRequest(URI url) {
        if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                stored = stored + String.format(parameters[1] + "\n");
                return stored;
            }
        else{
            return "404 Not Found!";
        }
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);
        Server.start(port, new Handler());
    }
}
```

*Compiling and running the program in the terminal, and ensuring that a unique port number is used:*

```
ziadbanany@Ziads-MacBook-Air StringServer % javac Server.java StringServer.java 
ziadbanany@Ziads-MacBook-Air StringServer % java StringServer 4111
Server Started! Visit http://localhost:4111 to visit.
```

*Below are screenshots of adding different Strings to the URL, which are then concatenated and printed out in succession, followed by a brief description for each:* 

![Image](labreport2ss1.png)

- Here, the `main` method is called, and since `args[0]` is not empty, it declares the port number as 4111. It then starts the server and launches a new handlerRequest. 
- For this code the `handleRequest(URI url)` method is called. Since the URL contains the specific keyword, it then enters the conditional statement, and performs the task of printing out the String. 
- The relevant argument to this method is the URL, which through `url.getPath()` is checked for the keywords. 
- Also, an Array is initialized to include the String after the split mark. The value of the first index in this array, which is what the user types in after the equals sign, is then added to the String Array, and returned. 
- No values get changed because this is still the first request, thus the stored String is empty. 

![Image](labreport2ss2.png)

- The `main` method is first called, and the aforementioned order of operations is performed. The value of port is not changed, but a new `Handler()` is called.
- This calls the same `handleRequest(URI url)` method with the URL taken in as the parameter. Since, it also contains the key word `“/add-message”`, it enters the conditional statement and performs the same task. 
- Here, the value of `parameters[1]` is changed, because this is a new request, and so it is now `“hello \n how are you”` instead of `“hello”`. 
- Similarly, the value of the String stored is also changed because it is concatenated with the new request, and is no longer one single entry. 


## Part 2

The buggy program that will be analyzed is that related to the implementation of the reversed method in ArrayExamples.java. This method is meant to take in an Array, and return the Array in reversed order. For example the following Array: {1,2,3} should be reversed and returned as {1,2,3}. To further understood the characteristics that make this method buggy, it is important to look at the code, test out different inputs and discover which symptons result from failure-inducting inputs. 

The original, unedited buggy code for the reverse method in ArrayExamples.java is seen below. 

```
static int[] reversed(int[] arr) {
   int[] newArray = new int[arr.length];
   for(int i = 0; i < arr.length; i += 1) {
     arr[i] = newArray[arr.length - i - 1];
   }
   return arr;
 }
```

A failure-inducing input is an input, in this case an Array, that when run through the method causes a failure, meaning it does not return the expected result. The failure-inducing input for the buggy program that we will test is an int array of three elements {1,2,3}. The JUnitTest for that is seen below:

```
@Test
 public void reverseTester(){
   int[] arr = {1,2,3};
   int[] reversedArr = {3,2,1};
   assertArrayEquals(reversedArr, ArrayExamples.reversed(arr));
 }
 ```
 
 The expected outcome of this input is the reversed version of {1,2,3} which is {3,2,1}. However, this induces a failure, as seen below by the JUnitTest results. 


```
There was 1 failure:
1) reverseTester(ArrayTests)
arrays first differed at element [0]; expected:<3> but was:<0>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.reverseTester(ArrayTests.java:24)
        ... 32 trimmed
Caused by: java.lang.AssertionError: expected:<3> but was:<0>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 38 more

FAILURES!!!
Tests run: 5,  Failures: 1
```

The symptom of this that was revealed was that the first element of the returned array was actually 0, not what it is supposed to be which is 3, and this is shown in the failure report above, where it says `arrays first differed at element [0]; expected:<3> but was:<0>`. 

An input however that does not induce any failure is an array with the one element {0}. Below is the jUnitTest code for that. 

```
@Test
 public void reverseTester(){
   int[] arr = {0};
   int[] reversedArr = {0};
   assertArrayEquals(reversedArr, ArrayExamples.reversed(arr));
 }
 ```
 
As you can see below in the jUnitTest results, this did not cause any failures:

```
.....
Time: 0.004

OK (5 tests)
```

Based on the symptom revealed which is that the first element is always 0, the following observations were made. First, it is not possible for that to be a viable result since 0 was not even one of the elements in the failure-inducing input array. However, it is known that the default value of an undeclared element in any array is 0. Thus, it became clear that possibly the printed elements are yet to be declared.

By looking at the code, it became evident that the bug was that it was incorrectly assigning values from newArray to arr, as seen in the below line:

```
arr[i] = newArray[arr.length - i - 1];
```

Since newArray elements are yet to be declared, this was setting every element in arr to just the default value which is 0. Instead, the code should be assigning values from arr to newArray and then returning newArray, not arr. Thus, the before and after code look as follows:

*Before code:*

```
static int[] reversed(int[] arr) {
   int[] newArray = new int[arr.length];
   for(int i = 0; i < arr.length; i += 1) {
     arr[i] = newArray[arr.length - i - 1];
   }
   return arr;
 }
 ```
 
 *After code:*
 
 ```
 static int[] reversed(int[] arr) {
   int[] newArray = new int[arr.length];
   for(int i = 0; i < arr.length; i += 1) {
     newArray[i] = arr[arr.length - i - 1];
   }
   return newArray;
 }
 ```

This evidently addresses the issue because, as described above, it assigns the correct values from the correct array and returns the correct array. 

## Part 3

I learned a lot from the labs of Week 2 and Week 3 that I did not know before. Particularly, I learned the how to setup a server with a unique fork, and the way in which a program is capable of understanding the user’s input in a URL. I was introduced to new libraries such as java.net.URI and new functions such as URL.getPath() and URL.getQuery(). As a result, I learned how a programmer can manipulate the input URL to perform certain tasks, and how reading the URL and looking for specific keywords is the key for the computer to understand what to do next. This was made clear in the distinctions between /add and /search. As a result, I gained a better understanding of the functioning of URLs and the role they play in running servers. 
