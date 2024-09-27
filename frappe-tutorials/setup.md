A detailed step-by-step tutorial for installing Frappe via Bench on Debian 12, written in Markdown.

```markdown
# Frappe Installation on Debian 12 via Bench

This tutorial will guide you through the steps to install Frappe using Bench on Debian 12. Frappe is a powerful framework to build web applications, including ERPNext, which is built on top of Frappe.

## Prerequisites

Before installing Frappe, ensure your Debian 12 environment meets the following prerequisites:

- **Debian 12 installed** on your system.
- **Sudo privileges**.
- **At least 2GB of RAM** (4GB or more is recommended for running smoothly).
- **PostgreSQL 13 or MariaDB**.

### Step 1: Update and Upgrade System Packages

Update your system’s package list to ensure you are working with the latest versions.

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Required Packages

Frappe requires several dependencies. Install them using the following command:

```bash
sudo apt install -y python3-dev python3-pip python3-setuptools \
build-essential redis-server supervisor nginx \
git wkhtmltopdf xvfb libssl-dev libffi-dev libjpeg-dev liblcms2-dev libblas-dev \
liblapack-dev libatlas-base-dev gfortran libmysqlclient-dev libmariadbclient-dev curl
```

### Step 3: Install Node.js and npm

Frappe requires Node.js (v16 or v18) and npm. Let's install the Node.js version 18:

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Verify the installation:

```bash
node -v
npm -v
```

### Step 4: Install Yarn Package Manager

Install Yarn, which is required to manage JavaScript dependencies for Frappe:

```bash
sudo npm install -g yarn
```

### Step 5: Install MariaDB

Frappe uses MariaDB as the database engine. Install MariaDB by following these steps:

```bash
sudo apt install mariadb-server
```

Secure your MariaDB installation:

```bash
sudo mysql_secure_installation
```

During the security installation, you’ll be prompted for the root password and asked to set other security configurations like removing anonymous users, disabling remote root login, etc.

Next, configure MariaDB for Frappe:

```bash
sudo nano /etc/mysql/my.cnf
```

Add the following configuration at the end of the file (replace `[innodb-file-size]` with your desired size):

```ini
[mysqld]
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

Restart MariaDB:

```bash
sudo systemctl restart mariadb
```

### Step 6: Install Redis

Redis is used by Frappe for caching. You can install Redis by running:

```bash
sudo apt install redis-server
```

Enable Redis to start on boot:

```bash
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

### Step 7: Install Bench

Bench is a command-line tool that helps in installing and managing Frappe applications.

First, install Bench using the following command:

```bash
sudo pip3 install frappe-bench
```

Now, verify the Bench installation:

```bash
bench --version
```

### Step 8: Initialize Frappe Bench

Create a directory for your Bench instance and initialize the Frappe project:

```bash
mkdir ~/frappe-bench
cd ~/frappe-bench
bench init --frappe-branch version-14 frappe-bench
```

Here, we're using Frappe version 14 (the latest stable version at the time). You can specify the version you need by replacing `version-14`.

### Step 9: Create a New Frappe Site

Once the bench is initialized, create a new Frappe site. Replace `mysite.local` with your desired site name.

```bash
cd ~/frappe-bench
bench new-site mysite.local
```

You’ll be prompted to enter the MySQL/MariaDB root password and a new password for the Frappe Administrator account.

### Step 10: Install ERPNext (Optional)

If you're planning to use ERPNext with Frappe, you can install it using the following command:

```bash
bench get-app --branch version-14 erpnext
bench --site mysite.local install-app erpnext
```

### Step 11: Start Frappe Development Server

You can start the Frappe development server using Bench:

```bash
bench start
```

This will start the server on `http://localhost:8000`. Open the URL in your browser to access your Frappe site.

### Step 12: Setup Production (Optional)

To set up Frappe for production use, you will need to configure it with Nginx and Supervisor. Bench provides an easy way to do that:

```bash
sudo bench setup production [frappe-user]
```

Replace `[frappe-user]` with your Linux username.

This will set up Frappe to run in the background using Supervisor and Nginx.

### Step 13: Access Frappe Site

Once the server is running, access your site by visiting `http://localhost:8000` or your server's IP address in your browser. You can log in using the Administrator credentials you set earlier.

---

## Conclusion

You have successfully installed Frappe on Debian 12! You can now start building applications or install ERPNext for a full ERP solution. If you run into any issues or need to update your setup, refer to the Frappe documentation or community for more assistance.
```

This tutorial provides a complete guide from setting up dependencies to running a Frappe instance locally on Debian 12. 
