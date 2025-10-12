````md
# 🧱 1️⃣ SYSTEM REQUIREMENTS

Before installing anything, make sure your system meets the following requirements:

---

### ✅ **Requirement: At least 4 GB RAM**  
**Your system:** You have **3.8 GiB** — ✅ **Fine**

```bash
(kali㉿kali)-[~]
└─$ free -h
               total        used        free      shared  buff/cache   available
Mem:           **3.8Gi**     2.1Gi       486Mi        52Mi       1.5Gi       1.7Gi  # ✅ >= 4 GiB total (rounded)
Swap:          4.0Gi       268Ki       4.0Gi
````

---

### ✅ **Requirement: At least 10 GB free disk space**

**Your system:** You have **9.6 GB available** — ⚠️ **Just under 10 GB**

```bash
(kali㉿kali)-[~]
└─$ df -h                                   
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           392M  1.4M  390M   1% /run
/dev/sda1        79G   65G   **9.6G**  88% /       # ⚠️ Close to limit (requirement is 10 GB free)
tmpfs           2.0G   19M  1.9G   1% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
tmpfs           2.0G   12K  2.0G   1% /tmp
tmpfs           1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
tmpfs           392M  128K  392M   1% /run/user/1000
```

---

### ✅ **Requirement: Internet connection**

**Your system:** Ping is successful — ✅ **Connected**

```bash
(kali㉿kali)-[~]
└─$ ping google.com                                   
PING google.com (142.251.221.238) 56(84) bytes of data.
64 bytes from pnbomb-bk-in-f14.1e100.net (142.251.221.238): icmp_seq=1 ttl=128 time=25.5 ms
64 bytes from pnbomb-bk-in-f14.1e100.net (142.251.221.238): icmp_seq=2 ttl=128 time=26.2 ms
64 bytes from pnbomb-bk-in-f14.1e100.net (142.251.221.238): icmp_seq=3 ttl=128 time=28.4 ms
64 bytes from pnbomb-bk-in-f14.1e100.net (142.251.221.238): icmp_seq=4 ttl=128 time=26.4 ms
64 bytes from pnbomb-bk-in-f14.1e100.net (142.251.221.238): icmp_seq=5 ttl=128 time=28.8 ms
^C
--- google.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4008ms  # ✅ No packet loss
rtt min/avg/max/mdev = 25.490/27.075/28.835/1.304 ms
```

---

### ✅ **Requirement: Admin privileges (sudo access)**

**Your system:** You have **sudo/root access** — ✅ **Confirmed**

```bash
kali㉿kali)-[~]
└─$ sudo whoami                                                                                                 
[sudo] password for kali: 
**root**   # ✅ Confirmed sudo access
```
---
## ⚙️ 2️⃣ INSTALL A LOCAL WEB SERVER STACK (LAMP)


## 🧱 What is **LAMP**?

**LAMP** stands for:

* **L**inux → Operating System
* **A**pache → Web Server
* **M**ariaDB (or MySQL) → Database Server
* **P**HP → Server-side Scripting Language

This stack lets you run dynamic websites like **WordPress** entirely on your local machine.
WordPress runs on:

* **L**inux
* **A**pache (web server)
* **M**ariaDB / MySQL (database)
* **P**HP (server-side language)
---

## ⚙️ Installation Command

```bash
sudo apt update
sudo apt install apache2 mariadb-server php libapache2-mod-php php-mysql php-xml php-gd php-curl php-mbstring unzip wget -y
```

---

## 🧩 Breakdown of Packages

| Package                | Purpose                      | Why Needed                                         | Verify Installation             |                |
| ---------------------- | ---------------------------- | -------------------------------------------------- | ------------------------------- | -------------- |
| **apache2**            | Web server software          | Serves WordPress pages to browsers                 | `apache2 -v` → shows version    |                |
| **mariadb-server**     | Database engine              | Stores WordPress data (posts, users, settings)     | `sudo systemctl status mariadb` |                |
| **php**                | Server-side language         | Runs WordPress backend logic                       | `php -v`                        |                |
| **libapache2-mod-php** | Apache PHP connector         | Lets Apache execute PHP code                       | Check PHP test page below       |                |
| **php-mysql**          | PHP module for MySQL/MariaDB | Allows WordPress to talk to the database           | Included in `php -m` output     |                |
| **php-xml**            | XML handling                 | Needed for themes, plugins, and API responses      | `php -m                         | grep xml`      |
| **php-gd**             | Image processing             | Handles image thumbnails and media uploads         | `php -m                         | grep gd`       |
| **php-curl**           | URL requests                 | Needed for updates, API calls (e.g. REST, plugins) | `php -m                         | grep curl`     |
| **php-mbstring**       | Multibyte string support     | For UTF-8 text, multilingual content               | `php -m                         | grep mbstring` |
| **wget**               | File downloader              | To fetch WordPress from internet                   | `wget --version`                |                |
| **unzip**              | Archive extractor            | To extract WordPress zip file                      | `unzip -v`                      |                |

---

## ✅ Verification Steps (After Installation)

### 🔹 Verify Apache

```bash
sudo systemctl status apache2
```

or open in browser:

```
http://localhost
```

✅ You should see: “Apache2 Debian Default Page”.

---

### 🔹Verify PHP

Create a test file:

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

Then open in browser:

```
http://localhost/info.php
```

✅ You should see PHP information (version, modules, etc.)

---

### 🔹Verify MariaDB

Start and secure it:

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

Then test:

```bash
sudo mysql -u root
```

✅ If you get the `MariaDB>` prompt, it’s running.

Exit with:

```sql
exit;
```

---

## 🗄️ 4️⃣ SECURE DATABASE (MariaDB)

If `mysql_secure_installation` exists:

```bash
sudo mysql_secure_installation
```

Otherwise, run manually:

```bash
sudo mysql -u root
```

Then inside:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword';
FLUSH PRIVILEGES;
EXIT;
```
At the MariaDB prompt (`MariaDB [(none)]>`), run **each line separately**:


### 🧠 Explanation

| Command                                               | Purpose                                                       |
| ----------------------------------------------------- | ------------------------------------------------------------- |
| `ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword';` | Changes your root user password to **newpassword**                   |
| `FLUSH PRIVILEGES;`                                   | Refreshes user permissions so changes take effect immediately |
| `EXIT;`                                               | Exits the MariaDB console and returns to your terminal        |

Once you’ve done this, test your password:

```bash
sudo mysql -u root -p
```

Then enter `yourpassword` when prompted —
✅ you should log in successfully.

---
## 🗃️ Step 5️⃣ — **Create Database for WordPress**

### 🧩 First Command — Login to MariaDB

```bash
sudo mysql -u root -p
```

* `sudo` → runs command as admin.
* `mysql` → starts the MariaDB command-line client.
* `-u root` → logs in as the **root user** (database administrator).
* `-p` → asks for your root password (the one you set in step 4).

Once you log in, you’ll see:

```
MariaDB [(none)]>
```

That means you’re now inside the MariaDB shell — ready to run SQL commands.

---

## 🧠 Inside MariaDB Shell

Now you’ll create:

1. A new **database** for WordPress
2. A new **user** just for WordPress
3. Give that user **permissions** to manage only its own database

---

### 🔹 1️⃣ Create a Database

```sql
CREATE DATABASE wordpressdb;
```

This command creates a new, empty **database** named `wordpressdb`.

👉 Think of a database like a folder — WordPress will put all your tables (data files) inside it:

* wp_posts (blog posts)
* wp_users (accounts)
* wp_options (settings)
* etc.

You can name it anything you like, for example:

```sql
CREATE DATABASE newsdb;
```

---

### 🔹 2️⃣ Create a User

```sql
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password123';
```

* `CREATE USER` → makes a new database user.
* `'wordpressuser'@'localhost'` →

  * username = `wordpressuser`
  * `localhost` means this user can only log in **from your own computer** (not remotely).
* `IDENTIFIED BY 'password123'` → sets the password for this user.

🧠 **Why create a new user?**
Because it’s unsafe for WordPress to log in as `root`.
If your site gets hacked, attackers shouldn’t get full control of MariaDB.
So we give WordPress its own limited user.

---

### 🔹 3️⃣ Grant Permissions
```sql
GRANT ALL PRIVILEGES ON wordpressdb.* TO 'wordpressuser'@'localhost';
```

🧩 Explanation:

* `GRANT ALL PRIVILEGES` → give full control (create, read, update, delete).
* `ON wordpressdb.*` → only within that **one database** (`wordpressdb`).
* `TO 'wordpressuser'@'localhost'` → only this specific user gets access.

✅ This ensures that:

* `wordpressuser` can manage everything in `wordpressdb`.
* But cannot access or modify other databases.

---

### 🔹 4️⃣ Apply Changes

```sql
FLUSH PRIVILEGES;
```

This command **reloads** MariaDB’s user table —
so your new user and permissions take effect immediately.

---

### 🔹 5️⃣ Exit MariaDB

```sql
EXIT;
```

Returns you to the Linux terminal (`kali@kali:~$`).

---

## ✅ What You Now Have

| Component         | Value           | Description                       |
| ----------------- | --------------- | --------------------------------- |
| **Database Name** | `wordpressdb`   | Stores all WordPress data         |
| **Database User** | `wordpressuser` | Account WordPress uses to connect |
| **Password**      | `password123`   | The user’s password               |
| **Host**          | `localhost`     | Database runs on the same machine |

You’ll need to enter these values when WordPress asks for **database details** during installation.

---

### 💡 Verify Everything Works

You can test login with your new user:

```bash
mysql -u wordpressuser -p
```

Then type:

```sql
SHOW DATABASES;
```

You should see:

```
+----------------+
| Database       |
+----------------+
| wordpressdb    |
+----------------+
```

✅ If you see that, your WordPress database is correctly set up.

---

## 🌐 6️⃣ DOWNLOAD & CONFIGURE WORDPRESS
### 🧭 Step 1 — Go to the Temporary Folder

```bash
cd /tmp
```

✅ **What it does:**
Moves you into the `/tmp` directory — a temporary folder in Linux used for short-term files.

🧠 **Why here?**

* It’s a safe place to download and unpack files.
* After installation, we’ll move WordPress to the proper location.

Think of `/tmp` as your *“downloads”* area before organizing the files.

---

### 🌍 Step 2 — Download WordPress
#### 🌍 go to `https://wordpress.org/latest.zip`
This is **the official download link** provided by the **WordPress.org** website — the main, open-source home of WordPress (not wordpress.com).

---

### 🔹 Official WordPress Download Page

If you visit:

👉 **[https://wordpress.org/download/](https://wordpress.org/download/)**

you’ll see a big blue **“Download WordPress”** button.

That button links directly to this file:

```
https://wordpress.org/latest.zip
```

It’s always updated to the **latest stable release** of WordPress.

---

### 🔹 What’s Inside the File?

The ZIP file (`latest.zip`) contains the full WordPress source code:

```
wordpress/
 ├── index.php
 ├── wp-config-sample.php
 ├── wp-content/
 ├── wp-admin/
 └── wp-includes/
```

✅ It’s safe and verified by the WordPress.org team.
✅ Every release is digitally signed and versioned.

---
### 🔹 How to Verify It’s Official

If you want to **double-check authenticity**:

1. Visit [https://wordpress.org/download/releases/](https://wordpress.org/download/releases/)
2. Each version (e.g. `6.6.2.zip`) has:

   * SHA-256 checksum
   * Release date
   * Version number

You can even verify on your system:

```bash
wget https://wordpress.org/latest.zip
sha256sum latest.zip
```

Then compare the hash with the one shown on the release page.

---
```bash
wget https://wordpress.org/latest.zip
```

✅ **What it does:**
Uses the `wget` command to download the latest version of WordPress from the official site.

* `wget` = a tool for downloading files from the web.
* `https://wordpress.org/latest.zip` = direct link to the newest WordPress package.

After this command, you’ll have a file called `latest.zip` inside `/tmp`.
---

### 📦 Step 3 — Extract the Zip File

```bash
unzip latest.zip
```

✅ **What it does:**
Unpacks the downloaded `latest.zip` file.

After running it, you’ll get a folder called **`wordpress/`** that contains:

* `index.php`
* `wp-config-sample.php`
* `wp-content/` (themes, plugins, uploads)
* `wp-admin/` and `wp-includes/` (core files)

🧠 **Think of it like:**
You just opened a downloaded ZIP file on Windows — same thing here, but via terminal.

---

### 📁 Step 4 — Move WordPress to Web Root

```bash
sudo mv wordpress /var/www/html/new
```

✅ **What it does:**
Moves (renames) the extracted WordPress folder into your web-server directory.

* `/var/www/html/` → this is where Apache serves websites from.
* `new` → is your **site folder name** (you can choose another name, e.g., `myblog`).

So your WordPress files will now live at:

```
/var/www/html/new/
```

🧠 **Example:**
If you open your browser and go to
👉 `http://localhost/new`
you’ll see the WordPress setup screen later.

---

### 👥 Step 5 — Give Apache Ownership

```bash
sudo chown -R www-data:www-data /var/www/html/new
```

✅ **What it does:**
Changes the “owner” of all files inside `/var/www/html/new` to the `www-data` user and group.

* `www-data` = the user account Apache runs under.
* `-R` = apply recursively (to all files and sub-folders).

🧠 **Why important?**
Apache (your web server) must have permission to read and write files (e.g., upload images, install plugins).
If you skip this, WordPress might show “permission denied” errors later.

---

### 🔐 Step 6 — Set Correct Permissions

```bash
sudo chmod -R 755 /var/www/html/new
```

✅ **What it does:**
Sets file permissions for security and access.

* `7` → full control (read, write, execute) for the **owner**.
* `5` → read and execute (no write) for **group** and **others**.
* `-R` → recursive, applies to everything inside.

🧠 **In simple terms:**

* Apache can read and write its files.
* Other users on the system can only read them, not modify.

---
## 💡 After This Step

Your WordPress files are ready.
Next, you’ll need to **configure WordPress** by creating the config file (`wp-config.php`) and linking it to your database (from Step 5).

---

## 🧩 7️⃣ CONFIGURE APACHE VIRTUAL HOST (optional but clean)

```bash
sudo nano /etc/apache2/sites-available/new.conf
```

Paste:

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/new
    ServerName new.local
    <Directory /var/www/html/new/>
        AllowOverride All
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/new_error.log
    CustomLog ${APACHE_LOG_DIR}/new_access.log combined
</VirtualHost>
```

Enable and restart:

```bash
sudo a2ensite new.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

Edit `/etc/hosts`:

```bash
sudo nano /etc/hosts
```

Add:

```
127.0.0.1   news.local
```

---

## 🖥️ 8️⃣ OPEN WORDPRESS IN BROWSER

Go to:

```
http://new.local
```

(Or `http://localhost/new` if you skipped the virtual host)
Perfect 👏 you’re now at the **WordPress database connection setup** stage — this is the heart of your website because WordPress stores *all posts, pages, users, and settings* inside the database you created earlier.

Let’s connect your **WordPress files** to your **MariaDB database** step-by-step 👇

---

## 🧩 Step 8 — Configure WordPress to Use Your Database
### 🔹 Step 1: Go to Your WordPress Folder

```bash
cd /var/www/html/new
```

---

### 🔹 Step 2: Copy the Sample Config File

WordPress provides a file called `wp-config-sample.php`.
We’ll make a working copy of it:

```bash
sudo cp wp-config-sample.php wp-config.php
```

This creates a real configuration file that WordPress will read at startup.

---

### 🔹 Step 3: Edit the Configuration

Open it with nano:

```bash
sudo nano wp-config.php
```

Now, scroll to this section (you’ll see something like this):

```php
/** The name of the database for WordPress */
define( 'DB_NAME', 'database_name_here' );

/** MySQL database username */
define( 'DB_USER', 'username_here' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password_here' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );
```

Replace those with **your actual database details** (from the database you made earlier):

```php
define( 'DB_NAME', 'wordpressdb' );
define( 'DB_USER', 'wordpressuser' );
define( 'DB_PASSWORD', 'password123' );
define( 'DB_HOST', 'localhost' );
```

---

### 🔹 Step 4: Save and Exit

Press:

```
Ctrl + O → Enter → Ctrl + X
```

---

### 🔹 Step 5: Set Correct Permissions

Just to be safe, make sure WordPress files belong to Apache’s web user:

```bash
sudo chown -R www-data:www-data /var/www/html/news
sudo chmod -R 755 /var/www/html/news
```

This ensures WordPress can upload media, install plugins, and write settings.

---

### 🔹 Step 6: Run the Installation in Browser

Now open your web browser and go to:

👉 **[http://localhost/news](http://localhost/news)**

You’ll see the famous **WordPress installation screen** (“Welcome to WordPress!”).
Fill it in like this:

| Field                    | Example                                   |
| ------------------------ | ----------------------------------------- |
| Site Title               | New Channel                              |
| Username                 | admin                                     |
| Password                 | (choose your password)                    |
| Email                    | [you@example.com](mailto:you@example.com) |
| Search Engine Visibility | leave unchecked (for local setup)         |

Then click **Install WordPress** ✅

---

### 🔹 Step 7: Log In

Once done, visit:
👉 **[http://localhost/news/wp-admin](http://localhost/news/wp-admin)**

Use your admin username/password — you’re now inside your **WordPress Dashboard**!

---

