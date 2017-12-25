# The Extended Euclidean Algorithm Explained

As a PhD student at EURECOM (https://www.eurecom.fr)
I also supervise some course labs as a teaching assistant.
One of the labs I supervise is about RSA encryption
([Wikipedia:RSA (cryptosystem)](https://en.m.wikipedia.org/wiki/RSA_(cryptosystem))).
RSA is great to teach the basics of cryptography because
it's a simple cryptosystem that is widely used in the real world.
However, there is one part at the beginning of the lab where many students
are struggling:
implementing the *extended euclidean algorithm*
([Wikipedia: Extended Euclidean algorithm](https://en.m.wikipedia.org/wiki/Extended_Euclidean_algorithm)).

In this post I will not cover why this algorithm is useful
(but it is, and not only for RSA!).
Instead, I will try to give an intuitive explanation of it,
showing how it can be found and written with very basic mathematics and simple intuition.
Computer code will either be Python code
(https://www.python.org/)
or pseudo-code mimicking Python. 

The Extended Euclidean algorithm
is an algorithm that computes the Greatest Common Divisor (GCD) of two numbers.
The GCD of two numbers A and B
(we're talking about *integers*, so "whole" numbers without a decimal part:
1, 2, 3, 42, 123456789 &hellip;)
is the greatest number that divides both A and B.
For instance the divisors of 42 are 1, 2, 3, 6, 7, 14, 21 and 42
and the divisors of 30 are 1, 2, 3, 5, 6, 10, 15, and 30
(you can get the divisors of any number using
[sympy gamma](http://gamma.sympy.org/input/?i=divisors%2842%29)
or [wolfram alpha](https://www.wolframalpha.com/input/?i=divisors+of+42)),
so the greatest common divisor of 42 and 30 is 6.

But the extended Euclidean algorithm gives you more than
just the GCD of two numbers
(the "normal" Euclidean algorithm is the one that only gives the GCD).
There is a theorem called "Bézout's lemma"
([Wikipedia: Bézout's identity](https://en.m.wikipedia.org/wiki/B%C3%A9zout%27s_identity))
that says the following:

> ### Bézout's Lemma
> The GCD of A and B is the minimum
> combination X*A + Y*B (positive and non-null)
> for all integers X and Y;

The extended Euclidean algorithm,
on top of giving the GCD,
gives some coefficients X and Y
such that gcd(A, B) = X*A + Y*B.

## Brainstorming

Let us try to find the X and Y for A = 120 and B = 23.
One way to do it would be to compute the GCD using the normal euclidean algorithm
and then to try a huge number of different combinations of A and B
until we find the one that is equal to the GCD (the GCD of 120 and 23 is 1).

    In [1]: 4*120 + 5*23
    Out[1]: 595

    In [2]: # Wow that was way too big.
       ...: # We're going to need one positive coefficient and a negative one
       ...: 2*120 - 7*23
    Out[2]: 79

    In [3]: # much smaller !
       ...: # We can still remove 23 a few times to make it smaller
       ...: 2*120 - 8*23
    Out[3]: 56

    In [4]: 2*120 - 10*23
    Out[4]: 10

    In [5]: # Now that's rather small, we're getting better!

    In [6]: 2*120 - 11*23
    Out[6]: -13

    In [7]: # It's OK to have a negative number
       ...: # because I can easily get the combination with a positive value

    In [8]: -2*120 + 11*23
    Out[8]: 13

    In [9]: # but it's greater than 10 so it's not interesting.
       ...: # What should we try now?

    In [10]: 3*120 - 11*23
    Out[10]: 107

    In [11]: 3*120 - 15*23
    Out[11]: 15

    In [12]: 3*120 - 16*23
    Out[12]: -8

    In [13]: - 3*120 + 16*23
    Out[13]: 8

    In [14]: # OK, even smaller...
        ...: # but how much time is this going to take me until I find the right combination?
        ...:

## The Trick

Let's be smarter than that.

The sequence `2*120 - 7*23, 2*120 - 8*23, 2*120 - 9*23 ...`
should remind you of something:
This is how we learn the Euclidean division
(or "division with a remainder")
when we are kids.
We learn to do the division of A by B
by trying A - B, A - 2B, A - 3B...
until "we cannot subtract any more Bs".
More formally, doing to euclidean division of A by B consists in finding Q and R such that

    A = QB + R
    and R < B

Now this R looks very interesting, because:

- it is a combination of A and B (R = A - QB)
- it cannot be too big because it has to be lesser than B

However we don't want a "rather small" combination of A and B,
we want *the smallest one*.
How do we get there?

Linear combinations (XA + YB is a linear combination)
have this interesting property:
If R1 and R2 are linear combinations of A and B,
then any linear combination of R1 and R2 is also a linear combination of A and B:

    Let
    R1 = X1*A + Y1*B
    R2 = X2*A + Y2*B

    Then,
    C*R1 + D*R2 = C*(X1*A + Y1*B) + D*(X2*A + Y2*B)
                = (C*X1 + D*X2)*A + (C*Y1 + D*Y2)*B

So we have this R that is a linear combination of A and B
and which is smaller than B, thanks to the properties of the Euclidean division...
The magic happens when we divide B by R;
We get Q2 and R2 such that:

    B = Q2*R + R2
    R2 < R

R2 is even more "interesting" than R:
first it *has* to be smaller than R,
and it is still a linear combination of A and B
because it is a combination of B and R.
Namely:

    R2 = B - Q2 (A + QB)
       = -Q2 A + (1 - Q2 Q) B

We can repeat this trick again and again
to find ever smaller combinations of A and B...
but it has to stop at some time,
because the remainder of the euclidean division will always be positive
so you cannot go "lower" than zero.
What will happens is that we will find `Ri = 0` for some i.
The GCD cannot be zero (zero does not divide anything)
so our R(i-1) will be our best candidate,
it will be "the smallest combination we found".

Does that mean that we're done?
Actually we are,
R(i-1) is the GCD of A and B
and the coefficients corresponding to it are the ones we were looking for.
Now we need to prove that this is not only the smallest number we could get with our trick,
but that it is also the smallest combination possible. 

## Proof of Correctness

One way to prove that R(i-1) = gcd(A,B)
is to remember a property of the GCD
that is central to the normal euclidean algorithm:

    gcd(A + MB) = gcd(A, B) for all M

As a consequence we have:

    gcd(R1, B)  = gcd(A - Q1 B, B)   = gcd(A, B)
    gcd(R1, R2) = gcd(R1, B - Q2*R1) = gcd(R1, B) = gcd(A, B)
    etc...

And thus:

    gcd(R(i-1), Ri) = gcd(A, B)
    gcd(R(i-1), 0)  = gcd(A, B)
    R(i-1)          = gcd(A, B)
    (Since gcd(X,0) = X for all X)

## The algorithm

The algorithm is going to be mainly a loop
that iterates until we find a GCD equal to zero.
To find the variables to keep in memory in the loop,
we compute one of the combinations
and we keep note of the variables we need during the process.
Say that we already have:

    3 = -4*120 + 21*23

That is R2 and its two coefficients X2 = -4 and Y2 = 21
and we want the next linear combination, that is R3 and its coefficients.
We get R3 (and Q3) by dividing R1 by R2:

    R1 = R2*Q3 + R3

Or in a more "programatic" style:

    Q3 = R1 // R2
    R3 = R1 %  R2

This step requires to have R1 and R2 in memory.
Then we want to compute the coefficients X3 and Y3 for R3:

    R3 = R1 - Q3*R2
    R3 = X1*A + Y1*B - Q3*(X2*A + Y2*B)
    R3 = (X1 - Q3*X2)*A + (Y1 - Q3*Y2)*B

Again the code will look like:

    X3 = X1 - Q3*X2
    Y3 = Y1 - Q3*Y2

For this step we need the coefficients for both R1 and R2,
and we also need Q3 but it was computed in the previous step from R1 and R2.

Summing up,
at step n we need the values of R(n-1) and R(n-2)
as well as their coefficients as combinations of A and B.

Now we need to think about the initial values of these variables before we start the loop;
we could "cheat" by doing the first two steps (R1 and R2) before we enter the loop,
but it's not as elegant.
It suffices to notice that A and B can trivially be expressed as combinations of A and B:

    A = 1*A + 0*B
    B = 0*A + 1*B

and that the computation of R1 from A and B follows the general rule we described above:

    recall:
    A = Q1*B + R1
    R1 = A - Q1*B

    thus:
    Q1 = A // B
    R1 = A % B
    X1 = 1 - Q1*0
    Y1 = 0 - Q1*1

So here is the algorithm we obtain:

    def extended_gcd(a, b):
        '''returns (g, x, y) where
        g is the GCD of a and b
        and a*x + b*y = g'''

        # a = 1*a + 0*b
        (r1, x1, y1) = (a, 1, 0)
        # b = 0*a + 1*b
        (r2, x2, y2) = (b, 0, 1)

        while r2 != 0:
            # dividing r1 by r2 to get r3:
            # r1 = q3*r2 + r3
            q3 = r1 // r2
            r3 = r1 % r2
            
            # r3 = r1 - q3*r2
            #    = x1*a + y1*b - q3*(x2*a + y2*b)
            #    = (x1 - q3*x2)*a + (y1 - q3*y2)*b
            x3 = (x1 - q3*x2)
            y3 = (y1 - q3*y2)
            
            # next step r1 will be our current r2
            # and r2 will be our current r3
            (r1, x1, y1) = (r2, x2, y2)
            (r2, x2, y2) = (r3, x3, y3)
            
        return (r1, x1, y1)

## Optimizations?

There are a few "improvements" we could think about,
but I would rather keep the code as it is now.

### Handling A smaller than B

At the beginning we assumed that A was greater than B;
do problems arise when it's not the case?

The answer is no.
If B is greater than A,
the dividing A by B will give:

    A = 0*B + A

So at the next step we will be dividing B by A and the algorithms works as expected.

### Removing temporary variables

Instead of creating variables `r3, x3, y3`

Variables `r3, x3, y3` are only here as temporary variables.
Their purpose is to be stored in `r2, x2, y2` at the end of the iteration.
The reason why we do not directly assign `r2, x2, y2` to their new value
is because we need to remember their previous value
to store them in `r1, x1, y1`.
However we cannot do this either ahead of time
because we need the previous values of `r1, x1, y1` for the computation of `r3, x3, y3`.

The Python language provides a workaround with *multiple assignement*,
but I think it makes the code less readable:
it seems clearer to say that *we compute a new combination* first and *we discard the oldest combination* second.
