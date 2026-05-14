# 🔐 Linux Caesar Cipher & File Decryption Lab

**Author:** Robert Ursache  
**Date:** 14/05/2026  
**Objective:** Demonstrate the ability to navigate a Linux filesystem, find hidden files, break a Caesar cipher, and decrypt an AES‑256‑CBC encrypted file using OpenSSL.

## 📌 Scenario

All files in my home directory (`/home/analyst`) were encrypted. A `README.txt` file contained a clue: a hidden file in the `caesar` subdirectory held the key to recovery. I had to:

1. Find the hidden file.
2. Decode a Caesar cipher (left shift 3).
3. Use the recovered command to decrypt an `.encrypted` file and read the hidden message.

This lab simulates a real‑world situation where an attacker uses basic encryption to lock files, and an analyst must reverse it.

## 🛠️ Tools & Environment

- **OS:** Linux (Ubuntu/CentOS style)
- **Shell:** Bash
- **Commands used:** `ls`, `cat`, `cd`, `tr`, `openssl`
- **Cipher type:** Caesar cipher (shift by 3)
- **Strong encryption:** AES‑256‑CBC with PBKDF2 key derivation

## 🧪 Step‑by‑Step Execution

### 1. Explore the home directory

```bash
ls /home/analyst
```

**Output:**
```
Q1.encrypted  README.txt  caesar
```

The `README.txt` contained:

> Hello,
> All of your data has been encrypted. To recover your data, you will need to solve a cipher. To get started look for a hidden file in the caesar subdirectory.

### 2. Find the hidden file

Moved into the `caesar` directory and listed all files (including hidden ones):

```bash
cd caesar
ls -a
```

**Output:**
```
.  ..  .leftShift3
```

The file `.leftShift3` (hidden by the leading dot) was the ciphertext.

### 3. Decode the Caesar cipher

Inspected the file:

```bash
cat .leftShift3
```

It showed scrambled text. I recognised a Caesar cipher where each letter was shifted **3 positions to the left** (e.g., `d` → `a`).

To decode, I shifted **3 positions to the right** using `tr`:

```bash
cat .leftShift3 | tr "d-za-cD-ZA-C" "a-zA-Z"
```

**Why this works:**  
- Source set `d-za-c` means `d e f … z a b c` (lowercase, rotated left by 3)  
- Source set `D-ZA-C` does the same for uppercase  
- Target set `a-zA-Z` is the normal alphabet  
- `tr` maps each source character to the target at the same position → effectively shifts right by 3.

**Decoded output:**
```
In order to recover your files you will need to enter the following command:

openssl aes-256-cbc -pbkdf2 -a -d -in Q1.encrypted -out Q1.recovered -k ettubrute
```

### 4. Decrypt the encrypted file

Returned to the home directory:

```bash
cd ~
```

Ran the exact command provided:

```bash
openssl aes-256-cbc -pbkdf2 -a -d -in Q1.encrypted -out Q1.recovered -k ettubrute
```

**Command breakdown:**

| Option | Meaning |
|--------|---------|
| `aes-256-cbc` | Strong symmetric cipher |
| `-pbkdf2` | PBKDF2 key derivation (adds security) |
| `-a` | Base64 encoding of the input |
| `-d` | Decrypt mode |
| `-in` | Input file (encrypted) |
| `-out` | Output file (decrypted) |
| `-k` | Password (`ettubrute`) |

### 5. Read the recovered message

Listed files again to confirm the new file:

```bash
ls
```

**Output:**
```
Q1.encrypted  Q1.recovered  README.txt  caesar
```

Displayed the decrypted message:

```bash
cat Q1.recovered
```

**Output:**
```
If you are able to read this, then you have successfully decrypted the classic cipher text. You recovered the encryption key that was used to encrypt this file. Great work!
```

✅ **Success** – I fully recovered the data and proved my ability to handle basic cryptography in Linux.

## 📚 Skills Demonstrated

- **Linux command line** – navigation, file listing, hidden files
- **Text manipulation** – using `tr` to implement character translation
- **Cipher analysis** – recognising and breaking a Caesar cipher (shift by 3)
- **OpenSSL decryption** – using industry‑standard AES‑256‑CBC with PBKDF2
- **Documentation** – clear, step‑by‑step reporting suitable for technical and managerial audiences

## 🏁 Conclusion

This lab provided hands‑on experience with two critical layers of data protection:

1. **Classic cipher** (Caesar) – easily broken but educational.
2. **Modern symmetric encryption** (AES‑256‑CBC) – requires the correct key to decrypt.

I successfully navigated the filesystem, uncovered a hidden file, reversed the Caesar shift, and used the recovered key to decrypt the real encrypted data. These skills are directly applicable to incident response, malware analysis, and digital forensics.
