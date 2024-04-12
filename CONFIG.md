# 1. Install and configure MongoDB in local machine.
Procedure:
**Step 01: Platform Support**
```bash
cat /etc/lsb-release
```
![image](https://github.com/KKBUGHUNTER/Getting-start-ReactJs-NodeJs-MongoDB/assets/91019132/4f845203-4ab5-40df-bd31-9e25b10f83a2)

**Step 02: Import the public key used by the package management system**
```bash
sudo apt-get install gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
**Step 03: Create a list file for MongoDB**
for 22.04 LTS ("Jammy") > 
```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```
for 20.04 LTS ("Focal") 
```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```
**Step 04: Reload local package database**
```bash
sudo apt-get update
```
**Step 05: Install the MongoDB packages**
```bash
sudo apt-get install -y mongodb-org
```
```bash
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```
**Step 06: Select the appropriate tab below based on the result:**
```bash
ps --no-headers -o comm 1
```
Then select the appropriate tab below based on the result:

![image](https://github.com/KKBUGHUNTER/Getting-start-ReactJs-NodeJs-MongoDB/assets/91019132/93e77ea8-352e-4ee4-b0e5-f1b064223c38)

- systemd - select the systemd (systemctl) tab below.
```bash
sudo systemctl start mongod
sudo systemctl daemon-reload
sudo systemctl enable mongod
mongosh
```
- init - select the System V Init (service) tab below.
```bash
sudo service mongod start
sudo service mongod status
mongosh
```

![image](https://github.com/KKBUGHUNTER/Getting-start-ReactJs-NodeJs-MongoDB/assets/91019132/64b55a1c-e688-4fca-b8ef-eadf1346ef04)

**Step 07: Download and install MongoDB Compass**
Download - https://www.mongodb.com/try/download/shell
```bash
sudo dpkg -i <filename.dpkg>
```

***
# Thank you....
