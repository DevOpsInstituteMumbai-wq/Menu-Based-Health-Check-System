# System Health Check Script

## Overview
This is a **menu-based Bash script** that performs essential **system health checks** and sends a **comprehensive report via email** every four hours. It helps in monitoring disk usage, running services, memory usage, and CPU performance.

## Features
- **Check Disk Usage**: Displays available and used disk space.
- **Monitor Running Services**: Lists currently active services.
- **Assess Memory Usage**: Shows memory consumption.
- **Evaluate CPU Usage**: Monitors CPU performance.
- **Send Comprehensive Report via Email**: Gathers system data and sends a detailed report.
- **Automated Email Report Every Four Hours** (via `cron`).

## Prerequisites
Ensure your system meets the following requirements:
- **Linux (Ubuntu, CentOS, or any Unix-based system)**
- **Bash Shell**
- **mailx (for email functionality)**
- **sudo access**

To install `mailx`, run:
```sh
sudo apt install mailutils  # Ubuntu/Debian
# (select 1 when asked for configuration - No Configuration)
# You can always reconfigure later using:
sudo dpkg-reconfigure postfix

sudo yum install mailx      # CentOS/RHEL
```

## Installation
1. Clone this repository:
```sh
git clone https://github.com/DevOpsInstituteMumbai-wq/Menu-Based-Health-Check-System.git
cd Menu-Based-Health-Check-System
```

2. Make the script executable:
```sh
chmod +x menu.sh
```

3. Set up your email for receiving reports (modify `EMAIL` in the script):
```sh
EMAIL="your-email@example.com"
```

## Usage
Run the script using:
```sh
./menu.sh
```

A menu will appear:
```
=============================
 System Health Check Menu
=============================
1. Check Disk Usage
2. Monitor Running Services
3. Assess Memory Usage
4. Evaluate CPU Usage
5. Send Comprehensive Report
6. Exit
Enter your choice:
```
Select an option and follow the on-screen instructions.

## Automating the Report
To send the system health report every **four hours**, add a `cron` job:
```sh
crontab -e
```
Add the following line at the end:
```sh
0 */4 * * * /path/to/system_health.sh --email
```
Save and exit.

## Logging
- Logs are stored in `/var/log/sys_health.log`
- You can check logs using:
```sh
tail -f /var/log/sys_health.log
```

## To send via Gmail SMTP

Edit `/etc/postfix/main.cf` and update/add the following:
```sh
relayhost = [smtp.gmail.com]:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```

> `relayhost` tells Postfix to send all outgoing mail through Gmail.

### Create `/etc/postfix/sasl_passwd` with your Gmail credentials:
```
[smtp.gmail.com]:587 your-email@gmail.com:your-app-password
```

Replace `your-app-password` with an **App Password**, not your regular Gmail password.

If 2FA is enabled (which it should be), generate an App Password.

### Secure the password file:
```sh
sudo chmod 600 /etc/postfix/sasl_passwd
```

### Postmap and reload:
```sh
sudo postmap /etc/postfix/sasl_passwd
sudo systemctl restart postfix
```

### Test it:
Try sending an email using `mail`:
```sh
echo "Hello from Postfix" | mail -s "Test Email" someone@example.com
```

Then check the log:
```sh
tail -f /var/log/mail.log
```
Also helpful for debugging if mail is not received.

## To Create App Password on Google Account for Gmail

1. Go to your Google Account: https://myaccount.google.com/  
   Click on **"Security"** in the left-hand navigation panel.

2. Enable **2-Step Verification** (if not already):  
   Scroll down to **"How you sign in to Google"** → Click **"2-Step Verification"**

3. Generate an **App Password**:
   - After enabling 2FA, go to **"App passwords"**
   - Select the app (e.g., "Mail") and device (e.g., "Linux server")
   - Click **Generate**

### 📌 Copy the generated app password and store it securely.

**Important Notes**:
- App passwords are revoked when you change your Google Account password.
- If lost, you'll need to generate a new one.
- If you can't find the **"App passwords"** option, make sure **2-Step Verification** is enabled.

## Contributing
Feel free to fork this repository, enhance the script, and submit pull requests!

## License
This project is licensed under the MIT License.

## Author
DevOps Institute Mumbai
