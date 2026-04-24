# Git SSH Key Setup on Debian WSL2

First time setting this up on a fresh Debian instance. Writing it down because I always forget the exact sequence.

---

## 1. Generate the key

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

Hit `Enter` to accept the default path (`~/.ssh/id_ed25519`). Add a passphrase or skip it — up to you. I use one.

---

## 2. Start the agent and load the key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

## 3. Copy the public key

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output — it starts with `ssh-ed25519` and ends with your email.

---

## 4. Add it to GitHub

Go to **github.com → Settings → SSH and GPG keys → New SSH key**

- Title: something that tells you which machine this is, e.g. `debian-wsl2`
- Key type: Authentication Key
- Paste the public key

Save it.

---

## 5. Test it

```bash
ssh -T git@github.com
```

If it worked:
```
Hi YOUR_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

If you get a `Permission denied (publickey)` error, see the troubleshooting section below.

---

## 6. Make sure you're using SSH, not HTTPS

Check your remote URL:

```bash
git remote -v
```

If it shows `https://github.com/...` instead of `git@github.com:...`, switch it:

```bash
git remote set-url origin git@github.com:YOUR_USERNAME/devops-articles.git
```

Verify again with `git remote -v`.

---

## 7. Auto-start the agent on login

Without this, you'd need to run `eval "$(ssh-agent -s)"` every time you open a new terminal. Add this to your `~/.bashrc` to handle it automatically:

```bash
cat >> ~/.bashrc << 'EOF'

# SSH agent auto-start
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval "$(ssh-agent -s)" > /dev/null
  ssh-add ~/.ssh/id_ed25519 2>/dev/null
fi
EOF

source ~/.bashrc
```

---

## Troubleshooting

**`Permission denied (publickey)`**
- Run `ssh-add -l` — if it says "no identities", run `ssh-add ~/.ssh/id_ed25519` again
- Double-check the public key on GitHub matches what `cat ~/.ssh/id_ed25519.pub` prints

**`Could not open a connection to your authentication agent`**
- The agent isn't running — run `eval "$(ssh-agent -s)"` first

**`WARNING: UNPROTECTED PRIVATE KEY FILE!`**
- Permissions on the key are too open — fix with:
```bash
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

**Multiple GitHub accounts**
- You'll need an SSH config file. Create `~/.ssh/config`:
```
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519

Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
```
Then clone using the alias instead: `git clone git@github-personal:USERNAME/repo.git`