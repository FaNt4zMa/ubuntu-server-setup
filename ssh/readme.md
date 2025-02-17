# Add user to sudoers
When running a command with sudo remotely via SSH, it might prompt for a password, which can cause issues with automation scripts.

To enable running sudo commands without a password prompt via SSH, you need to configure sudo on the server to allow the specified user (fantaz in this case) to run commands without entering a password.

Here's how you can do it:
1. Edit sudoers file: On the server, run the command `sudo visudo` to open the sudoers file.

2. Add a sudoers entry: Add the following line to the sudoers file, replacing <username> with your username:
    ```bash
    <username> ALL=(ALL) NOPASSWD: ALL`
    ```
3. Save and exit the sudoers file.