# Cryptography Basics: Stream Ciphers & Security

This guide explains the foundational concepts of modern stream ciphers, how they work, and how they can be broken if implemented incorrectly.

## Table of Contents

1.  [The Language: ASCII](https://www.google.com/search?q=%231-the-language-ascii)
2.  [The Math: XOR](https://www.google.com/search?q=%232-the-math-xor)
3.  [The Ideal: One-Time Pad (OTP)](https://www.google.com/search?q=%233-the-ideal-one-time-pad-otp)
4.  [The Practical: Pseudo Random Generator (PRG)](https://www.google.com/search?q=%234-the-practical-pseudo-random-generator-prg)
5.  [Vulnerability 1: The Two-Time Pad](https://www.google.com/search?q=%235-vulnerability-1-the-two-time-pad)
6.  [Vulnerability 2: Malleability](https://www.google.com/search?q=%236-vulnerability-2-malleability-no-integrity)
7.  [The Fix: Checksums](https://www.google.com/search?q=%237-the-fix-checksums)

-----

## 1\. The Language: ASCII

Before we can encrypt anything, we must understand how computers store text. Computers don't understand letters (A, B, C); they only understand numbers (0s and 1s).

**ASCII** is the dictionary that translates human text into computer numbers.

  * `A` = 65
  * `B` = 66
  * `Space` = 32

> **Why this matters:** Cryptography is math. You cannot mathematically "add" a password to the letter 'A'. You first convert 'A' to `65`, and then you can do math on the number.

-----

## 2\. The Math: XOR ($\oplus$)

The primary mathematical operation used in stream ciphers is **XOR** (Exclusive OR). It acts like a reversible toggle switch.

**The Golden Rules of XOR:**

1.  **Reversible:** If you XOR a number twice, you get the original number back.
2.  **Cancellation:** Any number XORed with itself becomes Zero.
    $$K \oplus K = 0$$
3.  **Identity:** Any number XORed with Zero stays the same.
    $$M \oplus 0 = M$$

> **Analogy:** Think of XOR like mixing paint.
>
>   * **Plaintext:** Clear water.
>   * **Key:** Red dye.
>   * **Encryption:** Water + Red Dye = Red Water.
>   * **Decryption:** Red Water + (Anti-Red Dye) = Clear water again.

-----

## 3\. The Ideal: One-Time Pad (OTP)

The One-Time Pad is the theoretical "perfect" encryption.

  * **How it works:** You generate a truly random key (Keystream) that is **the exact same length** as your message.
  * **The Process:**
    $$Ciphertext = Message \oplus Key$$
  * **The Condition:** You must **never** use the key again.

> **Analogy:** A bag of Scrabble tiles.
> You and your friend blindly pull random tiles from a bag to create a list. This list is your key. Because the tiles were pulled blindly, there is no pattern for a spy to guess. Once you use the list to send a letter, you burn it.

**Why don't we use it?** It is impractical. To encrypt a 1GB movie file, you need to physically hand your friend a 1GB key file first.

-----

## 4\. The Practical: Pseudo Random Generator (PRG)

Since sharing massive keys is hard, we use a **Pseudo Random Generator (PRG)**.

  * **Definition:** An algorithm that takes a tiny secret key (called a **Seed**) and expands it into a massive stream of numbers that *looks* random.
  * **Stream Cipher:** This is the modern version of the One-Time Pad.
    $$\text{Stream Cipher} \approx \text{OTP} + \text{PRG}$$

> **Analogy:** A book (e.g., *Harry Potter*).
> Instead of carrying a massive random list, Alice and Bob just agree on a "Seed": **"Page 42"**.
> They both open their copy of the book to Page 42 and use the text as their key. It looks random to anyone who doesn't know which book they are using.

-----

## 5\. Vulnerability 1: The Two-Time Pad

The cardinal sin of cryptography is **Key Reuse**.
If you use the same Key (or PRG Seed) to encrypt two different messages, the encryption cancels itself out.

**The Math:**

1.  **Ciphertext 1:** $C_1 = M_1 \oplus K$
2.  **Ciphertext 2:** $C_2 = M_2 \oplus K$

If an attacker intercepts both, they XOR them together:
$$C_1 \oplus C_2 = (M_1 \oplus K) \oplus (M_2 \oplus K)$$

The Keys ($K$) cancel out because $K \oplus K = 0$.
$$\text{Result} = M_1 \oplus M_2$$

The attacker now has the two plain messages mixed together, with **no encryption** standing in the way.

> **Analogy:** Writing on transparencies.
> Imagine encrypting a photo by overlaying a sheet of static noise. If you put the **same** sheet of static noise over a second photo, and then hold both photos up to the light, the static aligns and disappears, revealing both images.

-----

## 6\. Vulnerability 2: Malleability (No Integrity)

Stream ciphers provide **Confidentiality** (Privacy), but they do not provide **Integrity** (Protection from tampering). They are **Malleable**.

If an attacker flips a bit in the ciphertext, the exact same bit flips in the decrypted message.

**The Attack:**

1.  **Message:** "Pay Bob **1**00"
2.  **Attacker:** Does not know the key, but knows "1" is the 9th character.
3.  **Action:** The attacker modifies the 9th character of the ciphertext.
4.  **Result:** When the bank decrypts it, the message becomes "Pay Bob **9**00".

> **Analogy:** A pencil note inside a Ziploc bag.
> The bag is sealed (encrypted), but the bag is flexible (malleable). An enemy can smudge the pencil writing or draw new lines through the plastic without ever opening the bag.

-----

## 7\. The Fix: Checksums

To fix Malleability, we add a **Checksum** (or MAC).
A checksum is a digital fingerprint of the data.

  * **Sender:** Calculates the checksum of the message (e.g., `A7F2`) and sends it along with the message.
  * **Receiver:** Decrypts the message, calculates the checksum, and checks if it matches `A7F2`.
  * **Security:** If an attacker changes "100" to "900", the checksum will no longer match, and the receiver will reject the message.

> **Analogy:** The Total Price on a grocery receipt.
> The security guard checks your cart items and sums up the price. If the sum doesn't match the receipt total, they know you added or removed an item.

-----

## Summary: Putting it all together (WEP Example)

**WEP** was the original Wi-Fi security standard. It failed because it ignored these lessons.

1.  **The Goal:** Encrypt internet traffic.
2.  **The Mistake:** It used a very short "Initialization Vector" (IV) to create keys.
3.  **The Result:** The system ran out of unique keys quickly and started reusing them.
4.  **The Breach:** Attackers waited for a **Two-Time Pad** situation (key reuse), mathematically removed the key, and read the traffic.
5.  **The Malleability:** WEP used a weak checksum (CRC), allowing attackers to modify packets without being caught.
