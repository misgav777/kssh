# ⚡️ kssh – The Smart Way to SSH into AWS EC2 (via SSM)

`kssh` is a zero-dependency shell tool that lets you quickly start a secure SSM session into EC2 instances based on tag-based hostnames.

🔐 No SSH keys.  
🛡 No bastion hosts.  
🧰 Just AWS CLI + tag-based lookups.

---
## 🚀 Quick Install (macOS/Linux)

### Create dir & Install kssh
```bash
sudo mkdir -p /usr/local/bin \
  && curl -sL https://raw.githubusercontent.com/misgav777/kssh/main/kssh \
     | sudo tee /usr/local/bin/kssh > /dev/null \
  && sudo chmod +x /usr/local/bin/kssh
```

## ✅ Requires

> - AWS CLI installed (`brew install awscli`)
> - IOKTA CLI ToolKit (`https://stackoverflowteams.com/c/kenshoo/articles/1974`)

---

## ⚙️ Usage

```bash
kssh <hostname>
```

---

## 🔍 Examples

```bash
kssh ecliwb2913     # Matches tag Fe_Name=ecliwb2913
kssh eclidb2913     # Matches tag Be_Name=eclidb2913
kssh ftp-prod-1     # Matches tag Name=ftp-prod-1*
```

---

## 🧠 What It Does

1. 🔎 Detects the correct tag key based on the hostname prefix  
2. 🧼 Queries EC2 with `aws ec2 describe-instances`  
3. 🧾 Extracts the instance ID from the result  
4. 🚀 Initiates the SSM session using:

```bash
aws ssm start-session --target <instance-id>
```

---

## ✅ Supported Hostname Patterns

| Prefix     | Tag Key Used |
|------------|--------------|
| `ecliwb*`  | `Fe_Name`    |
| `eclidb*`  | `Be_Name`    |
| `ftp*`     | `Name` (wildcard `*` applied) |

If a prefix doesn't match one of the expected formats, `kssh` will throw an error and exit.

---

## 🔐 IAM & EC2 Requirements

- The EC2 instance must:
  - ✅ Be in `running` state  
  - ✅ Have the SSM agent installed and active  
  - ✅ Have an IAM role attached that allows SSM access

- The IAM user or role you're using must have:
  - `ssm:StartSession`
  - `ec2:DescribeInstances`

---

## Troubleshooting

- **`No such directory /usr/local/bin`**  
  Run `sudo mkdir -p /usr/local/bin` or install to `$HOME/.local/bin` by exporting `KSSH_INSTALL_DIR`.
  
---

## 🧼 Uninstall

To remove `kssh` from your system:

```bash
sudo rm /usr/local/bin/kssh
```

---

## 🏷 Maintained by

This tool is built and maintained by **Misgav Ngaithe**  
Used internally to simplify EC2 access workflows through AWS Systems Manager.


