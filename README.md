# CS_312_Project_1
Primality Tester
Project 1: Primality Test
In this project you will implement all the mechanics behind generating an RSA public-private keypair.

Background
There are two parts to this project.

First you will implement the specified functions in fermat.py. This file focuses on the task of determining whether a number is prime or composite.

Second you will implement the specified functions in rsa.py. This file focuses on the task of generating a valid public-private keypair.

Instructions
Download the provided code:
project1.zipDownload project1.zip
Make sure you have pytest installed (conda install pytest or pip install pytest in your python environment)
Make sure you have byu-pytest-utils installed (pip install byu-pytest-utils)
Implement all the specified functions in fermat.py
Make sure you pass all the tests in test_fermat.py
Implement all the specified functions in rsa.py
Make sure you pass all the tests in test_rsa.py
Write a report as desribed below
Submit your source code and report to Gradescope
fermat.py
Implement modular exponentiation (pseudo-code in Figure 1.4 on p. 19). Your primality test should use your modular exponentiation function and should work properly for numbers as large as 1073741824.
Code up the Fermat primality test pseudocode from Figure 1.8 of the text. You may set 
 to any value you like (see p. 27). This value indicates how many random trials (values of 
) are used.
Code the probability that 
 Fermat trials gave you the correct answer -- see the discussion between Figure 1.7 and Figure 1.8.
Implement the Miller-Rabin primality test. There is no pseudo-code in the book for this, but you can find what you need in the sidebar on p. 28 and in the discussion that follows.
Code the probability that 
 Miller-Rabin trials gave you the correct answer -- see the note in the sidebar on p. 28 and the discussion below.
The Miller-Rabin test
Let's say the number we are testing is 
; just as in the Fermat case, we choose a random test in the range \(1 \le a

as Fermat's theorem says it should. Since this is 
, if we take the square root, we would expect the result to be either 1 or -1. We can check this by computing

just as expected. But, since this is also 1, we can take the square root again by computing

which passed (still looks prime).

Continuing the sequence (purely for demonstration purposes; once we get a 
, we can stop the sequence knowing the number is prime), we can take yet another square root, but since we are now taking the square root of -1, we don't expect such nice behavior, and indeed, we get

We can complete the sequence with

and
and we can't divide the exponent by 2 anymore, so the sequence ends.

To summarize, what we are doing here is repeatedly taking the square root of a number that is $\equiv 1 \pmod N$. For a while, the result is, not unexpectedly, 
, but at some point it is 
. As we know, taking the square root of -1 is weird (though in this modular case that doesn't mean complex numbers; rather, we just get away from 1).

What Miller and Rabin showed is that for prime numbers, for all choices of 
, 
, this sequence of square roots, starting with a 
, will either consist of all 
, or, if at some point it changes to something else, that something else will always be 
, which is exactly what we saw in our example.

What they also showed was that for composites (including Carmichael numbers), for at least 3/4 of the possible choices for 
, this will not be the case—either the initial test will not equal 
, just as in the Fermat case, or, if it does, in taking the series of square roots, the first number to show up after the sequence of 1s will be something other than 
.

Here is another example: suppose 
 and we choose 
. Then, computing our sequence of square roots, we get

Notice that this time, that when we encountered something other than 
 in the sequence, it was also not 
.

This is common for composites, but never happens for primes.

Hopefully now it is becoming clear how we can implement an alternative to the Fermat-test-based primality testing algorithm with this new information—if our number 
 passes the initial test 
 (which is equivalent to a Fermat test, right?), we then compute this sequence of square roots, looking for the first result that is not 
. If there isn't one (because the entire sequence is 1s) or if the first such non-1 number is 
, 
 has passed one round of the Miller-Rabin test 
. We still don't know anything for sure (we can only demonstrate that a number is not prime vs is probably prime).

So we continue to choose another random 
 and repeat our test; however, if we instead find that there is a first result in the sequence that is neither 
, then we know the number is composite.

rsa.py
Implement the specified functions in rsa.py.

To generate an RSA public-private keypair:

Generate two random prime numbers, p and q
To generate a random prime number, generate a random number and check if it is prime; repeat until you find a prime number
use random.getrandbits(bits) to generate a random number of bits bits
use a primality testing function from fermat.py to see if it is prime (use k=20)
Select an e from the provided list of prime numbers that is relatively prime to (p-1)(q-1)
Compute d, the multiplicative inverse of e mod (p-1)(q-1)
Use the extended euclid algorithm discussed in class to test e and find d
Return N (i.e. p*q), e, and d
N and e are the public key
N and d are the private key
Report
Your PDF report should include:

[20 points] Explain the time and space complexity of your algorithm by showing and summing up the complexity of each subsection of your code. NOTE: to make this easy for the TAs to find, this should appear as a subsection of your report (that is,it is great to have this info in your code comments, but you should also pull it out and include it in the report proper).
[10 points] Discuss the equation you used to compute the probability 
 of correctness for both your Fermat and Miller-Rabin implementations.
Code
The autograder will award the following points:

Task	Points
Modular exponentiation	10
Fermat - prime numbers	5
Fermat - composite numbers	5
Miller-Rabin prime numbers	10
Miller-Rabin composite numbers	10
Generate an RSA keypair	20
There is no performance requirement for this project.

Note: if you pass the autograder tests, but your code does not follow the provided instructions, your score will be adjusted accordingly. For example, if you use a built-in library to implement steps you were instructed to implement yourself, you will not receive credit for passing the tests.

Submission
Submit your fermat.py, rsa.py and any additional files you wrote, along with your PDF report, to Gradescope.

 

Turn in a PDF of your work to Gradescope.
