# SSH key-pair configuration

A more secure method for SSH authentication is **SSH key-based authentication**. This method uses a **key pair** (public and private key) for authentication instead of passwords.

## 1. Generate an SSH Key Pair  
Run the following command on your **local machine** to create a key pair:

For **stronger RSA encryption** (recommended):  
```bash
ssh-keygen -t rsa -b 4096
```
For **better security & performance**, use Ed25519 (if supported):
```bash
ssh-keygen -t ed25519
```
Follow the prompts. This creates:
* A **private key** (`id_rsa` or `id_ed25519`) — **Keep this secure!**
* A **public key** (`id_rsa.pub` or `id_ed25519.pub`) — **This goes on the server.**

## 2. Copy the Public Key to the Server  
Use `ssh-copy-id` to securely add the public key to the server:
```bash
ssh-copy-id username@server
```
If `ssh-copy-id` is unavailable, use:
```bash
cat ~/.ssh/id_rsa.pub | ssh username@server 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
```
Ensure correct permissions:
```bash
ssh username@server "chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys"
```
## 3. Use SSH Agent (Optional)  
To avoid entering the private key password every time, use an SSH agent:  
On **Linux/macOS:**
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```
On **Windows PowerShell:**
```pwsh
Start-Service ssh-agent
ssh-add ~/.ssh/id_rsa
```
## 4. Connect to the Server  
Once the public key is installed, connect using SSH without a password:
```bash
ssh username@server
```