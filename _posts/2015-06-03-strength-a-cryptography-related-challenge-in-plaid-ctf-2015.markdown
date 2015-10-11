---
title:  "Strength - a cryptography related challenge in PlaidCTF 2015"
date:   2015-06-03 11:20:01
description: Solving the strength challenge in PlaidCTF 2015
---

I recently took part in [PlaidCTF](https://ctftime.org/event/185) organized by the folks at [Plaid Parliament of Pwning](https://ctftime.org/team/284), CMU.

This was right before my semester end terms, so I wasn't expecting a lot of people to participate with me. At the end of the competition, our team was able to solve **4 challenges**. We also managed to hack another challenge, but we could grab the flag only past the competition end time.

Almost the entire time, I was trying to solve the **[PNG Uncorrupt](https://github.com/ctfs/write-ups-2015/tree/master/plaidctf-2015/forensics/png-uncorrupt)** challenge. They crafted a corrupt PNG image, which we were supposed to uncorrupt, to recover the original image and hence the flag. I learned all about the internal structure of different image formats while attempting this challenge.

My friend [Rohan](https://twitter.com/smartyrohan12) was looking into other challenges at the same time. Near the end of the competition we stumbled upon a cryptography related challenge named **strength**. It is based on the **RSA cryptosystem**. My area of knowledge, so I took a shot.

Make sure you grok a [primer on number theory](http://jeremykun.com/2011/07/30/number-theory-a-primer/ "Number Theory, a primer") and the **[RSA cryptosystem](https://en.wikipedia.org/wiki/RSA "Wikipedia page for RSA")** before you read ahead.

The challenge has a [text file](https://github.com/ctfs/write-ups-2015/blob/master/plaidctf-2015/crypto/strength/captured_827a1815859149337d928a8a2c88f89f) with 20 tuples, each with three elements: the modulus **N**, public key exponent **e** and the cipher text **c**.

![](/assets/images/content/strength_txtfile.png)

First observation pointed out the use of a common modulus **N** for all the encrypted messages. The exponent **e** varied for each encryption.

The **RSA cryptosystem** is inherently prone to attacks.

**Why?**
Because it is based on certain mathematical and computational assumptions. Like **factoring** is computationally difficult, that computers can generate an unpredictable sequence of **random** numbers.

Yes, it sure is hard, but to what extend and with what quantitative limits?

On searching for possible vulnerabilities in such a scheme, I found something interesting. Using two different public key exponents (`e1` and `e2`) without a common prime factor, and a common modulus `n`, makes the recovery of the original message possible.

**How?**

_Pause here and see if you can figure out!_

Moving on, this is how the message **m** can be recovered:

The cipher texts **c1** and **c2** corresponding to the two public key exponents (**e1** and **e2**) would have been calculated as:

![](/assets/images/content/c_1_equals_m_power_e1_mod_n.png)

![](/assets/images/content/c_2_equals_m_power_e2_mod_n.png)

By bringing in the assumption that `gcd(e1, e2) = 1`, we can leverage Euclid's extended algorithm to recover the original message **m**

Leveraging Euclid's extended algorithm, we have

![](/assets/images/content/e_1_mul_a_plus_e_2_mul_b.png)

in this case either `a` or `b` will be negative (see if you can figure out why).  
Without loss of generality assume **a** to be negative

so if `gcd(e1, e2)` is equal to 1, we have

![](/assets/images/content/i_equals_c_1_pow_minus_1.png)  
(modular inverse of **c1** w.r.t. **n**)

![](/assets/images/content/m_equals_c_1_pow_a_mul.png)  
(substitute **c1** and **c2** in this equation to see why this equation is valid)

finally, substituting with the modular inverse of `c1`, the equation becomes

![](/assets/images/content/m_equals_i_pow_min_a.png)

Now, in this equation, the value of every variable is know:

* **a** and **b** can be calculated using Euclid's extended algorithm
* **c2** is known
* **i** can be calculated as the modular inverse of **c1** w.r.t. **n**

The numeric message **m** can be converted back to ASCII text by using the following code in Python (removes the **0x** in the beginning).

{% highlight python %}
hex(m)[2:-1].decode("hex")
{% endhighlight %}

So basically, I iterated over all the possible pairs of public key exponents `example: (e1, e2)` in the given file, to see if any of the pair satisfied the condition `gcd(e1, e2) = 1`.

One of the pair satisfies the condition, and I used the maths briefed above to recover the original message **m**.

References: [RSA cracking: The same message is sent to two different people problem](http://crypto.stackexchange.com/questions/1614/rsa-cracking-the-same-message-is-sent-to-two-different-people-problem)
