# 🔐 Cryptography 101: From XOR to Stream Ciphers

Welcome to a digest of foundational cryptography concepts. This guide breaks down how modern encryption (specifically **Stream Ciphers**) evolved from the mathematically perfect **One-Time Pad**.

It progresses from the basic math (Binary/XOR) to the theoretical ideal (OTP), and finally to the practical real-world implementation (PRNGs).

---

## 🧱 1. The Building Blocks: Binary & XOR

Before understanding encryption, we must understand the tools used to scramble data.

### Binary
Computers don't see letters or images; they only see **0**s and **1**s (bits).
* **0** = False / Off
* **1** = True / On

### XOR (Exclusive OR)
This is the "magic" operator of cryptography. Unlike normal addition, XOR is strictly logical.
* **Rule:** If bits are the **same**, result is **0**. If bits are **different**, result is **1**.

| A | B | Result ($A \oplus B$) |
| :--- | :--- | :--- |
| 0 | 0 | **0** |
| 1 | 1 | **0** |
| 0 | 1 | **1** |
| 1 | 0 | **1** |

### The "Reversibility" Property
This is why we use XOR. It acts like a toggle switch. If you apply the same key twice, you get your original data back.

$$Message \oplus Key = Ciphertext$$
$$Ciphertext \oplus Key = Message$$

---

## 🏆 2. The "Perfect" System: One-Time Pad (OTP)

The One-Time Pad is the theoretical "Holy Grail" of cryptography. It is mathematically unbreakable.

### How it works
1. **The Message:** Your secrets (converted to binary).
2. **The Key (Pad):** A list of **truly random** bits (noise).
3. **The Rule:** The Key must be **at least as long** as the Message.

### The Encryption
You simply XOR the Message with the Random Key.
Because the Key is truly random, the result (Ciphertext) is also truly random. Even with infinite computing power, a hacker cannot crack it because every possible decryption is equally likely.

### Why we don't use it everywhere (The Problem)
Logistics.
* If you want to send a 10GB file, you need to physically hand your friend a 10GB random key beforehand.
* You can **never reuse** the key (or security collapses).

---

## ⚙️ 3. The "Practical" Solution: Stream Ciphers

To solve the problem of carrying around giant keys, we use **Stream Ciphers**. This technique "fakes" a One-Time Pad using a computer algorithm.

### The Concept: PRNG
A **Pseudo-Random Number Generator (PRNG)** is a mathematical machine.
* **True Random:** Coin flips (Unpredictable).
* **Pseudo Random:** Calculated by a formula (Predictable if you know the starting number).

### The Analogy: The Blender 🌪️
* **The PRNG (The Machine):** A public algorithm (like ChaCha20 or RC4). Everyone has this software.
* **The Seed (The Key):** A short secret password (e.g., `my_secret_password`). This is the only thing you keep secret.
* **The Keystream (The Smoothie):** The machine takes the Seed and churns out an infinite stream of random-looking bits.

### The Architecture
Instead of carrying a giant pad, Alice and Bob just share a small **Seed**. They both put the Seed into their PRNG to generate the exact same **Keystream**.

---

## 🚀 4. Putting It All Together: The Workflow

We do **not** put the message into the PRNG (because PRNGs are one-way "blenders"—you can't un-blend a smoothie to get the fruit back).

Instead, we use the PRNG to **stretch** our short key, and XOR to **lock** the message.

### The Formula
1. **Expand Key:** $$Key \xrightarrow{PRNG} Infinite Keystream$$
2. **Encrypt:** $$Message \oplus Keystream = Ciphertext$$
3. **Decrypt:** $$Ciphertext \oplus Keystream = Message$$

---

## 📝 5. Concrete Example

Let's encrypt the letter "A" using a Stream Cipher workflow.

**1. The Setup**
* **Message (Plaintext):** "A" $\rightarrow$ Binary: `01000001`
* **Secret Key (Seed):** `9` (Just a starting number).

**2. Generating the Keystream (The PRNG)**
* The computer runs `PRNG(9)` and spits out random bits.
* **Keystream:** `11011010`

**3. Encryption (XOR)**
We mix the Message and the Keystream.

```text
   01000001  (Message "A")
⊕  11011010  (Keystream)
   --------
   10011011  (Ciphertext)
