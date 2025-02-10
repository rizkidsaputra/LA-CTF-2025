# extended - LA CTF Writeup

## 🔍 Challenge Overview

We are given a Python script (`gen.py`) that encrypts the flag and generates `chall.txt`. The resulting file contains weird characters due to bit manipulation and `ISO-8859-1` encoding, making the flag appear differently on Mac and Windows. Our goal? **Reverse the encryption process and retrieve the flag!**

## 🚀 How to Solve

### Code Breakdown (`gen.py`)

1. **Convert characters to 8-bit binary** → `bin(ord(c))[2:].zfill(8)`
2. **Flip the first `0` bit to `1`** → Obfuscating the original flag
3. **Convert back to ASCII characters** → Producing encrypted text
4. **Save the output in `ISO-8859-1` encoding** → Causes OS-dependent visual differences

### Decrypting the Flag

To retrieve the original flag, we simply **reverse the encryption process**:

1. **Read `chall.txt` using `ISO-8859-1` encoding**
2. **Convert characters to 8-bit binary**
3. **Flip the first `1` bit back to `0`**
4. **Convert back to ASCII characters**

### Exploit Code

```python
encrypted_text = "ìáãôæûÆõîîéìùßÅîïõçèßÔèéóßÌïïëóßÄéææåòåîôßÏîßÍáãßÁîäß×éîäï÷óý"

original_flag = ""

for c in encrypted_text:
    o = bin(ord(c))[2:].zfill(8)  # Convert character to 8-bit binary
    first_one_index = o.index("1")  # Find first "1" bit
    o = o[:first_one_index] + "0" + o[first_one_index + 1:]  # Flip it back to "0"
    original_flag += chr(int(o, 2))  # Convert back to character

print(original_flag)
```

### Flag
```
lactf{Funnily_Enough_This_Looks_Different_On_Mac_And_Windows}
```

No magic, just **bit flipping and encoding tricks**. GG! 🚩
