# 🔐 Linux File Hashing & Integrity Lab

**Author:** Robert Ursache  
**Date:** 14/5/2026

**Objective:** Demonstrate the ability to generate cryptographic hashes of files, write them to separate files, and compare hashes to determine whether two files are identical or different.

## 📌 Scenario

Two files, `file1.txt` and `file2.txt`, reside in my home directory (`/home/analyst`). Visually, their contents appear identical when displayed with `cat`. However, I need to investigate whether they are truly the same or if there are hidden differences.  

To do this, I will:

1. Display the contents of both files.  
2. Generate SHA‑256 hashes for each file.  
3. Write the hashes to separate files.  
4. Compare the hash files byte‑by‑byte using `cmp`.

This lab simulates a real‑world integrity check – for example, verifying that a downloaded file hasn’t been tampered with, or detecting accidental changes like different line endings or hidden characters.

## 🛠️ Tools & Environment

- **OS:** Linux (Ubuntu/CentOS style)  
- **Shell:** Bash  
- **Commands used:** `ls`, `cat`, `sha256sum`, `cmp`  
- **Hashing algorithm:** SHA‑256 (cryptographically secure)  
- **Key concept:** Even a single byte difference produces a completely different hash.

## 🧪 Step‑by‑Step Execution

### 1. Explore the home directory

```bash
ls /home/analyst

Output:
text

file1.txt  file2.txt

2. Display the contents of both files
bash

cat file1.txt

Output:
text

X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*

bash

cat file2.txt

Output:
text

X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*

At first glance, the contents look identical.
3. Generate SHA‑256 hashes

Even though the cat output looks the same, a hash will reveal any hidden difference (e.g., line endings, trailing spaces, or binary bytes).
bash

sha256sum file1.txt

Output:
text

131f95c51cc819465fa1797f6ccacf9d494aaaff46fa3eac73ae63ffbdfd8267  file1.txt

bash

sha256sum file2.txt

Output:
text

2558ba9a4cad1e69804ce03aa2a029526179a91a5e38cb723320e83af9ca017b  file2.txt

The hash values are completely different. This proves the files are not identical, despite looking the same in plain text.
4. Write hashes to separate files

To preserve and compare the hashes later, I redirect the output to new files.
bash

sha256sum file1.txt >> file1hash
sha256sum file2.txt >> file2hash

5. View the saved hashes
bash

cat file1hash

Output:
text

131f95c51cc819465fa1797f6ccacf9d494aaaff46fa3eac73ae63ffbdfd8267  file1.txt

bash

cat file2hash

Output:
text

2558ba9a4cad1e69804ce03aa2a029526179a91a5e38cb723320e83af9ca017b  file2.txt

6. Compare the hash files byte‑by‑byte with cmp

The cmp command shows exactly where the first difference occurs.
bash

cmp file1hash file2hash

Output:
text

file1hash file2hash differ: char 1, line 1

The difference is at the very first character of the first line – confirming the hashes are different from the start.
✅ Conclusion

Even though cat showed identical text, the SHA‑256 hashes proved the files are different. This is because sha256sum reads the raw bytes of the file, including invisible characters like line endings (LF vs CRLF), trailing spaces, or null bytes.

I successfully demonstrated:

    Generating hashes with sha256sum

    Storing hash outputs in separate files

    Comparing files byte‑by‑byte with cmp

📚 Skills Demonstrated

    Linux command line – listing, reading, and redirecting output

    Cryptographic hashing – using SHA‑256 for integrity verification

    File comparison – detecting differences at the byte level

    Attention to detail – knowing that visual inspection is not enough for security‑critical tasks

🏁 Real‑World Application

These skills are essential for:

    Malware analysis – verifying if a suspicious file matches a known hash

    Forensics – detecting file tampering

    Software distribution – ensuring downloaded files haven’t been altered

    Scripting – automating integrity checks with hash comparisons

This lab gave me hands‑on practice with tools that every security analyst should know.
