
**[https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/)**
https://cryptii.com/

## Bases system / Ascii (numbers rep letters)

- Binary - base 2 (0 and 1)
- Hexadecimal - base base 16 (0 to 9, A to F)
- octal - base 8 (0 to 7)
- Decimal - base 10 (0 to 10)
- Ascii - numbers representing characters (0 -255)

if got a bunch of numbers can figure out the base, convert to decimal,  then convert to ascii

*Basic Ascii / bases conversion*
`int(string, base)` --> gives decimal representation
`chr(number)` --> gives ascii representation
`ord(string)` --> gives number rep (ascii)
`bin(number)`
`hex()`
`oct()`

*hex strings*
generally, every 2 hex characters gives one ascii, you can tell its hex when it has numbers and letters up to f
`bytes.fromhex()` --> gives bytes from hex string

*Base64*
Used to represent binary data as an ascii string using 64 characters, one character encodes 6 bits, 4 characters (24 bits) would be 3 bytes
characters is A-Z a-z 0-9 /+ and = for padding
`import base64`
`base64.b64encode()`

*Bytes to integers*
ascii -> hex -> concatenate the hex to form one number -> convert to decimal
`from Crypto.Util.number import *`
`long_to_bytes(num)`


## Substitution Cyphers

https://cryptii.com/pipes/caesar-cipher

*caesar*
- Shift characters by certain fixed amount
- Note: online crackers usually don't know how to deal with non alphabet, can write a basic python script for conversion to ascii, adding amount, then reconversion

*vigenere*
* caesar but each characters shift is different, based on key
* if key is less than length of string, it is repeated
##  XOR Encryption

*links*
https://ctf101.org/cryptography/what-is-xor/
https://idafchev.github.io/crypto/2017/04/13/crypto_part1.html

`^` --> XOR operation means exclusive or, its a bitwise operation where it returns true if one condition is true and false otherwise (only a 1 and 0 returns 1)
101 ^ 100 = 001
001 ^ 100 = 101 --> XOR is reversible!

*XOR properties*
![[Pasted image 20240516220248.png]]

*Encryption / decryption* (Single-byte key)
`encrypted = ''.join([chr(ord(x) ^ ord(key)) for x in data])`
`decrypted = ''.join([chr(ord(x) ^ ord(key)) for x in encrypted])`

*crack*
brute force with different keys ;-;
Try and keep all the data to lists of integers, easiest to work with (instead of bytes objects or strings or wtv)

*multi-byte keys*
e.g. key = "pwn" --> more than one char so above decryption doesnt work alr
to crack, loop thru the key

##  Hashing Functions

*links*
http://www.unixwiz.net/techtips/iguide-crypto-hashes.html
https://ctf101.org/cryptography/what-are-hashing-functions/
https://www.mscs.dal.ca/~selinger/md5collision/
https://www.researchgate.net/publication/225230142_How_to_Break_MD5_and_Other_Hash_Functions

Hashing functions are **one way** (not meant for hash to be decrypted to original message) functions which theoretically provide a unique output for every input (e.g. MD5, SHA-1)

*uses*
- passwords are often stored as hashes instead of clear text (input is hashed and compared with initial hash)
- file hashes can be used to check if the file is uncorrupted

*hash collisions / crack*
if you can find a string/file that has the same hash as the password, you can bypass the system using a hash collision


## MATHH THAT MAY BE HELPFUL

*greatest common divisor*
if gcd = 1, then 2 numbers are coprime
If `a` and `b` are prime, they are also coprime. If `a` is prime and `b < a` then `a` and `b` are coprime.

**Euclidian algorithm** can be used to find the GCD of any 2 numbers:
```
let a and b the 2 numbers
since a = gcd * x and b = gcd * y
then a-nb = gcd * (x-ny)

find a%b, which will be a multiple of gcd (x-ny)
then take b%(a%b) also a multiple of gcd
until eventually you get zero, in this case the penultimate value was gcd

e.g. 12 and 8, gcd=4
12 = 4*3, 8=4*2
12%8 = 4 = 4*1
8%4 = 0
```
https://wonghoi.humgar.com/blog/2021/11/21/easiest-way-to-remember-euclid-gcd-algorithm-laying-tiles/

**Bezout's Identity** states that there exists `ax + by = gcd(a,b)`
This can be found with **Extended Euclidian algorithm**
```
function extended_gcd(a, b)
    (old_r, r) := (a, b)
    (old_s, s) := (1, 0)
    (old_t, t) := (0, 1)
    
    while r ≠ 0 do
        quotient := old_r div r
        (old_r, r) := (r, old_r − quotient × r)
        (old_s, s) := (s, old_s − quotient × s)
        (old_t, t) := (t, old_t − quotient × t)
    
    output "Bézout coefficients:", (old_s, old_t)
    output "greatest common divisor:", old_r
    output "quotients by the gcd:", (t, s)
```
honestly idk how this works

*modular arithmetic*
2 numbers, a and b are **congruent modulo n** if `a%n = b%n`
`a is congruent modulo n to a%n, since (a%n)%n = a%n`

**Fermat's little theorem** states that, if `p` is a prime number
![[Pasted image 20240525221631.png]]
so `a^p - a` is a multiple of `p`.
![[Pasted image 20240525222009.png]]
(both sides divide `a` from earlier)
e.g. 3^16 mod 17 = 1,  3^17 mod 17 = 3

**multiplicative inverse** (d) is a number where:
`g * d and 1 congruent mod p`
`g*d mod p = 1`
NOTE: the multiplicative inverse is always < p since if its > p then its like k+p and the p part cancels out

`x` is a **Quadratic Residue** if there exists an `a` such that `a^2 and x congruro mod p`. If there is no such solution, then the integer is a Quadratic Non-Residue. **square root** of x is `a`
use `pow(a, 2, p)` for` x = a^2 mod p`

*misc*
in python to change from scientific notation, apply precision with f strings
e.g. `f"{num : .1f}"`

## RSA

*links*
https://ctf101.org/cryptography/what-is-rsa/
https://www.youtube.com/watch?v=sYCzu04ftaY&ab_channel=LiveOverflow
https://en.wikipedia.org/wiki/RSA_(cryptosystem)

Asymmetric encryption: public key encrypts data, private key decrypts it (RSA is mathhh)

*definitions*
The Public Key is made up of **(n, e)**
The Private Key is made up of **(n, d)**
The message is represented as **m** and is converted into a number
The encrypted message or ciphertext is represented by **c**
**p** and **q** are prime numbers which make up n
**e** is the public exponent
**n** is the modulus and its length in bits is the bit length (i.e. 1024 bit RSA)
**d** is the private exponent
The totient λ(n) is used to compute d and is equal to the number of integers (1 to d) that is relatively prime with n, it is a multiplicative function: λ(n) = λ(p)λ(q), since p and q are primes, λ(p) = p-1, so λ(n)  = (p-1)(q-1) 

*equation*
`(m^e)^d = m(mod n)`

*implementation*
https://leimao.github.io/article/RSA-Algorithm/
1. Choose 2 prime numbers p and q (_p_ = 61 and _q_ = 53)
2.  Find n (*pq* = 3233)
3.  Find λ(_n_) = (p-1)(q-1) OR lcm(p-1, q-1)
4.  Choose a public exponent such that 1 < e < λ(n) and is coprime (not a factor of) λ(n). The standard is most cases is 65537 (e = 17)
5.  Calculate d as the modular multiplicative inverse or in english find d such that:  d * e mod λ(n) = 1 (d = 1/17 mod 480 = 413) --> this can be found with `d = pow(e, -1, tot)`
6. Public key = (n, e)
7. Private key = (n, d)
8.  ==Encrypted message, c = m^e mod n==
9. ==decrypted message, m = c^d mod n==

==Note: getting the private key from public key is math-y but only involves n, e, p and q. n e is the public key, the problem is p q is hard to derive from n (prime factorisation==)

calculations -> https://www.tausquared.net/pages/ctf/rsa.html

*factorise N to find p and q*
http://factordb.com/ --> especially if n value is in this database (means the prime factors p,q are available and can be brute forced)
otherwise try pyfactorise
or try sagemath ecm.find_factor https://doc.sagemath.org/html/en/reference/interfaces/sage/interfaces/ecm.html

*N is a monoprime*
tot(N) becomes really easy to find - N-1
the point of rsa is that there is no easy way to find tot(N) = (p-1)(q-1) without knowing p and q and so private key cannot be found

*rsactf*
https://github.com/RsaCtfTool/RsaCtfTool - clone it from here

*small exponent*
If e is very small, m^e is alot smaller than n: (usually e=3)
`c = m^e mod n = m^e` 
so if you are given c you can `^(1/e)` to get m  

*more attacks*
https://bitsdeep.com/posts/attacking-rsa-for-fun-and-ctf-points-part-2/

## Diffie Helman key exchange

key exchange*
public numbers --> mod P (large prime), base g (primitive root modulo of P)
secret values --> random numbers a and b for client and server (private key)
A  = pow(g, a, P)
B = pow(g, b, P)
A is sent to b, B is sent to a
shared key --> B^a mod P, A^b mod P
after that some kind of symetric encryption can be used, eg. AES

*ephemeral*
that means the secret values change every session

*vulnerability*
small p: https://www.alpertron.com.ar/DILOG.HTM log jam attack



## AES (block ciphers)

block based = input is in blocks of specific sizes e.g. 16 bytes, have to split then pad the plaintext to fit into blocks
plaintext --> 4 by 4 matrix of bytes (state)

1. **KeyExpansion** or Key Schedule  
  
 From the 128 (16 bytes) bit key, 11 separate 128 bit "round keys" are derived: one to be used in each AddRoundKey step.  
  
2. **Initial key addition**  
  
 _AddRoundKey_ - the bytes of the first round key are XOR'd with the bytes of the state.  
  
3. **Round** - this phase is looped 10 times, for 9 main rounds plus one "final round"  
  
 a) _SubBytes_ - each byte of the state is substituted for a different byte according to a lookup table ("S-box"). Basically every byte turns into another byte based on some complicated function.
  
 b) _ShiftRows_ - the last three rows of the state matrix are transposed—shifted over a column or two or three.  2nd row shift by one, 3rd row by two, fourth row by three.
  
 c) _MixColumns_ - matrix multiplication is performed on the columns of the state, combining the four bytes in each column. This is skipped in the final round.  

 d) _AddRoundKey_ - the bytes of the current round key are XOR'd with the bytes of the state.

*decryption*
obviously if you can somehow brute force the key you can just decrypt it
`from Crypto.Cipher import AES`
`cipher = AES.new(key, AES.MODE_ECB)` --> need to know mode and key
`decrypted = cipher.decrypt(ciphertext)`

*operation modes*
1. ECB (electronic code book) --> plain text is divided into different blocks of equal block sizes. Then each block is encrypted  /decrypted in parallel (same key?)
2. CBC (Cipher Block Chaining) --> Uses a random block of bytes called the initialisation vector (IV) to ensure randomization. First block is XORed with the IV then encrypted, second block is XORed with first cipher block then encrypted and so on. To decrypt, starting from last block, you decrypt then XOR with next cipher block to get plaintext

*ECB Byte at a time*
if your input is added to a secret string then encrypted, you can find the secret string by varying your input. `encrypt(<input>+<secret>)`
https://c0nradsc0rner.com/2016/07/03/ecb-byte-at-a-time/




## Bcrypt

*links*
https://en.wikipedia.org/wiki/Bcrypt

identify: strings that have 3 $ sign
![[Pasted image 20240327210438.png]]

tools - debcrypt (https://github.com/BREAKTEAM/Debcrypt)

