---
title: COMP4109 Midterm 1 Practice
author: Findlay et al.
---

1. A symmetric-key encryption scheme is defined as a 5-tuple (P,C,K,E,D). Complete the definition.

Where:

- $P$ is plaintext space
- $C$ is ciphertext space
- $K$ is keyspace
- $E$ is a set of possible encryption rules $e_k \in E \forall k \in K$
- $D$ is a set of possible decryption rules $d_k \in D \forall k \in K$

$E_k$ and $D_k$ are defined such that $D_k(E_k(x)) = x \forall \text{plaintext } x$

2. Describe the shift cipher (Caesar cipher).

Define a key $k \in K$ where $K \equiv \mathbb{Z}_{|P|}$.
In other words, $k$ can be any integer mod the size of the plaintext space.
Then, simply shift all plaintext characters by $k$ mod the size of the plaintext space.

3. Explain one situation when the shift cipher gives you prefect security? (That is, you cannot determine the secret key with probability greater than 1/26.)

You would probably have to be encrypting nonsense. At the very least, no frequency analysis should yield
any keys that are more probable than others. This is hugely impractical; as soon as the caesar cipher is used for
anything practical, it completely breaks.

4. Briefly explain the following (giving a possible real-world example for each if you can):

	1. ciphertext-only attack
	2. known-plaintext attack
	3. chosen plaintext attack
	4. chosen ciphertext attack

- ciphertext-only attack
    - adv. is given $c$ = $E_k(m)$
    - but they do not know $m$ or $E_k$
- known-plaintext attack
    - adv. knows one or more pairs $c_i,m_i$
- chosen plaintext attack
    - choose $m$ and given $c$
- chosen ciphertext attack
    - choose $c$ and given $m$

5. What needs to be specified in a security model?

- level of security for a security goal with respect to a specific attack model

6. A symmetric-key encryption scheme is **semantically secure** if `Pr(m) = Pr(m|c)`. Explain what this means.

- This means the probability of guessing the original message is the same as the probability of guessing
the original message given the ciphertext.
- In otherwords, a third party should not be able to determine
*any* additional information about the plaintext (other than length), given the ciphertext.


7. We briefly discussed **unicity distance** in class.  What does this mean?  Give an example of where it is important.

- minumum length of ciphertext needed to uniquely compute the secret key
- this is an estimate
    - in actuality, it could be much different
- having spurious keys is beneficial to add an element of uncertainty as to whether or not an attacker has guessed the correct key

\[
u = \frac{\log_2(|K|)}{R_L\log_2{|P|}}
\]
where $R_L$ is a redundancy coefficient (English has about 0.75)

8. The one-time pad is **unconditionally secure** (i.e., it provides perfect security). What are two disadvantages of the one-time pad?

- in order for perfect security, Shannon showed we need a key as long as the plaintext
    - this means that for example a 1TB hard drive would need a 1TB key
    - this is a lot of redundancy
    - also, difficult to share a 1TB key on a secret channel
        - if we can do that, why not just shared the 1TB message on the secret channel?
- having to pick a new random key each time (esp. given length above) can introduce a lot of overhead and logistical issues

9. The one-time pad is **malleable**.  Briefly explain what this means and what it means for the one-time-pad.

- this means that it is possible to transform the ciphertext into another ciphertext which still makes sense when decrypted
- an attacker could modify the message on its way
- this means we don't have any integrity or origin authentication


10. How is a stream cipher related to the one-time pad?

- stream ciphers also encode using random bits
- however, they can use a small number of truly random bits to generate a secret key
    - this key is then used with a nonce
- this produces a long keystream
- it is similar to the one-time pad but addresses a few of its disadvantages


11. Name a block cipher that you can use to encrypt Carleton's yearly calendar. If you cannot explain why not.

- You could use a CBC block cipher with ciphertext stealing padding and a block size of say 128 bits
- encode the calendar in some meaningful way (say, as a JSON object)
- pad with zeroes until we get a multiple of 128 bits
- take the first block and xor with an IV
- then encrypt with encryption rule
- then take that ciphertext and xor with the next block
- and so on
- then swap last two blocks and truncate by how much we padded
- this is our final encrypted calendar

12. Briefly explain the following security goals of block ciphers

	1. diffusion
	2. confusion
	3. key size

- diffusion
    - changing a single bit of the message should change about half the bits in ciphertext
    - this is helpful for reducing malleability
    - also helps to complicate the ciphertext
- confusion
    - each bit of c should depend on several bits of k
    - this helps complicate the relationship between c and k
    - complicates the overall ciphertext
- key size
    - key size should be big enough to stop easy attacks
    - should be small enough to still be efficient


13. DES was a great block cipher when it was created.  Briefly explain the two reasons why it is no longer great today.

- its key size was way to small
    - brute forcing has gotten easier due to more powerful hardware
- it had a small block size
    - it is highly likely to have a duplicated block (which could yield information about the key)


14. Describe double-DES (2DES) as described in class. Why is 2DES essentially no more secure than DES even though the number of key bits has doubled? Explain the attack involved.

- weak to a "meet in the middle" attack
    - because the block length is still the same and we are not
      really making the problem any harder just by doubling key size
- how meet in the middle works
    - three plaintext ciphertext pairs
    - compute $D_h_1(c_1)$ for all possible keys $h_1$ and store $(D_h_1(c_1), h_1)$ pairs in a hash table
    - compute $E_h_2(m_1)$ for all possible keys $h_2$
        - check to see if the result is a key in the hash table
        - then verify $E_h_2(E_h_1(m_2)) = c_2$ and $E_h_2(E_h_1(m_3)) = c_3$
- it's basically as fast as bruteforcing regular DES
    - $2^{57}$


15. Triple-DES (3DES) is believed to be a more secure replacement for DES than double-DES.  It encrypts, decrypts then encrypts using DES with 3 different keys K1, K2, K3.

	1. 3DES has 3*56 bits of secret key. Describe how the attack on 2DES can be mounted on 3DES and what the **effective keylength** is. The effective keylength is the number of bits of a key in which a brute force attack has the same cost as the best attack on the system.
	2. Explain why anyone might want to use 3DES with K1=K2=K3.

- pretty much run the same attack with an extra step of decrypting with the extra key in the middle
    - effective keylength is 112 since it takes $2^{112}$ steps here
- having all three keys identical gives backwards compatibility to DES
    - but this is not recommended
      as now we can brute force in $O(2^{56})$ steps

16. What block cipher should you use today if you were to need to use a block cipher?

- probably AES-256
    - considered the most secure
    - AES-192 is okay
    - AES-128 probably shouldn't be used compared to the other two


17. Briefly explain what a block cipher mode of operation is.

- use of a block cipher to accomplish a cryptographic goal
- modes + encryption rule + padding method yields the full block cipher
- example mode = CBC
    - IV for first block or ciphertext of previous block for subsequent blocks to xor plaintext before encrypting


18. What is the ECB (electronic code book) mode of operation?  What is one flaw of ECB?

- encrypt each block individually
- it has no semantic security


19. What is the CBC (cipher block chaining) mode of operation? Why should the initialization vector (IV) be different for each plaintext message we want to encrypt with this mode?

- cbc is when we XOR the plaintext with a vector before encrypting
    - for the first block, we use an IV
    - for the other blocks, we use the previous block's resulting ciphertext
- without a different IV each time, we open ourselves up to known-plaintext attacks
    - the attacker will know something about subsequent encryptions by figuring out the common IV


20. There are three properties a cryptographic hash function might have.  Briefly explain each.

	1. pre-image resistance (one-way)
	2. second pre-image resistance (week collision resistance)
	3. collision resistance (strong collision resistance)

- pre-image resistance (one-way)
    - if we have some hash $H(m) = h$, it should be intractable that we determine $m$ using $h$
- second pre-image resistance (week collision resistance)
    - given a fixed $H(m_1)$, it should be intractable to find $m_2$ such that $H(m_1) = H(m_2)$
- collision resistance (strong collision resistance)
    - should be intractable to find any two inputs such that $H(m_1) = H(m_2)$


21. The hash function from class

```
J(x) = / 1||x,    if |x| == n bits
       \ 0||H(x)  if |x| != n bits
```

is collision resistant, if `H(x)` is collision resistant, but not pre-image resistant.  Briefly explain why this so.


22. Suppose a hash function outputs an n-bit hash.  How many times do we expect to compute this function (with random inputs) before we get a collision? What name do we give to this result?

- we expect a collision after about $2^{n/2}$ steps where $n$ is the hash length
    - this is called the birthday attack
    - related probabilistically to the idea of finding two people with the same birthday


23. What is the Merkle-Damgard construction for hash functions?

- take message blocks and add padding to last one until they are all size $r$
- run through a compression function that maps to size $n$
- use an initialization vector for initialization on the first compression function,
then use subsequent compression outputs to initialize next
- sometimes a final step before output
- this is used in many hash functions in the 90s
    - MD5
    - SHA-1
    - SHA-2
- susceptible to length extension attacks
    - we can compute the hash value of a message we don't already know


24. What is MAC? (What is this an acronym for?) What is it used for (which problem does it help address)?

- message authentication code
    - calculated using a private key, verified using a public key
- it addresses the problem of data origin authentication and data integrity authentication


25. Briefly explain what each of the following mean w.r.t. MACs.

	1. existential forgery
	2. selective forgery
	3. universal forgery


26. Explain why `H_k(m) = h(k||m)` and `H_k(m) = h(m||k)` are not secure MACs when the underlying hash function uses the Merkle-Damgard constructions? Go through the attacks for each.


27. The CBC-MAC uses a block cipher in cbc-mode to create a hash function. Is this secure? When is it secure and when is it not?


28. Suppose we propose the following MAC: given a message broken into blocks `m_1, m_1, ..., m_r`, we compute

```
 c_i = AES_k(m_{i}) XOR  c_{i-1}
```
for `i=1..r`, where `k` is a shared secret key and `c_0 = k`.

The MAC tag is the value `c_r`. Is this a secure MAC? Can you create any forgeries? Does it matter if we insist that the message length be a multiple of the block length? Does it matter if we have to pad the last block?


29. EMAC is the encrypted CBC-MAC.  How does it differ from cbc-mac? Is this secure? What flaw in CBC-MAC does EMAC address?


30. In class we saw why that the first version of SSL in Netscape was insecure. Briefly explain why. (The full exact details are not required)


31. A cryptographically secure pseudorandom bit generator (CSPRBG) should have two additional properties compared to a typical PRBG.  Briefly, explain the following two properties.

	1. next-bit test
	2. forward security


32. The Blum-Blum-Shub PRBG is as follows:

```
  choose random primes p,q with p,q != 3 mod 4
  let N=pq
  choose random seed x_0, such that x_0 > 1 and gcd(x_0,N) = 1
  for i from 1 to (as many bits as needed) do
    compute x_{i} := x_{i-1}^2 mod N
    output 1 if x_i is odd or output 0 if x_i is even
  od
```
	1. why is this not used in practice?
	2. why is it considered secure?




33. Briefly outline how an exhaustive search (brute-force) can be used to find the secret key in a symmetric-key encryption scheme.  This is a ciphertext-only attack. Briefly outline when this attack works, when it fails and
when the time/space complexity of it.



34. Suppose you have a private key encryption scheme.  Explain why you might want to send a MAC along with the ciphertext when sending messages.


35. Suppose you have an ideal random function
```
f : {0,1}^n --> {0,1}^{60}.
```
	1. What is the probability that two random inputs lead to a collision?
	2. How many randomly chosen elements do we need so that the probability that a collision occurs is 1/2.


36. Suppose we have a hash function
```
h : Z_12 --> Z_8
```
defined as follows:

```
x    h(x)
---------
0     4
1     3
2     6
3     7
4     3
5     0
6     2
7     1
8     5
9     2
10    7
11    6
```
Using this hash function, illustrate why it does not satisfy
any of the properties that a cryptographic hash function might have (pre-image resistance, second pre-image resistance, collision resistance).



37. Describe the padding scheme outlined in class (and discussed in the textbook).


38. What is ciphertext stealing? Why would we want to employ this? How does it work when using CBC-mode for a block cipher?


39. Given the ciphertext "YOUCANTDECRYPTME", encrypted using the one-time-pad, what is the corresponding plaintext?

If the key was sufficiently random, this should be pretty much impossible to do by hand.
If we were somehow able to intercept the secret key or had sufficient computational resources available, things would
be different. Another thing to bear in mind is that if the key is shorter than the plaintext, we can potentially
use clever frequency analysis and factoring techniques to help determine the key.

However, as it stands, this is pretty much a trick question.


