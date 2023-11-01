# How do Tests Work?
## Let's Dive in Before We Execute!
Most tests start by passing `() -> {run(...); assertThat(...);}` as the first argument. While `assertThat` is a function from assertJ which we can understand, what does run do?
![|300](https://i.imgur.com/IC4bFRb.png)
![|500](https://i.imgur.com/gX1PHpg.png)
Hmm! It puts the input strings into `System.in`, that is, the input stream, and then it executes `runMain`. Our defined `runMain` is as follows:
![](https://i.imgur.com/q5lTDkX.png)
So, it’s about putting input and running the main function! Now let's get into the details.

---
## Let's Understand Mockito
### Why Use It? 🧐
> [!NOTE] It is an object created by the programmer to manage behaviors directly.
> So, for difficult testing scenarios (like when we use the Random function), we use a **fake object** called **Mock** to replicate the situation.

### Where is it Used?
![](https://i.imgur.com/cazwT7I.png)
This function is called within our test function `assertRandomNumberInRangeTest`! If we look at the conditional in the try statement, we can see the Mock object we were looking for.
![](https://i.imgur.com/rD8P87U.png)
1. If we run `assertRandomNumberInRangeTest(...)`, the first argument is for Verification to check if the **`pickNumberInRange()`** function was executed.
2. Afterwards, it executes `assertTimeoutPreemptively(...)` which carries out the test for a duration of RANDOM_TEST_TIMEOUT.
3. Then, the try statement applies a `MockStatic` type to `Randoms.class` and returns the result of `mockStatic()`. If we look more closely at the official documentation of `MockStatic`, it states:
   > Represents an active mock of a type's static methods. The mocking only affects the thread on which this static mock was created and it is not safe to use this object from another thread.
   
   Let's now look at the official document of `mockStatic`.
   > Creates a thread-local mock controller for all static methods of the given class or interface.
   > In other words, it creates a thread-local mock controller for the given class or interface.
   
4. `mock.when(verification).thenReturn(value, Arrays.stream(values).toArray())` means that when the verification is met, the mock object will return `value`, `values` during execution.
5. ![](https://i.imgur.com/DEHSVkm.png)
   Shall we look at an example?
   In this case, after setting the `Randoms` Mock object to return `MOVING_FORWARD` and `STOP`, it sets the input as "pobi,woni", "1" and runs the `main` function. The result must include the values within `contains()`.

---
# Review!
> [!NOTE] Indeed, technology thrills people
> I was thrilled to dissect new code..! Especially, I was curious about the code from Woowa Brothers, and it certainly didn't disappoint 🥰. I'll strive to become a better programmer and will add more posts if I get a chance to use other test methods.