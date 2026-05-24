# 🐱 Meow — CTF Writeup

> **Platform:** HackTheBox | **Category:** Starting Point | **Difficulty:** ⭐ Very Easy | **Written by:** Chamith De Silva

---

## 🧠 What This Machine Is About

Meow is the very first Starting Point machine on HackTheBox. It introduces one of the oldest and most insecure remote login protocols called Telnet. The machine teaches you how to find open ports, connect to a remote service, and log in without a password due to a misconfiguration.

> 💡 Think of this machine like a very old building with a door that has no lock. Anyone who knows the building exists can just walk straight in!

---

## 📌 Key Concept — What is Telnet?

| Thing | What it means (simple) |
|-------|------------------------|
| Telnet | A very old protocol for remotely logging into another computer |
| Port 23 | The default port Telnet runs on — always look for this |
| No encryption | Telnet sends everything as plain text — very insecure! |
| Misconfiguration | Admin left the root account with no password — we get in for free! |

> 💡 Telnet is like a phone call with no security — anyone listening on the network can hear everything you type, including passwords! That is why SSH replaced it. But old systems still use it sometimes.

---

## 🛠️ Tools Used

| Tool | Purpose | Simple Example |
|------|---------|---------------|
| `ping` | Check if the target machine is alive | Like calling someone's phone to see if they are home |
| `nmap` | Scan for open ports and services | Like checking which doors are open in a building |
| `telnet` | Connect to the remote machine | Like using an old walkie-talkie to talk to the server |
| `ls` | List files in current directory | Like looking inside a folder on your computer |
| `cat` | Read the contents of a file | Like opening and reading a text file |

---

## ⚙️ Full Attack — Step by Step

### ✅ STEP 1 — Connect to VPN

```bash
sudo openvpn {vpn_file_name}.ovpn
```

> 💡 HackTheBox machines are inside a private network. The VPN is like a special pass that lets you enter that network from outside. Always connect first before doing anything!

> 📝 **my Note:** I had to change to a different VPN region — after that it worked fine!

---

### ✅ STEP 2 — Spawn the Machine and Get IP Address

On HackTheBox, click the **Spawn Machine** button. After a minute or two, HackTheBox will give you the target IP address. This is the address of the machine you are going to attack.

To check your own IP address in Kali Linux, run:

```bash
ip a
```

> 💡 ip a shows all the network interfaces on your machine. Look for the **tun0** interface — that is your VPN IP address assigned by HackTheBox!

---

### ✅ STEP 3 — Confirm Target is Alive

```bash
ping {target_IP}
```

Press `Ctrl+C` after 2-3 replies. If you see replies coming back, the target is alive and reachable!

> 💡 Think of ping like throwing a small ball at the target machine. If the ball comes back, you know someone is there! If nothing comes back, the machine is either off or unreachable.

| What you see | What it means |
|---|---|
| 64 bytes from {IP}: icmp_seq=1... | Target is alive and reachable ✅ |
| Request timeout / no response | Target is unreachable ❌ — check VPN connection! |

---

### ✅ STEP 4 — Scan with Nmap

```bash
nmap {target_IP}
```

This scans the target machine to find which ports are open and what services are running on them.

> 💡 Nmap is like a security guard walking around a building checking every door and window, writing down which ones are open and what is behind them!

**What you find:**

```
PORT   STATE  SERVICE
23/tcp open   telnet
```

Port 23 is open and Telnet is running! This is our way in.

| Port | Service | What it means |
|------|---------|--------------|
| 23 | Telnet | Old remote login service — and it has no password protection! |

> 📝 **RULE:** When you see port 23 open, always try Telnet with common usernames and no password!

---

### ✅ STEP 5 — Connect via Telnet

```bash
telnet {target_IP}
```

> 💡 Telnet is like knocking on the door of the server and asking to come in. If the server has no security configured, it will just let you in!

After running this command, Telnet will ask you for a login. Try the most common default username:

```
login: root
```

When asked for a password, just press Enter (leave it blank):

```
password: (just press Enter)
```

> 💡 Many systems have a root account set up with no password by mistake. Think of it like a building manager who forgot to put a lock on the master key cabinet — anyone can just open it!

Your terminal changes to:

```
root@Meow:~#
```

> 👑 You are now inside the machine as root!

---

### ✅ STEP 6 — Find and Read the Flag

Now that you are inside the machine, list the files in the current directory:

```bash
ls
```

You will see a file called flag.txt. Read it using:

```bash
cat flag.txt
```

> 💡 cat is like opening a text file and reading it out loud. It prints the contents of any file directly to your terminal!

> 🏁 Copy the flag value and submit it on HackTheBox. Machine owned!

---

## 🗺️ Simple Attack Summary

| Step | Command | What it does |
|------|---------|-------------|
| 1 | `sudo openvpn {file}.ovpn` | Connect to HackTheBox VPN |
| 2 | `ip a` | Check your own VPN IP address |
| 3 | `ping {target_IP}` | Confirm target machine is alive |
| 4 | `nmap {target_IP}` | Find open ports — discover port 23 (Telnet) |
| 5 | `telnet {target_IP}` | Connect to the target machine via Telnet |
| 6 | `login: root / password: (blank)` | Log in with no password |
| 7 | `ls` | List files in the directory |
| 8 | `cat flag.txt` | Read and capture the flag! 🏁 |

---

## 🔑 Key Lessons to Remember

| # | Lesson | Why It Matters |
|---|--------|---------------|
| 1 | Always connect VPN first | Without VPN you cannot reach HackTheBox machines |
| 2 | Use ping to confirm target is alive | No point scanning a machine that is off or unreachable |
| 3 | Port 23 = Telnet | Always try Telnet with default credentials when you see port 23 |
| 4 | Try root with no password first | Many misconfigured systems leave root with no password |
| 5 | `ls` lists files, `cat` reads them | These two commands are used in almost every machine |
| 6 | `Ctrl+C` stops any running command | You will use this constantly in hacking |
| 7 | `ip a` shows your network interfaces | Use this to check your VPN IP address (tun0) |

---

## 📖 Extra Knowledge — Telnet vs SSH

| Feature | Telnet | SSH |
|---------|--------|-----|
| Year created | 1969 (very old!) | 1995 |
| Encryption | ❌ None — everything is plain text | ✅ Fully encrypted |
| Security | ❌ Very insecure | ✅ Secure |
| Port | 23 | 22 |
| Still used? | Rarely — only on old systems | Yes — the standard today |

> 💡 Think of Telnet like sending a postcard — anyone who handles it can read your message. SSH is like sending a letter in a sealed, locked envelope that only the receiver can open!

---

> **Meow completed! First machine done — keep going! 💪**

*📝 Written by: Chamith De Silva | Cybersecurity & Digital Forensics Student @ Kingston University, UK*
