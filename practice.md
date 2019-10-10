---
title: COMP4109 Midterm 1 Practice
author: Findlay et al.
---

1. A symmetric-key encryption scheme is defined as a 5-tuple (P,C,K,E,D). Complete the definition.


2. Describe the shift cipher (Caesar cipher).


3. Explain one situation when the shift cipher gives you prefect security? (That is, you cannot determine the secret key with probability greater than 1/26.)


4. Briefly explain the following (giving a possible real-world example for each if you can):

	1. ciphertext-only attack
	2. known-plaintext attack
	3. chosen plaintext attack
	4. chosen ciphertext attack


5. What needs to be specified in a security model?


6. A symmetric-key encryption scheme is **semantically secure** if `Pr(m) = Pr(m|c)`. Explain what this means.


7. We briefly discussed **unicity distance** in class.  What does this mean?  Give an example of where it is important.


8. The one-time pad is **unconditionally secure** (i.e., it provides perfect security). What are two disadvantages of the one-time pad?


9. The one-time pad is **malleable**.  Briefly explain what this means and what it means for the one-time-pad.


10. How is a stream cipher related to the one-time pad?


11. Name a block cipher that you can use to encrypt Carleton's yearly calendar. If you cannot explain why not.


12. Briefly explain the following security goals of block ciphers

	1. diffusion
	2. confusion
	3. key size


13. DES was a great block cipher when it was created.  Briefly explain the two reasons why it is no longer great today.


14. Describe double-DES (2DES) as described in class. Why is 2DES essentially no more secure than DES even though the number of key bits has doubled? Explain the attack involved.


15. Triple-DES (3DES) is believed to be a more secure replacement for DES than double-DES.  It encrypts, decrypts then encrypts using DES with 3 different keys K1, K2, K3.

	1. 3DES has 3*56 bits of secret key. Describe how the attack on 2DES can be mounted on 3DES and what the **effective keylength** is. The effective keylength is the number of bits of a key in which a brute force attack has the same cost as the best attack on the system.
	2. Explain why anyone might want to use 3DES with K1=K2=K3.


16. What block cipher should you use today if you were to need to use a block cipher?


17. Briefly explain what a block cipher mode of operation is.


18. What is the ECB (electronic code book) mode of operation?  What is one flaw of ECB?


19. What is the CBC (cipher block chaining) mode of operation? Why should the initialization vector (IV) be different for each plaintext message we want to encrypt with this mode?


20. There are three properties a cryptographic hash function might have.  Briefly explain each.

	1. pre-image resistance (one-way)
	2. second pre-image resistance (week collision resistance)
	3. collision resistance (strong collision resistance)


21. The hash function from class

```
J(x) = / 1||x,    if |x| == n bits
       \ 0||H(x)  if |x| != n bits
```

is collision resistant, if `H(x)` is collision resistant, but not pre-image resistant.  Briefly explain why this so.


21. Suppose a hash function outputs an n-bit hash.  How many times do we expect to compute this function (with random inputs) before we get a collision? What name do we give to this result?


22. What is the Merkle-Damgard construction for hash functions?


23. What is MAC? (What is this an acronym for?) What is it used for (which problem does it help address)?


24. Briefly explain what each of the following mean w.r.t. MACs.

	1. existential forgery
	2. selective forgery
	3. universal forgery


25. Explain why `H_k(m) = h(k||m)` and `H_k(m) = h(m||k)` are not secure MACs when the underlying hash function uses the Merkle-Damgard constructions? Go through the attacks for each.


26. The CBC-MAC uses a block cipher in cbc-mode to create a hash function. Is this secure? When is it secure and when is it not?


27. Suppose we propose the following MAC: given a message broken into blocks `m_1, m_1, ..., m_r`, we compute

```
 c_i = AES_k(m_{i}) XOR  c_{i-1}
```
for `i=1..r`, where `k` is a shared secret key and `c_0 = k`.

The MAC tag is the value `c_r`. Is this a secure MAC? Can you create any forgeries? Does it matter if we insist that the message length be a multiple of the block length? Does it matter if we have to pad the last block?


28. EMAC is the encrypted CBC-MAC.  How does it differ from cbc-mac? Is this secure? What flaw in CBC-MAC does EMAC address?


29. In class we saw why that the first version of SSL in Netscape was insecure. Briefly explain why. (The full exact details are not required)


30. A cryptographically secure pseudorandom bit generator (CSPRBG) should have two additional properties compared to a typical PRBG.  Briefly, explain the following two properties.

	1. next-bit test
	2. forward security


31. The Blum-Blum-Shub PRBG is as follows:

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




32. Briefly outline how an exhaustive search (brute-force) can be used to find the secret key in a symmetric-key encryption scheme.  This is a ciphertext-only attack. Briefly outline when this attack works, when it fails and
when the time/space complexity of it.



33. Suppose you have a private key encryption scheme.  Explain why you might want to send a MAC along with the ciphertext when sending messages.


34. Suppose you have an ideal random function
```
f : {0,1}^n --> {0,1}^{60}.
```
	1. What is the probability that two random inputs lead to a collision?
	2. How many randomly chosen elements do we need so that the probability that a collision occurs is 1/2.


35. Suppose we have a hash function $
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



36. Describe the padding scheme outlined in class (and discussed in the textbook).


37. What is ciphertext stealing? Why would we want to employ this? How does it work when using CBC-mode for a block cipher?


38. Given the ciphertext "YOUCANTDECRYPTME", encrypted using the one-time-pad, what is the corresponding plaintext?



