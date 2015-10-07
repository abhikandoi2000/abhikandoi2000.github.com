---
title:  "A cryptography related challenge named strength"
##date:   2015-06-03 11:20:01
description: Solving the strength challenge in PlaidCTF 2015
---

I recently took part in [PlaidCTF](https://ctftime.org/event/185) organized by the folks at [Plaid Parliament of Pwning](https://ctftime.org/team/284), CMU.

This was right before my semester end terms, so I wasn't expecting many people to participate with me. At the end of the competition, our team was able to solve **4 challenges**. We also managed to hack another challenge but we grabbed the flag only past the competition end time.

Almost the entire time, I was trying to solve the **PNG Uncorrupt** challenge. They had crafted a corrupt PNG image which we were supposed to uncorrupt, to recover the original image and hence the flag. I learned all about the internal structure of different image formats while attempting this challenge.

My friend [Rohan](https://twitter.com/smartyrohan12) was looking into other challenges at the same time. Near the end of the competition we stumbled upon a cryptography related challenge **strength**. It is based on the **RSA cryptosystem**, my area of knowledge, so I took a shot.

Make sure you grok a primer on number theory and the **RSA cryptosystem** before you read ahead.

The challenge had a [text file](https://github.com/ctfs/write-ups-2015/blob/master/plaidctf-2015/crypto/strength/captured_827a1815859149337d928a8a2c88f89f) with 20 tuples, each with three elements: the modulus **N**, public key exponent **e** and the cipher text **c**.

![](/assets/images/content/strength_txtfile.png)

First observation pointed out the use of a common modulus **N** for all the encrypted messages. The exponent **e** varied for each encryption.

The **RSA cryptosystem** is inherently prone to attacks.

Why?
Because it is based on certain mathematical and computational assumptions. Like **factoring** is computationally difficult, that computers can generate sequence of **random** numbers, which is unpredictable.

Yes, it sure is hard, but to what extend and with what quantitative limits?

On searching for possible vulnerabilities in such a scheme, I found something interesting. Using two different public key exponents(`e1` and `e2`) without a common prime factor, and a common modulus `n`, makes the recovery of the original message possible.

How? **Pause here and see if you can figure out!**

Moving on, this is how the message **m** can be recovered:

The cipher texts **c1** and **c2** corresponding to the two public key exponents(**e1** and **e2**) would have been calculate as below:

```c1 = m ^ e1 mod n```
`c2 = m ^ e2 mod n`

Bringing in the assumption of `gcd(e1, e2) = 1`, we can leverage Euclid's extended algorithm

Leveraging Euclid's extended algorithm, we have

`e1 * a + e2 * b = gcd(e1, e2)`

in this case either `a` or `b` will be negative (see if you figure out why)
Without loss of generality assume **a** to be negative

so if `gcd(e1, e2) == 1`, we have

`i = c1 ^ (-1) mod n` (modular inverse of **c1** w.r.t. **n**)

`m = ((c1 ^ a) * (c2 ^ b)) mod n` (substitute **c1** and **c2** in this equation for clarity)

finally, substituting by the modular inverse of `c1`, we have

`m = (i ^ -a) * (c2 ^ b) mod n`

The numeric message `m` can be converted back to ASCII text by using `hex(m)[2:-1].decode("hex")` in Python.

So basically, I iterated over all the possible pairs of public key exponents `example: (e1, e2)` in the given file, to see if any of the pair satisfied the condition `gcd(e1, e2) = 1`.

One of the pair satisfied the condition, and I used the maths briefed above to recover the original message `m`.

References: [RSA cracking: The same message is sent to two different people problem](http://crypto.stackexchange.com/questions/1614/rsa-cracking-the-same-message-is-sent-to-two-different-people-problem)
