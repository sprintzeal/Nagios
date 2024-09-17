Here's the updated installation guide for **Nagios Core 4.5.4** on **Ubuntu**, using `nano` as the text editor:

---

### **Nagios Core Installation and Configuration on Ubuntu (Version 4.5.4)**

#### **Overview**
This guide explains how to install and configure **Nagios Core 4.5.4** on Ubuntu (22.04, 20.04, or 18.04 LTS). Nagios Core is a powerful, open-source monitoring tool for network resources, services, and applications.

#### **Prerequisites**
- **Operating System**: Ubuntu 22.04 LTS | 20.04 LTS | 18.04 LTS
- **User Privileges**: Root or non-root with `sudo` privileges.

---

### **Step 1: Update System**
To ensure your system is up to date with the latest package information, run:

```bash
sudo apt update && sudo apt upgrade -y
```

---

### **Step 2: Install Required Packages**
Install the necessary packages for Nagios Core:

```bash
sudo apt install -y autoconf bc gawk dc build-essential gcc libc6 make wget unzip apache2 php libapache2-mod-php libgd-dev libmcrypt-dev make libssl-dev snmp libnet-snmp-perl gettext
```

---

### **Step 3: Download and Extract Nagios Core Version 4.5.4**

1. Create a directory for Nagios Core:

    ```bash
    mkdir nagioscore && cd nagioscore
    ```

2. Download **Nagios Core 4.5.4** from the official website:

    ```bash
    sudo wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.4.tar.gz
    ```

3. Extract the downloaded package:

    ```bash
    sudo tar -xvf nagios-4.5.4.tar.gz
    ```

---

### **Step 4: Compile and Install Nagios**

1. Navigate to the extracted directory:

    ```bash
    cd nagios-4.5.4
    ```

2. Compile Nagios and configure Apache:

    ```bash
    sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
    sudo make all
    ```

3. Create the Nagios user and group, and add the Apache `www-data` user to the `nagios` group:

    ```bash
    sudo make install-groups-users
    sudo usermod -a -G nagios www-data
    ```

4. Install Nagios binaries and set up service configurations:

    ```bash
    sudo make install
    sudo make install-daemoninit
    sudo make install-init
    sudo make install-commandmode
    sudo make install-config
    sudo make install-webconf
    ```

5. Enable Apache modules required for Nagios:

    ```bash
    sudo a2enmod rewrite cgi
    ```

---

### **Step 5: Create a Nagios Admin User**
Create a Nagios admin user to access the web interface:

```bash
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Follow the prompt to set the password for this user.

---

### **Step 6: Install and Configure Nagios Plugins and NRPE**

1. Install Nagios Plugins and NRPE:

    ```bash
    sudo apt install monitoring-plugins nagios-nrpe-plugin -y
    ```

2. Create a directory for server configurations:

    ```bash
    sudo mkdir /usr/local/nagios/etc/servers
    ```

3. Edit the main Nagios configuration file to include server configuration:

    ```bash
    sudo nano /usr/local/nagios/etc/nagios.cfg
    ```

    Uncomment the following line by removing the `#`:

    ```bash
    cfg_dir=/usr/local/nagios/etc/servers
    ```

    Save and close the file (`CTRL + O` to save, `Enter`, then `CTRL + X` to exit).

4. Set the path to the Nagios Plugins binaries in the resource configuration:

    ```bash
    sudo nano /usr/local/nagios/etc/resource.cfg
    ```

    Set the `$USER1$` variable:

    ```bash
    $USER1$=/usr/lib/nagios/plugins
    ```

5. Define the Nagios admin email in `contacts.cfg`:

    ```bash
    sudo nano /usr/local/nagios/etc/objects/contacts.cfg
    ```

    Update the `email` field:

    ```bash
    email your-email@example.com
    ```

6. Define the NRPE check command in `commands.cfg`:

    ```bash
    sudo nano /usr/local/nagios/etc/objects/commands.cfg
    ```

    Add the following block to the end of the file:

    ```bash
    define command{
        command_name check_nrpe
        command_line $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
    }
    ```

---

### **Step 7: Adjust Firewall Settings**
Configure the firewall to allow Nagios and Apache access:

```bash
sudo ufw enable
sudo ufw allow 80
sudo ufw allow 443
sudo ufw allow 22
sudo ufw reload
```

---

### **Step 8: Start Services and Access Nagios**

1. Restart Apache and Nagios services:

    ```bash
    sudo systemctl restart apache2
    sudo systemctl restart nagios
    ```

2. Enable both services to start on boot:

    ```bash
    sudo systemctl enable apache2
    sudo systemctl enable nagios
    ```

3. Check the status of Apache and Nagios:

    ```bash
    sudo systemctl status apache2
    sudo systemctl status nagios
    ```

4. Access the Nagios web interface:
   - Open a web browser and navigate to `http://your-server-ip/nagios`
   - Log in with `nagiosadmin` and the password you set earlier.

---

### **Conclusion**
You have successfully installed and configured **Nagios Core 4.5.4** on Ubuntu! You can now monitor your system and network resources effectively.

---



