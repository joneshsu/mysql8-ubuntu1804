## Install MySQL 8 on ubuntu 18.04 using Vagrant

### Step 1: Setup a virtual machine

We use `Vargrant` as our local virtual machine. We need choose ubuntu version and config vm
information in `Vagrantfile`. In this practice chooses 18.04 of ubuntu, and below is Vagrantfile configuration.

```
Vagrant.configure("2") do |config|
  # Choose 18.04 of ubuntu 
  # Vagrant will try to install from local boxes or remote repository.
  config.vm.box = "bento/ubuntu-18.04"

  # For connecting MySQL, binding local port 3306 and virtual machine port 3306   
  config.vm.network "forwarded_port", guest: 3306, host: 3306
  
  # Expose a private ip for MySQL hostname
  config.vm.network "private_network", ip: "192.168.168.168"
  
  config.vm.provider "virtualbox" do |vb|
    # Naming for virtualbox image
    vb.name = "mysql8"
    # Allocate 2048 GB memory for this vm
    vb.memory = "2048"
  end
end
```

Let's start a virtual machine after finished above configuration. Move on to your `Vagrantfile` directory.

```
$ vagrant up
```

Enter the virtual machine after vagrant configures all settings.

```
$ vagrant ssh
```

### Step 2: Install MySQL on virtual machine

The MySQL developers maintains the APT repository which provides a simple and convenient way to install
and update MySQL products with the latest software packages using `apt` or `apt-get`.

Now go to the `tmp` folder and download the file.

```
$ cd /tmp
```

```
$ sudo curl -OL https://dev.mysql.com/get/mysql-apt-config_0.8.13-1_all.deb
```

Now a `.deb` software package will get downloaded in the tmp folder.

Use the `dpkg` command to install the package.
```
$ sudo dpkg -i mysql-apt-config*
```

You will prompted to choose the version, you can move to OK menu and hit `Enter`.

```
$ sudo apt update
```

Now the MySQL repository is added and you can delete the downloaded file.

```
$ sudo rm mysql-apt-config*
```

Install MySQL 8.0

```
$ sudo apt install mysql-server
```

You can provide a secure password for your root. Next you need to configure the authentication plugin.
Use the ****Use Strong Password Encryption**** and hit Enter.

Wait for the installation to complete.

Once the installation is complete you can view the status of the MySQL server using the following command.

```
$ sudo systemctl status mysql
```

```
  mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2019-08-26 06:24:47 UTC; 1 day 23h ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 14420 ExecStartPre=/usr/share/mysql-8.0/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
 Main PID: 14470 (mysqld)
   Status: "Server is operational"
    Tasks: 43 (limit: 2344)
   CGroup: /system.slice/mysql.service
           └─14470 /usr/sbin/mysqld
```

****Securing MySQL****

Once the installation is completed it is necessary to secure the MySQL installation.

```
$ mysql_secure_installation
```

 * Enable the ****validate password plugin****
 * Update the ****root**** password if you need
 * Remove anonymous MySQL user
 * Remove test database
 * Reload privileges
 
****Add New User To MySQL****

e.g. Create a new user for localhost, newuser and 12345678

```
mysql> CREATE USER 'newuser'@'localhost' IDENTIFIED BY '12345678';
```

Assign all database permission for newuser

```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
```

Flush database

```
mysql> FLUSH PRIVILEGES;
```

e.g. Create a new user for remote, newuser and 12345678

```
mysql> CREATE USER 'newuser'@'%' IDENTIFIED BY '12345678';
```

Assign all database permission for newuser

```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'%';
```

Flush database

```
mysql> FLUSH PRIVILEGES;
```
