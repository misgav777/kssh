# kssh – The Smart Way to SSH into AWS EC2 (via SSM)

`kssh` is a zero-dependency shell tool that lets you quickly start a secure SSM session into EC2 instances based on tag-based hostnames.

No keys. No bastions. Just AWS CLI + tags.

---

## Quick Install (macOS/Linux)

Paste this one-liner into your terminal:

```bash
curl -sL https://raw.githubusercontent.com/misgav777/kssh/main/kssh | sudo tee /usr/local/bin/kssh > /dev/null && sudo chmod +x /usr/local/bin/kssh

Usage
kssh <hostname>
