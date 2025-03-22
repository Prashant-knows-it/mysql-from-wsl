### **MySQL Remote Access in WSL**  

## **Step 1: Edit MySQL Configuration**  
You need to edit MySQL’s config file, which requires **Notepad as Administrator**.  

1. Open **Command Prompt as Administrator**.  
2. Run:  
   ```sh
   notepad C:\ProgramData\MySQL\MySQL Server 8.0\my.ini
   ```
3. Find and edit/add the following in `[mysqld]` section: By default it will be either blank or `bind-address = 0.0.0.0` just below [mysqld] tag
   ```ini
   [mysqld]
   bind-address = 0.0.0.0
   ```
4. Save and close **Notepad**.  

## **Step 2: Restart MySQL**  
Restart MySQL for changes to take effect: CMD as adminstrator  
```sh
net stop mysql && net start mysql
```

## **Step 3: Create a Remote User**  
Log into MySQL and run:  
```sql
CREATE USER 'wsl_user'@'%' IDENTIFIED WITH mysql_native_password BY 'yourpassword'; 
GRANT ALL PRIVILEGES ON *.* TO 'wsl_user'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
Make sure to give your password. This is new and seperate password for user 'wsl_user'

## **Step 4: Install MySQL Client & Quick Connect Script in WSL**  
Run these commands in WSL:  
```sh
sudo apt install mysql-client-core-8.0
// run this inside ubuntu or any wsl, it will take 40 mb
echo "mysql -h \$(grep nameserver /etc/resolv.conf | awk '{print \$2}') -u wsl_user -p" > ~/mysqlwsl
// run this inside ubuntu or wsl 'mysqlwsl' is the file name, you can change this
chmod +x ~/mysqlwsl
// this makes the normal mysqlwsl file as executable file
sudo mv ~/mysqlwsl /usr/local/bin/
// this moves the file from home page to somewhere better, so that you can open mysql from anywhere
mysqlwsl
type `mysqlwsl` to open mysql in ubuntu
```
Now you are connected to mysql which is downloaded in window localhost from your wsl
t.me/hellothor if any error occured
