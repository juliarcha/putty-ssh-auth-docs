# SSH Key Authentication

> * [The importance of SSH in secure authentication](#the-importance-of-ssh-in-secure-authentication)
> * [Public and private key cryptography](#public-and-private-key-cryptography)
> * [Tools required for authentication in PuTTY](#tools-required-for-authentication-in-putty)
> * [Authentication process in PuTTY](#authentication-process-in-putty)
> * [Benefits](#benefits)

---

For many tech professionals and enthusiasts, PuTTY is an essential tool for connecting local computers to servers around the world. It uses the **SSH protocol** to establish a secure connection, allowing users to manage systems, transfer files, and execute commands remotely. 

In a common scenario, you would use a login and a password to authenticate the connection using PuTTY's interface. However, the password-based method has significant security and efficiency limitations. While the SSH protocol encrypts the password during transmission, the password itself remains a vulnerable target. The **SSH Key Authentication** feature addresses this weakness, providing a robust method for establishing secure connections to remote servers.

This document explains the concept behind SSH Key Authentication, one of PuTTY's powerful functionalities, responsible for replacing the vulnerability of a password with a cryptographic proof of identity. 

After this reading, you will understand why this solution is the professional standard for secure remote access.

## The importance of SSH in secure authentication

Key authentication is the most recommended way to connect to servers, surpassing password-based authentication. By combining key authentication with the SSH protocol, you can ensure secure access to servers in any location. 

To understand the importance of keys, the first step is learning how the **SSH protocol** works. Secure Shell is a network protocol that enables secure communication between two computers in a network with safety limitations: the internet. Based on the principles of **security** and **flexibility**, the SSH protocol provides three fundamental guarantees:

1. **Confidentiality**: Since the data traffic is encrypted, the information shared between computers cannot be accessed by unauthorized users.
2. **Integrity**: SSH ensures that the data sent is not altered along the way.
3. **Authentication**: SSH verifies the identity of the user trying to connect to ensure only authorized users have access.

Passwords and keys play their role in the authentication pillar. Although the SSH protocol supports password authentication, it was designed for a much more robust, secure, and efficient method, which addresses the inherent weaknesses of passwords, such as phishing, brute-force attacks, and reuse across multiple systems.

## Public and private key cryptography

The key authentication process is a system known as **asymmetric cryptography**. It relies on a mathematically linked pair of keys, which is called a **cryptographic pair**, and follows a strict rule: what the public key encrypts, only the corresponding private key can decrypt.

Let's understand what are public and private keys:

* **Public key**
  * Can be shared openly without compromising security.
  * Used to encrypt data or verify a digital signature.

* **Private key** 
  * Must be kept secret and secure.
  * Used to decrypt data or create a digital signature.

> **INFO**
> 
> The mathematical link between the cryptographic pair makes it impossible to derive the private key from the public key. This property is what makes the system secure, so that any operation performed by one key can only be reversed by its counterpart.

## Tools required for authentication in PuTTY

To implement a key system, PuTTY works with two different tools:

* **`PuTTYgen.exe`**: PuTTY's auxiliary tool whose sole purpose is to securely generate your cryptographic pair. It works as a key factory.
* **`PuTTY.exe`**: PuTTY's main program, which works as a connection client. Instead of using a password to connect, PuTTY will act as the bearer of your private key, using it to prove your identity to the server.

## Authentication process in PuTTY

When you configure and use keys for authentication, a secure and invisible dialogue takes place between PuTTY and the SSH server. Below, we go through all the steps of this conversation and describe how the authentication process works in PuTTY:

1. **Preparation**: To start a dialogue between the tool and the server, you'll copy the public key and install it on the server using a specific file for authorized keys.
2. **Connection**: You establish a connection with PuTTY, which is configured to use your private key.
3. **Challenge**: When the SSH server sees your registered public key, it creates a random, unique message — called a "challenge" — and encrypts it using your public key. This encrypted challenge is then sent to your PuTTY client.
4. **Proof**: Your PuTTY client uses the private key stored on your computer to successfully decrypt the challenge.
5. **Validation**: PuTTY sends the decrypted answer back to the server, which confirms that the answer is correct. Since only the bearer of the private key could have decrypted the message, it grants access.

The entire process happens in a fraction of a second, but the most important concept to understand is that your private key — the secret of your identity — **never travels across the network**.

## Benefits

By incorporating this authentication method, you’ll instantly see improvements in your workflow:

* **Enhanced security**: Cryptographic keys are much more complex than any human-created password, making brute-force attacks infeasible.
* **Simplified login and automation**: After the initial setup, you can connect to servers without needing a password at each login, which is ideal for improving productivity and automating scripts.
* **Scalable access management**: In an environment with multiple users and servers, managing access becomes simpler. To grant access, you only need to add a public key. If you want to revoke access, you can simply remove it.

---

> * For detailed information about the usage of SSH keys, visit our step-by-step guide on [How to set up SSH Key Authentication in PuTTY](tutorial.md).
> * For quick reference on settings, commands, and troubleshooting, see our [Quick reference: SSH Key Authentication](reference.md).

---


