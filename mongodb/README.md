# MongoDB 5.0 Installation and Replica Set Setup on macOS

This guide walks you through installing MongoDB 5.0 using Homebrew and configuring it with a replica set for local development. This setup is required for applications like Rocket.Chat or any system using `$changeStream`.

---

## ✅ Step-by-Step Guide

### 1. Tap MongoDB Formula

```bash
brew tap mongodb/brew
```

### 2. Install MongoDB 5.0 Community Edition

```bash
brew install mongodb-community@5.0
```

### 3. Add MongoDB to Your PATH

```bash
echo 'export PATH="/opt/homebrew/opt/mongodb-community@5.0/bin:$PATH"' >> ~/.zprofile
source ~/.zprofile
```

Then verify:

```bash
which mongod
# Expected: /opt/homebrew/opt/mongodb-community@5.0/bin/mongod
```

### 4. Create MongoDB Data Directory

```bash
sudo mkdir -p /usr/local/var/mongodb
sudo chown -R $(whoami) /usr/local/var/mongodb
```

### 5. Create Replica Set Config File

Create a file:

```bash
vi ~/mongo-replset.conf
```

Paste the following:

```yaml
storage:
  dbPath: /usr/local/var/mongodb

net:
  bindIp: 127.0.0.1
  port: 27017

replication:
  replSetName: rs0
```

Save and exit (`ESC`, then `:wq`).

### 6. Start MongoDB with Replica Set

```bash
mongod --config ~/mongo-replset.conf
```

This will keep running — open another terminal for the next steps.

### 7. Connect Using mongosh

```bash
mongosh
```

### 8. Initiate the Replica Set

Inside the shell:

```javascript
rs.initiate()
```

Expected output:

```json
{
  "ok": 1,
  ...
}
```

You should now see:

```
rs0 [direct: primary] test>
```

---

## ✅ MongoDB is Ready

* Running at: `mongodb://localhost:27017`
* Replica set: `rs0`
* Supports: `$changeStream`, Rocket.Chat, Spring AI RAG, etc.

---

## Optional: Automate with Shell Script

You can wrap the setup into a script for repeatability. Let me know if you want a `setup-mongo-replset.sh` version!
