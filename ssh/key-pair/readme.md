# SSH key-pair configuration
A more secure method for SSH authentication involves using SSH keys. With SSH keys, you generate a key pair (public and private key), and the public key is placed on the server you want to access. The private key remains on your local machine and is used for authentication.

Here's a brief overview of how to set up SSH key-based authentication:
1. Generate SSH Key Pair: Use the ssh-keygen command to generate a key pair on your local machine.
    ```bash
    ssh-keygen -t rsa
    ```
Follow the prompts to generate the key pair. This will create a private key (usually id_rsa) and a public key (usually id_rsa.pub).

2. Copy Public Key to Server: Append the contents of the public key (id_rsa.pub) to the ~/.ssh/authorized_keys file on the server you want to access. If the file doesn't exist, create it.
    ```bash
    cat ~/.ssh/id_rsa.pub | ssh username@server 'cat >> ~/.ssh/authorized_keys'
    ```
3. Use SSH Agent (Optional): You can use an SSH agent to manage your private keys securely. On Windows, you can use ssh-agent from OpenSSH.
    ```bash
    ssh-agent
    ssh-add ~/.ssh/id_rsa
    ```
4. SSH Connection: Once the public key is installed on the server and the private key is loaded into the SSH agent, you can simply use ssh without providing a password.
    ```bash
    ssh username@server
    ```