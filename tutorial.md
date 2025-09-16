# How to set up SSH Key Authentication in PuTTY

> * [Requirements](#requirements)
> * [Step 1: Generating a key pair with PuTTY Key Generator](#step-1-generating-a-key-pair-with-putty-key-generator)
> * [Step 2: Installing a public key on the server](#step-2-installing-a-public-key-on-the-server)
> * [Step 3: Configuring a new session in PuTTY](#step-3-configuring-a-new-session-in-putty)
> * [Step 4: Establishing a connection with key authentication](#step-4-establishing-a-connection-with-key-authentication)
> * [Adding an extra safety layer with passphrase](#adding-an-extra-safety-layer-with-passphrase)

---

Key authentication is the most professional and reliable method to establish a secure connection with SSH servers. This document will guide you through each step of the key authentication process in PuTTY, using the SSH protocol and a cryptographic key pair. 

By the end of this guide, you will be able to generate an SSH key pair and configure PuTTY to establish a secure connection with your server, without needing to enter a password at each access. 

## Requirements

* The PuTTY package must be installed on your computer, including the `PuTTY.exe` and `PuTTYgen.exe` files. You can download the full package from [PuTTY's website](https://putty.org/).
* Access to an SSH server is also required. For this guide, we'll use the `localhost` connection in a Windows environment as an example.
* Login credentials (username and password) for an initial connection to the server.

## Step 1: Generating a key pair with PuTTY Key Generator

Your first task is to create the keys that will be your new digital identity. Follow these steps to generate a key pair with PuTTY Key Generator:

1. Open the **PuTTY Key Generator** (`PuTTYgen.exe`) program.

2. On the main interface, click the **Generate** button in the **Actions** section.

    ![PuTTY Key Generator interface with the "Generate" button highlighted in red inside the "Actions" section.](/images/puttygen-interface.png)

3. Move your cursor randomly around the blank area in the **Key** section, below the progress bar. This movement generates the randomness needed to calculate and create a strong cryptographic key. Continue until the progress bar is complete. By the end of the process, you'll have generated:
    * A public key
    * A key fingerprint
    * A key comment

4. Once the key pair is generated, click the **Save private key** button to store your secret key. An alert will ask if you want to save the key without a passphrase. For this initial setup, click **Yes**.

    !["Actions" section in PuTTY Key Generator's interface with the "Save private key" button highlighted in red.](/images/save-private-key.png)

    > Later in this document, we explain the process of [Adding an extra safety layer with passphrase](#adding-an-extra-safety-layer-with-passphrase).

5. Choose a secure location on your computer and save the `.ppk` (PuTTY Private Key) file with a descriptive name (e.g., `my_private_key.ppk`).

    > **IMPORTANT**
    > 
    > Never share your `.ppk` file with anyone.

6. Next, you need the public key to install on the server. To get it directly from PuTTY Key Generator, select all the content from "Public key for pasting into OpenSSH authorized_keys file" in the **Key** section and copy it (`Ctrl + C`). 

    > **TIP**
    >
    > If necessary, you can retrieve the public key by clicking the **Load** button and opening your saved `.ppk` file. Your keys will be displayed again in the Key section.

7. Paste your public key into a temporary Notepad file for the next step.

## Step 2: Installing a public key on the server

The second step is telling the server to trust your new key.

1. Open the PuTTY (`PuTTY.exe`) program and connect to your `localhost` server in the **Session** category. For that, you'll use the port 22 and the SSH connection type.

    ![PuTTY Configuration's interface with the "Session" category opened and the "Specify the destination you want to connect to" section highlighted in red.](/images/putty-config.png)

2. The terminal will prompt you for your credentials. When you see the `login as:` prompt, type your username and press Enter. At the `password:` prompt, type your password and press Enter.

    > **NOTE**
    > 
    > When typing the password, no characters will appear on the screen for security purposes. This is the standard feature for passwords.

3. Once logged in, you need to create a special folder to store authorized keys. Run the following command to create a `.ssh` directory in your local server: 

    ```shell
    mkdir .ssh
    ```

    > **TIP**
    > 
    > You can verify that the `.ssh` directory was successfuly created by running this command: 
    >
    > ```shell
    > dir .ssh
    > ```
    >
    > If it was a success, you will see the directory's content, which will be empty until the next step. If not, you will receive a "File not found" message and should repeat this step.

4. Inside the new `.ssh` directory, create a file for the authorized keys by running this command:

    ```shell
    notepad .ssh/authorized_keys
    ```

    This will open the Notepad program, where you will paste the public key copied from PuTTY Key Generator. 
    
5. Save the file (`Ctrl + S`), then close the Notepad and your current PuTTY session.

## Step 3: Configuring a new session in PuTTY

The next step is teaching PuTTY how to use your new identity by configuring a new session. This is the process you should follow:

1. Run PuTTY (`PuTTY.exe`) again to open a new configuration window and go to the **Session** category.

2. In the **Host Name** field, type your server's address (e.g., `localhost`).

    > **INFO**
    > 
    > You can go to **Connection > Data** in the category menu on the left and enter your username in the **Auto-login username** field. With this setting, your PuTTY client will automatically provide your username to the server, saving you from having to type it manually in the prompt.

3. Next, point PuTTY to your private key. In the category menu, go to **Connection > SSH > Auth > Credentials**. Click the **Browse…** button to the right of the **Private key file for authentication** field and select the `.ppk` file saved previously.

4. To reuse this configuration, return to the **Session** category. In the **Saved Sessions** field, enter a descriptive name for the session (e.g., `My Local Server with Key`) and click the **Save** button.

## Step 4: Establishing a connection with key authentication

Now, you can check the result by connecting to the server with your new configuration:

1. In the PuTTY **Saved Sessions** field, double-click the session you saved in the previous step.

2. When the terminal window opens, you will be logged in to the server automatically, without any password requested. The server used your public key to validate the signature from the private key presented by PuTTY, confirming your identity securely.

## Adding an extra safety layer with passphrase

For maximum security in your connection, you can protect your private key with a password, known as **passphrase**.

The passphrase is responsible for encrypting your private key file. With a passphrase, even if your `.ppk` file is compromised, it cannot be used without the password. Altought it is optional, adding a passphrase is highly recommended to increase the security of your credentials. 

1. When generating a key pair with PuTTY Key Generator, fill in the **Key passphrase** and **Confirm passphrase** fields with a strong password before saving the private key file.

2. When you connect using your key in PuTTY, the terminal will ask for your passphrase. Type the passphrase you created to complete the login. 

---

> * For deeper understanding of the concepts behind the SSH Key Authentication process, visit our [SSH Key Authentication](explanation.md) documentation.
> * For quick reference on settings, commands, and troubleshooting, see our [Quick reference: SSH Key Authentication](reference.md).


---
