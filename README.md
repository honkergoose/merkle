Lots of stuff uses merkle trees. I should learn a bit about them.

"hash tree" Merkle tree is a tree
 tree -> root node with children nodes. children nodes are subtrees. nothing points to root

every node of a merkle tree labelled with a cryptographic hash of a data block
 cryptographic hash -> map of data to a bit array of a fixed size; it is a one-way function SHA-blah
every non-leaf node is labelled with the cryptographic hash of the labels of its child nodes (huh)

Hash Trees are a generalization of hash lists & hash chains
Ralph Merkle

So first things first. How the hell do you hash stuff?
Lets implement SHA-256 first


arbitrary length data -> n-bit hash values
 - one way
 - collision resistent

n = 128 (MD4 & MD5) n = 160 (SHA-1)
64 bits provide security | 80 bits provide security against collisions
me: Think about a password. You wouldn't want something that's not your password unlocking your bank account
    so you probably don't want to use MD5 as your password hashing algorithm

SHA-256 spec
 - the message is padded with its length such that it is a multiple of 512 bits long
 - parsed into 512-bit message blocks M(1), M(2), M(3)
Blocks are processed once at a time going through H(idx) = H(idx - 1) + CM(idx)(H^idx-1)
 C is compression
 + is word-wise mod 2^23 addition (ugh what)
 H(N) is the hash of M

Message blocks are 512bit
256bit intermediate hash value (we're reducing the blocks)

operations used:
(+) bitwise XOR
^ bitwise AND
down arrow  bitwise OR
-|  bitwise compliment
+ mod 2^23 addition
R^n right shift by n bits
S^n right rotation by n bits

Lets see what each of these operations would look like in python

Remember your bit flips kids
XOR because I always forget

|Input|Output|
|-----|------|
|0 |0 |  0   |
|0 |1 |  1   |
|1 |0 |  1   |
|1 |1 |  0   |

```python3
def bitwise_or(x, y):
  return x | y

def bitwise_and(x, y):
  return x & y

def bitwise_xor(x, y):
  return x ^ y

def bitwise_compliment(x):
  return ~x

def bitshift_right(x, y):
  return x >> y

def bitshift_left(x, y):
  return x << y

# the mod thing makes no sense to me
# 2 ^ 23 addition... ok
# oof math
# This is somewhat useful: https://crypto.stackexchange.com/questions/5107/what-exactly-is-addition-modulo-232-in-cryptography
# based on this; this would also be called k-bit addition or 23 bit addition (?)

def bit_add_23(a, b):
  return None

```
All these operations act of 32-bit words

dammit im back to math
