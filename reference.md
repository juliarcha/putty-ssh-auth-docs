# Quick reference: SSH Key Authentication

> * [Overview](#overview)
> * [Control panel and components](#control-panel-and-components)
> * [Troubleshooting](#troubleshooting)
> * [Quick action summary](#quick-action-summary)

---

## Overview

This document provides a quick reference for the components, settings, and troubleshooting of the SSH key authentication process in PuTTY.

### Authentication flow

The diagram below illustrates the key authentication process, from the connection attempt to the granting of access.

![Diagram representing the authentication flow. On the computer side on the left, we have the client "PuTTY.exe" and the private key in the ".ppk" file. On the remote server side on the right, we have the "authorized_keys" file. The first step is a connection attempt from the computer to the server. Second, the server sends an encrypted challenge to the computer. Then, the computer solves the challenge and sends the response to the server. If the answer is correct, the server grants access to the computer.](/images/diagram.png)

## Control panel and components

The key authentication process involves five main components. The table below describes each one and its key settings:

| **Component**  | **Description** | **Main configurations/rules**  |
|----------------|-----------------|------------------------------------|
| **Private key (`.ppk`)** | Your secret, unique and non-transferable credential. Used by the PuTTY client to prove your identity to the server through a digital signature. | <ul><li>Must be stored in a secure, restricted-access location and **never shared**.</li><li>Can be protected by a passphrase for additional security.</li></ul> |
| **Public key** | Your public digital identity, derived from the private key. Used by the server to verify the signature created by your private key. | <ul><li>It must be copied and pasted into the `authorized_keys` file on the server.</li><li>The format should be a single, long line of text.</li></ul> |
| **PuTTY Key Generator (`PuTTYgen.exe`)** | Tool used to create and manage cryptographic key pairs that form your digital identity. | <ul><li>Click on the **Generate** button to create a new key pair.</li><li>Click on **Save private key** to save your confidential `.ppk` file.</li><li>Copy the generated public key to install on the server.</li></ul> |
| **PuTTY (`PuTTY.exe`)** | Main tool used to configure and start the SSH connection, using the private key to authenticate. | <ul><li>**Connection > Data > Auto-login username**: Fill in with your computer username for faster access.</li><li>**Connection > SSH > Auth > Credentials**: Use the **Browse…** button to point to your `.ppk` file. </li></ul> |
| **`authorized_keys`** | Text file on the server that acts as a whitelist, containing all public keys authorized to connect. | <ul><li>**Format**: Plain text (ANSI or UTF-8 encoding) containing one public key per line.</li><li>**Name**: The file must be named `authorized_keys` without the `.txt` extension.</li></ul> |

## Troubleshooting

In a SSH key authentication configuration, small details can prevent a successful connection. This section covers the most common errors, explaining their likely causes and recommended remediation actions.

| **Common error**  | **Probable cause**  | **Recommended action**   |
|-------------------|---------------------|---------------------------|
| The server refused your key | The server found the `authorized_keys` file but the public key offered by PuTTY doesn't match a valid key on the list. The most common causes are:<br> <br><ul><li>Copy/paste error</li><li>File formatting</li><li>Inappropriate permissions</li></ul> | <ul><li>Load your private key on PuTTY Key Generator, copy the public key and replace the content of the `authorized_keys` file.</li><li>Make sure that the public key is in a single line and the `authorized_keys` file doesn't have any extension.</li><li>Verify that only your user, `SYSTEM` and `Administrators` have access permissions for the `authorized_keys` file on the server.</li></ul> |
| The connection closes abruptly | The problem is on the PuTTY client, that attempts another authentication method before the public key, and this method fails in a way that ends the connection prematurely. | <ul><li>Verify if Pageant is running in the System Tray and close it.</li><li>During the session configuration in PuTTY, go to **Connection > SSH > Auth** and disable the priority of the GSSAPI method.</li><li>Remember to **save the session** after any changes.</li></ul> |
| PuTTY ignores the key and asks for the password directly | PuTTY was not configured correctly to use a key or can't find the `.ppk` file. The most common cause is that the session configuration were not saved. | <ul><li>Open PuTTY, load your saved session and go to **Connection > SSH > Auth > Credentials**.</li><li>Make sure that the path to your private key file is correct. Use the **Browse…** button to select your `.ppk` file again.</li><li>Go back to the **Session** category and click the **Save** button to save your changes on the session.</li></ul> |

> **INFO**
> 
> As a general rule, the source of truth for most authentication errors are the **SSH server logs**. On Windows, these can be found in the Event Viewer (`eventvwr.msc`) under **Applications and Services Logs > OpenSSH > Operational**.

## Quick action summary

| **Expected action**          | **Tool/Location**     | **Notes**      |
|------------------------------|-----------------------|----------------|
| Generate a new key pair  | PuTTY Key Generator (`PuTTYgen.exe`)          | Use RSA with 2048 or 4096 bits.           |
| Authorize a key on the server | `authorized_keys` file   | Paste the public key in a single line.    |
| Connect using a key       | PuTTY (`PuTTY.exe`): Connection > SSH > Auth > Credentials | Point to the path of your `.ppk` file.       |
| Investigate errors thoroughly    | Server: OpenSSH logs  | On Windows, check the Event Viewer. |

---

> * For deeper understanding of the concepts behind the SSH Key Authentication process, see our [SSH Key Authentication](explanation.md) documentation.
> * For detailed information about the usage of SSH keys in PuTTY, visit our step-by-step guide on [How to set up SSH Key Authentication in PuTTY](tutorial.md).


---
