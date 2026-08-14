# Name-Based Virtual Hosting (Apache HTTPD)

Is repository me **Apache HTTP Server (httpd)** ka use karke **name-based virtual hosting** setup kiya gaya hai — yaani ek hi server aur ek hi IP address par, alag-alag domain names ke through 3 alag-alag websites serve ki ja rahi hain.

## 📌 Project Overview

Ek AWS EC2 instance (Amazon Linux, `httpd` service) par teen virtual hosts configure kiye gaye hain:

| Domain | Purpose | Document Root |
|---|---|---|
| `payfoundry.online` | Production site | `/var/www/payfoundry` |
| `dev.payfoundry.online` | Development environment | `/var/www/dev-payfoundry` |
| `photo.payfoundry.online` | Photo/dev server | `/var/www/photo-payfoundry` |

Sabhi domains ek hi server pe host ho rahe hain — Apache `ServerName` directive ke basis par request ko sahi document root pe route karta hai (isi liye ise **"name-based" virtual hosting** kehte hain).

## 📂 Repository Structure

```
name-based-virtual-hosting/
├── config/
│   ├── payfoundry.conf          # Virtual host config: payfoundry.online
│   ├── dev.payfoundry.conf      # Virtual host config: dev.payfoundry.online
│   └── photo.payfoundry.conf    # Virtual host config: photo.payfoundry.online
├── payfoundry/
│   └── index.html                # Production site page
├── dev-payfoundry/
│   └── index.html                # Dev environment page
├── photo-payfoundry/
│   └── index.html                # Photo server page
├── screenshots/                  # AWS console, DNS records & DNS checker screenshots
├── setup-command.txt             # Terminal commands used during setup
├── setup-command.pdf             # Same commands (PDF format)
└── setup-commandss.rtf           # Same commands (RTF format)
```

## ⚙️ Virtual Host Configuration

Har site ke liye `/etc/httpd/conf.d/` me ek alag `.conf` file banayi gayi hai:

**`payfoundry.conf`**
```apache
<VirtualHost *:80>
    ServerName payfoundry.online
    DocumentRoot /var/www/payfoundry
</VirtualHost>
```

**`dev.payfoundry.conf`**
```apache
<VirtualHost *:80>
    ServerName dev.payfoundry.online
    DocumentRoot /var/www/dev-payfoundry
</VirtualHost>
```

**`photo.payfoundry.conf`**
```apache
<VirtualHost *:80>
    ServerName photo.payfoundry.online
    DocumentRoot /var/www/photo-payfoundry
</VirtualHost>
```

## 🚀 Setup Steps (jaisa `setup-command.txt` me follow kiya gaya)

1. Har website ke liye document root directories banayi gayi:
   ```bash
   mkdir /var/www/payfoundry
   mkdir /var/www/dev-payfoundry
   mkdir /var/www/photo-payfoundry
   ```
2. Har directory me ek `index.html` page daala gaya.
3. `/etc/httpd/conf.d/` me teeno domains ke liye virtual host `.conf` files banayi/edit ki gayin.
4. Configuration syntax check kiya gaya:
   ```bash
   httpd -t
   ```
5. Apache service restart/reload ki gayi:
   ```bash
   systemctl restart httpd
   # ya
   systemctl reload httpd
   ```
6. Domain names ke DNS records (A records) EC2 instance ke public IP par point kiye gaye (screenshots folder me DNS console aur DNS-checker results maujood hain).

## 🖥️ Requirements

- Linux server (Amazon Linux / RHEL / CentOS family — commands `httpd` aur `systemctl` use karte hain)
- Apache HTTP Server (`httpd`)
- Root/sudo access
- Domain names DNS-configured, server ke public IP ki taraf pointing

## 📸 Screenshots

`screenshots/` folder me deployment verification ke proof hain:
- AWS EC2 console
- Har domain ke DNS records
- DNS-checker results (propagation verify karne ke liye)
- Live browser output har teeno domains ka

## 📝 Notes

- Yeh setup guide/demo purpose ke liye hai — production use se pehle HTTPS (SSL/TLS via Let's Encrypt/Certbot) aur firewall/security group rules zaroor add karein.
- `setup-command.txt` ek raw terminal session log hai jisme actual commands aur unke outputs dono maujood hain — reference/debugging ke liye useful hai.

## 👤 Author

Repository: [alokbhateshwar/name-based-virtual-hosting](https://github.com/alokbhateshwar/name-based-virtual-hosting)
