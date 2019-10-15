---
title: COMP4109 Midterm 1 Practice
author: Findlay et al.
---


1. A symmetric-key encryption scheme is defined as a 5-tuple (P,C,K,E,D). Complete the definition.

    P is plaintext space
    C is ciphertext space
    K is keyspace
    E is encryption functions where $e_k \in E$
    D is encryption functions where $d_k \in D$

    where $d_k(e_k(m)) = m$ for all m in P and k in K

2. Describe the shift cipher (Caesar cipher).

    Take $P=C=K=\mathbb{Z}_{26}$

    Select one value for k.

    For all $p_i \in p$, $c_i = p_i + k \bmod{26}$


3. Explain one situation when the shift cipher gives you prefect security? (That is, you cannot determine the secret key with probability greater than 1/26.)

    -Where you are encrypting just one letter. OR
    -Where you are encrypting gibberish.
    -In both cases, requency analysis won'y work.


4. Briefly explain the following (giving a possible real-world example for each if you can):

	1. ciphertext-only attack
        - attacker has one or more piece of ciphertext and tries to find k
        - for example, brute forcing a shift cipher
	2. known-plaintext attack
        - attacker knows one or more ciphertext, plaintext pairs and tries to find k
        - for example, a meet-in-the-middle attack on a double DES encryption
	3. chosen plaintext attack
        - attacker chooses plaintext and encrypts it to get ciphertext
	4. chosen ciphertext attack
        - attacker chooses ciphertext and decrypts it to get plaintext


5. What needs to be specified in a security model?

    - attack model
    - security goal
    - security level


6. A symmetric-key encryption scheme is **semantically secure** if `Pr(m) = Pr(m|c)`. Explain what this means.

    - if the probability of guessing the message is the same as the probability of guessing the message given ciphertext
    - for example, if you encrypted Tux and saw an outline of a penguin in the ciphertext image, it would not be semantically secure
        - if the ciphertext was random noise, it would be semantically secure


7. We briefly discussed **unicity distance** in class.  What does this mean?  Give an example of where it is important.

    - this is the average length of ciphertext needed to uniquely compute the key
    - it depends on keyspace size, plaintext space size, and a redundancy coefficient for the language
    - for example, it is important for determining whether or not spurious keys are a possibility when breaking shift ciphers


8. The one-time pad is **unconditionally secure** (i.e., it provides perfect security). What are two disadvantages of the one-time pad?

    - key size needs to be huge (at least same size as plaintext)
    - need to use a new key each time
    - key needs to be sufficiently random
    - it malleable.... more in next question


9. The one-time pad is **malleable**.  Briefly explain what this means and what it means for the one-time-pad.

    - it means that an attacker could modify the ciphertext to decrypt to a different but still meaningful message
    - this means it only prrovides confidentiality, not authentication (data origin and data integrity)


10. How is a stream cipher related to the one-time pad?

    - stream cipher and one-time pad both encrypt by xoring with a pseudorandom key
    - stream cipher does this one bit at a time and generates the keystream as it goes
        - so it's like a one-time pad but with a small tradeoff in security for a big win in terms of overhead


11. Name a block cipher that you can use to encrypt Carleton's yearly calendar. If you cannot explain why not.

    - without a mode this would likely not be possible since the calendar will be larger than block size
    - you could use CBC mode AES block cipher with ciphertext stealing
    - you would just have to encode the calendar in some meaningful way first


12. Briefly explain the following security goals of block ciphers

	1. diffusion
        - one bit changed in m should change about half the bits in c
        - this complicates the relationship between m and c
        - it helps provide semantic security and can help provide authentication
	2. confusion
        - one bit in c should depend on many bits in k
        - this complicates the relationship between c and k
	3. key size
        - key should be large enough, but not too large such that it would add too much overhead


13. DES was a great block cipher when it was created.  Briefly explain the two reasons why it is no longer great today.

    - the key size was way too small
        - 2^56 operations is not good enough to stop reasonably good modern hardware
    - block size was way to small
        - combined, the above two mean that smart attackers can do even less than 2^56 operations
        - specialized hardware can break it in less than a day

14. Describe double-DES (2DES) as described in class. Why is 2DES essentially no more secure than DES even though the number of key bits has doubled? Explain the attack involved.

    - 2DES was through to double key length by encrypting with a second key
    - this allows a KPA attack called a meet in the middle attack with 3 pairs of m,k
    - using this attack, you can easily find the keys in 2^57 calls


15. Triple-DES (3DES) is believed to be a more secure replacement for DES than double-DES.  It encrypts, decrypts then encrypts using DES with 3 different keys K1, K2, K3.

	1. 3DES has 3*56 bits of secret key. Describe how the attack on 2DES can be mounted on 3DES and what the **effective keylength** is. The effective keylength is the number of bits of a key in which a brute force attack has the same cost as the best attack on the system.
    - the effective keylength is 2^112 which is how many operations we need to do in order to run a meet-in-the-middle attack
	2. Explain why anyone might want to use 3DES with K1=K2=K3.
        - this is useful for backward compatibility with DES but it is insecure, so not recommended


16. What block cipher should you use today if you were to need to use a block cipher?

    - AES using CTR mode is probably the most recommended


17. Briefly explain what a block cipher mode of operation is.

    - mode of operation is a way of running the underlying block cipher with respect to how the blocks (if at all) interact with each other or what operations are done before/after encryption
    - for example, CBC mode chains the blocks together by xoring previous ciphertext with plaintext before encrypting


18. What is the ECB (electronic code book) mode of operation?  What is one flaw of ECB?

    - encrypts blocks one at a time with no interaction between them
    - it doesn't provide semantic security


19. What is the CBC (cipher block chaining) mode of operation? Why should the initialization vector (IV) be different for each plaintext message we want to encrypt with this mode?

    - chain blocks together by xoring plaintext block with previous ciphertext before encrypting
    - first block is a special case, where we xor with IV instead
    - we want to have a different IV each time because otherwise we may reveal information to an attacker
    - if two first blocks are the same and have the same IV, we have leaked information about k


20. There are three properties a cryptographic hash function might have.  Briefly explain each.

	1. pre-image resistance (one-way)
        - hard to find x given h(x)
	2. second pre-image resistance (week collision resistance)
        - hard to find x' given x such that h(x)=h(x')
	3. collision resistance (strong collision resistance)
        - hard to find any x,x' pair such that h(x)=h(x')


21. The hash function from class

```
J(x) = / 1||x,    if |x| == n bits
       \ 0||H(x)  if |x| != n bits
```

is collision resistant, if `H(x)` is collision resistant, but not pre-image resistant.  Briefly explain why this so.

    - collision resistant
        - no collisions in bottom since H(x) is collision resistant
        - no collisions in top since x is unique and since top and bottom always differ by most significant bit
    - not pre-image resistant
        - if the output's MSB is 1, we know the other bits are just x, so we can find x easily


21. Suppose a hash function outputs an n-bit hash.  How many times do we expect to compute this function (with random inputs) before we get a collision? What name do we give to this result?

    - 2^{n/2} times
    - birthday attack, named after the birthday problem


22. What is the Merkle-Damgard construction for hash functions?

    - run IV and a message block through a compression function
    - run the output of that compression function and another message block through another compression function
    - and so on.....
    - take last output as your hash
    - somtimes an extra step right before we take the hash


23. What is MAC? (What is this an acronym for?) What is it used for (which problem does it help address)?

    - message authentication code
    - addresses data origin and data integrity authentication


24. Briefly explain what each of the following mean w.r.t. MACs.

	1. existential forgery
        - there exists some forgery such that m' can be sent with the tag of m
	2. selective forgery
        - the attacker can choose some m' that can be sent with the tag of m
	3. universal forgery
        - the attacker can send any message they want with a valid tag


25. Explain why `H_k(m) = h(k||m)` and `H_k(m) = h(m||k)` are not secure MACs when the underlying hash function uses the Merkle-Damgard constructions? Go through the attacks for each.

    - for h(k||m), a simple length extension attack will do the trick
        - can compute H_k(m||y) given m and length of k but not k
    - for h(m||k), length extension no longer works, but we are vulnerable to hash collision attacks
        - find a collision x1, x2
        - get tag for x1 and send x2 with same tag
        - this apparently only works if k is one block length


26. The CBC-MAC uses a block cipher in cbc-mode to create a hash function. Is this secure? When is it secure and when is it not?

    - it is secure when we only allow fixed length messages (assuming the underlying block cipher is secure)
    - if we allow variable-length messages, we are vulernable to length-extension attacks


27. Suppose we propose the following MAC: given a message broken into blocks `m_1, m_1, ..., m_r`, we compute

```
 c_i = AES_k(m_{i}) XOR  c_{i-1}
```
for `i=1..r`, where `k` is a shared secret key and `c_0 = k`.

The MAC tag is the value `c_r`. Is this a secure MAC? Can you create any forgeries? Does it matter if we insist that the message length be a multiple of the block length? Does it matter if we have to pad the last block?

    - this is just a CBC-MAC using AES
    - with fixed length, it would be secure since AES is secure
    - insisting message length be a multiple of block length does not make a difference as long as it can still vary
    - it shouldn't matter if we have to pad the last block


28. EMAC is the encrypted CBC-MAC.  How does it differ from cbc-mac? Is this secure? What flaw in CBC-MAC does EMAC address?

    - encrypt the result of CBC-MAC with a second key
    - it is as secure as the underlying block cipher
    - addresses the problem of length-extension attacks in CBC-MACs
        - now we can allow messages of any length with no security issues


29. In class we saw why that the first version of SSL in Netscape was insecure. Briefly explain why. (The full exact details are not required)

    - used some values that were not secret (pid, ppid)
    - used some values that were not uniformly distributed (ppid)
    - used some modular values that did not have a large period (milliseconds)


30. A cryptographically secure pseudorandom bit generator (CSPRBG) should have two additional properties compared to a typical PRBG.  Briefly, explain the following two properties.

	1. next-bit test
        - given n-1 bits of c, we don't know any extra information about bit n of c
	2. forward security
        - given bit n of c, we don't know any extra information about the n-1 bits of a c that came before


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
        - a lot of overhead associated with finding primes, factoring, and performing mod on large numbers
	2. why is it considered secure?
        - because prime factorization is considered a hard problem
        - and we think P != NP




32. Briefly outline how an exhaustive search (brute-force) can be used to find the secret key in a symmetric-key encryption scheme.  This is a ciphertext-only attack. Briefly outline when this attack works, when it fails and
when the time/space complexity of it.

    - would fail if we have spurious keys or
    - if the the set of keys we need to try is way too big or
    - if the message was nonsense in the first place or
    - if the message was only one character long
    - first, we would use frequency analysis to find candidate key lengths if the scheme allows
    - next, we would try the keys one by one and use a combination of frequency analysis and digraph frequency analysis for our language to tell us when we found a likely match
    - O(|K|) in space and time complexity (this can be reduced by making intelligent guesses)



33. Suppose you have a private key encryption scheme.  Explain why you might want to send a MAC along with the ciphertext when sending messages.

    - the MAC would help provided data integrity authentication
    - would also help provide even more proof of data origin


34. Suppose you have an ideal random function
```
f : {0,1}^n --> {0,1}^{60}.
```
	1. What is the probability that two random inputs lead to a collision?
        - same idea as birthday problem
        - 1 - (2^60 - 2) / (2^60 - 1)
	2. How many randomly chosen elements do we need so that the probability that a collision occurs is 1/2.
        - 2^{n/2}


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

    - PIR
        - it's easy to find x = 0 if h(x) = 4
    - SPIR
        - it's easy to find x' = 1 if x = 4 so that h(x) = h(x')
    - CR
        - there are lots of h(x) that have the same output



36. Describe the padding scheme outlined in class (and discussed in the textbook).

    - take x = number of bytes in a block - number of bytes in last block
    - append x x times
    - if the last block matched perfectly, append block size block size times to a new block


37. What is ciphertext stealing? Why would we want to employ this? How does it work when using CBC-mode for a block cipher?

    - minimizes space overhead from padding
    - pad with all 0's
    - encrypt as normal
    - swap last two blocks
    - strip number of bits we padded off the end of the new last block


38. Given the ciphertext "YOUCANTDECRYPTME", encrypted using the one-time-pad, what is the corresponding plaintext?

    - this is not possible to find out by hand
    - the only way would be frequency analysis + brute force but this is still extremely infeasible, and certainly can't be done by hand



