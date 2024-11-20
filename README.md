# github_ssh_key_issue
Steps to fix issue when  SSH key isn't being recognized by GitHub. 

---

### **Step 1: Ensure Your SSH Key Exists**
Run this command to list your SSH keys:
```bash
ls -al ~/.ssh
```
Look for a public key file (e.g., `id_rsa.pub` or `id_ed25519.pub`). If no such file exists, generate one:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- Use your GitHub email address.
- Save the key to the default location (`~/.ssh/id_ed25519`).
- If prompted, add a passphrase or press `Enter` to leave it empty.

---

### **Step 2: Add Your SSH Key to the SSH Agent**
Start the SSH agent:
```bash
eval "$(ssh-agent -s)"
```

Add your private key to the agent:
```bash
ssh-add ~/.ssh/id_ed25519
```

---

### **Step 3: Add Your Public Key to GitHub**
1. Copy your public key to the clipboard:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
2. Log in to GitHub and go to **Settings** > **SSH and GPG keys**.  
   [Direct Link to SSH Keys Settings](https://github.com/settings/keys).

3. Click **New SSH Key**:
   - **Title**: Add a descriptive name (e.g., "My Laptop").
   - **Key**: Paste the copied public key.
4. Click **Add SSH key**.

---

### **Step 4: Test Your SSH Connection**
Test if your SSH key works:
```bash
ssh -T git@github.com
```

You should see a message like:
```plaintext
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

### **Step 5: Clone the Repository**
Now, clone your repository:
```bash
git clone git@github.com:your-username/<repo_name>.git
```

---

### **Troubleshooting**
If you still see the error, try these steps:

#### 1. **Check Your SSH Key is Added to GitHub**
Verify that the correct public key is listed on your [GitHub SSH Keys Page](https://github.com/settings/keys).

#### 2. **Check SSH Key Is Loaded**
Ensure your key is loaded into the SSH agent:
```bash
ssh-add -l
```
If no key is listed, re-add it:
```bash
ssh-add ~/.ssh/id_ed25519
```

#### 3. **Edit SSH Config (Optional)**
If you manage multiple keys or custom configurations, edit/create a config file:
```bash
nano ~/.ssh/config
```
Add:
```plaintext
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
```
Save and close the file, then test again.

#### 4. **Debug SSH**
Use this command to debug your connection:
```bash
ssh -vT git@github.com
```
It will show detailed output about what's going wrong.

---
