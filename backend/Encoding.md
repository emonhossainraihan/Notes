There is often significant confusion around the differences between encryption, encoding, hashing, and obfuscation.
## Encoding
The purpose of encoding is to transform data so that it can be properly (and safely) consumed by a different type of system, e.g. binary data being sent over email, or viewing special characters on a web page. The goal is **not** to keep information secret, but rather to ensure that it’s able to be properly consumed.

Encoding transforms data into another format using a scheme that is publicly available so that it can easily be reversed. It does not require a key as the only thing required to decode it is the algorithm that was used to encode it.

Examples: ASCII, Unicode, URL Encoding, Base64

## Encryption
The purpose of encryption is to transform data in order to keep it secret from others, e.g. sending someone a secret letter that only they should be able to read, or securely sending a password over the Internet. Rather than focusing on usability, the goal is to ensure the data cannot be consumed by anyone other than the intended recipient(s).

Encryption transforms data into another format in such a way that only specific individual(s) can reverse the transformation. It uses a key, which is kept secret, in conjunction with the plaintext and the algorithm, in order to perform the encryption operation. As such, the ciphertext, algorithm, and key are all required to return to the plaintext.

Examples: AES, Blowfish, RSA

## Hashing
Hashing serves the purpose of ensuring integrity, i.e. making it so that if something is changed you can know that it’s changed. Technically, hashing takes arbitrary input and produce a fixed-length string that has the following attributes:
- The same input will always produce the same output.
- Multiple disparate inputs should not produce the same output.
- It should not be possible to go from the output to the input.
- Any modification of a given input should result in drastic change to the hash.
Hashing is used in conjunction with authentication to produce strong evidence that a given message has not been modified. This is accomplished by taking a given input, hashing it, and then signing the hash with the sender’s private key.

When the recipient opens the message, they can then validate the signature of the hash with the sender’s public key and then hash the message themselves and compare it to the hash that was signed by the sender. If they match it is an unmodified message, sent by the correct person.

Examples: SHA-3, MD5 (Now obsolete), etc.

## Obfuscation
The purpose of obfuscation is to make something harder to understand, usually for the purposes of making it more difficult to attack or to copy.

One common use is the the obfuscation of source code so that it’s harder to replicate a given product if it is reverse engineered.

It’s important to note that obfuscation is not a strong control (like properly employed encryption) but rather an obstacle. It, like encoding, can often be reversed by using the same technique that obfuscated it. Other times it is simply a manual process that takes time to work through.

Another key thing to realize about obfuscation is that there is a limitation to how obscure the code can become, depending on the content being obscured. If you are obscuring computer code, for example, the limitation is that the result must still be consumable by the computer or else the application will cease to function.

Examples: JavaScript Obfuscator, ProGuard

## Summary
- **Encoding** is for maintaining data usability and can be reversed by employing the same algorithm that encoded the content, i.e. no key is used.
- **Encryption** is for maintaining data confidentiality and requires the use of a key (kept secret) in order to return to plaintext.
- **Hashing** is for validating the integrity of content by detecting all modification thereof via obvious changes to the hash output.
- **Obfuscation** is used to prevent people from understanding the meaning of something, and is often used with computer code to help prevent successful reverse engineering and/or theft of a product’s functionality.

Notes

1. One might ask when obfuscation would be used instead of encryption, and the answer is that obfuscation is used to make it harder for one entity to understand (like a human) while still being easy to consume for something else (like a computer). With encryption, neither a human or a computer could read the content without a key.
