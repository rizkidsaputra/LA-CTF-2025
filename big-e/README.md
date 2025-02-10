# big e - LA CTF Writeup

## 🔍 Challenge Overview

This challenge presents an RSA encryption (`chall.py`) setup where the same modulus \( n \) is used with two different public exponents \( e_1 \) and \( e_2 \). Given two ciphertexts encrypted with these exponents, our task is to recover the original plaintext (flag).

## 🚀 How to Solve

### Understanding the Vulnerability

The encryption process follows:
- \( ct_1 = pt^{e_1} \mod n \)
- \( ct_2 = pt^{e_2} \mod n \)

Since both ciphertexts share the same modulus \( n \), we can exploit the **Common Modulus Attack**. The key idea is to leverage the **Extended Euclidean Algorithm** to express \( e_1 \) and \( e_2 \) as a linear combination:

\[ a \cdot e_1 + b \cdot e_2 = 1 \]

Rearranging the ciphertext equations, we get:

\[ (ct_1^a \cdot ct_2^b) \mod n = pt \]

This directly yields the plaintext.

### Exploitation Code

```python
from Crypto.Util.number import long_to_bytes
from sympy import mod_inverse

def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    gcd, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return gcd, x, y

# Given values
ct_1 = ...  # Ciphertext 1
ct_2 = ...  # Ciphertext 2
e_1 = ...   # Exponent 1
e_2 = ...   # Exponent 2
n = ...     # Modulus

# Compute coefficients a and b
gcd, a, b = extended_gcd(e_1, e_2)

# Adjust if negative
if b < 0:
    ct_2 = mod_inverse(ct_2, n)
    b = -b
if a < 0:
    ct_1 = mod_inverse(ct_1, n)
    a = -a

# Recover plaintext
pt = (pow(ct_1, a, n) * pow(ct_2, b, n)) % n
flag = long_to_bytes(pt)
print(flag)
```

### Flag
```
lactf{b1g_3_but_sm4ll_d!!!_part2_since_i_trolled}
```