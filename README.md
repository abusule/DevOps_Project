If you want to connect to GitHub using SSH, you need to generate an SSH key, add the public key to GitHub, and then test the connection.

1. Check if you already have an SSH key
ls -la ~/.ssh

Look for files such as:

id_ed25519
id_ed25519.pub
2. Generate a new SSH key

Recommended:

ssh-keygen -t ed25519 -C "your-email@example.com"

You'll see:

Enter file in which to save the key (/home/user/.ssh/id_ed25519):

Press Enter to accept the default.

Then:

Enter passphrase:

You can enter a passphrase or press Enter for none.

This creates:

~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
Important
id_ed25519 → private key — NEVER share this
id_ed25519.pub → public key — this is what you give GitHub
3. Start the SSH agent
eval "$(ssh-agent -s)"

You should get something similar to:

Agent pid 1234
4. Add your private key to the SSH agent
ssh-add ~/.ssh/id_ed25519
5. Display your public key
cat ~/.ssh/id_ed25519.pub

You'll see something like:

ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... your-email@example.com

Copy the entire line.

Do not copy or share:

~/.ssh/id_ed25519

Only the .pub key should be added to GitHub.

6. Add the key to GitHub

Go to GitHub → Settings → SSH and GPG keys → New SSH key.

Give it a name such as:

My Linux PC

Paste your public key and save it.

7. Test the SSH connection

Run:

ssh -T git@github.com

The first time, you may see:

Are you sure you want to continue connecting (yes/no/[fingerprint])?

Type:

yes

If everything is configured correctly, GitHub will authenticate you.

8. Clone a repository using SSH

Instead of HTTPS:

git clone https://github.com/USERNAME/REPOSITORY.git

Use SSH:

git clone git@github.com:USERNAME/REPOSITORY.git

For example:

git clone git@github.com:ABC/myproject.git

        OR 
git remote add origin git@github.com:ABC/my_Project.git
git branch -M main
git push -u origin main

9. If you already have a Git repository

Check the current remote:

git remote -v

If it shows HTTPS:

origin  https://github.com/USERNAME/REPOSITORY.git

Change it to SSH:

git remote set-url origin git@github.com:USERNAME/REPOSITORY.git

Verify:

git remote -v

You should now see:

origin  git@github.com:USERNAME/REPOSITORY.git

Then you can:

==========================================================
The complete process to remember
ssh-keygen -t ed25519 -C "your-email@example.com"

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_ed25519

cat ~/.ssh/id_ed25519.pub

Copy the .pub key → add it to GitHub → test:

ssh -T git@github.com

Then use SSH Git URLs:

git clone git@github.com:USERNAME/REPOSITORY.git
🔐 Important security rule

Never give anyone your private key:

❌ ~/.ssh/id_ed25519
=====================================================================

Your public key is safe to share:

✅ ~/.ssh/id_ed25519.pub
