# Install MongoDB 6.0 and free5GC WebUI
MongoDB 6.0.4 released on 2023.01.26 now supports Ubuntu 22.04.
The process for the following OS is shown here.

- Ubuntu 20.04
- Ubuntu 22.04

---

<a id="toc"></a>

## Table of Contents

- [Install MongoDB 6.0](#install_mongodb)
- [Install free5GC WebUI](#install_webui)
- [Changelog (summary)](#changelog)

---
<a id="install_mongodb"></a>

## Install MongoDB 6.0

**Note. MongoDB v4.4.19 and later will not run on CPUs that do not support AVX instruction.
In this case, it is necessary to downgrade it to v4.4.18.
For reference, I wrote the steps to install v4.4.18 on Ubuntu 20.04 on Raspberry Pi 4B [here](https://github.com/s5uishida/install_mongodb_on_ubuntu_for_rp4b).**

```
# apt update
# apt install wget gnupg software-properties-common ca-certificates lsb-release
# wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/mongodb-6.gpg
# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list
# apt update
# apt install -y mongodb-org
```
```
# systemctl enable mongod
# systemctl start mongod
```

<a id="install_webui"></a>

## Install free5GC WebUI

It is assumed that MongoDB 6.0 and [Go](https://github.com/free5gc/free5gc/wiki/Installation) has been installed already.

First, install Yarn.
```
# apt install wget
# wget -qO - https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/yarn.gpg
# echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
# apt update
# apt install -y yarn
```
Next, install Node.js, see [here](https://github.com/nodesource/distributions).
```
# apt install -y ca-certificates curl gnupg
# mkdir -p /etc/apt/keyrings
# curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
# echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
# apt update
# apt install -y nodejs
```
Then, install free5GC WebUI.
```
# git clone --recursive -j `nproc` https://github.com/free5gc/free5gc.git
# cd free5gc/webconsole
# git checkout main
# cd ..
# make webconsole
```
Run the WebUI.
```
# cd webconsole
# bin/webconsole
...
[GIN-debug] Listening and serving HTTP on :5000
```
If necessary, set the IP address and port to bind as follows (ex. `192.168.0.141:5000`).

`free5gc/webconsole/config/webuicfg.yaml`
```diff
--- webuicfg.yaml.orig  2023-09-12 19:50:47.430599984 +0900
+++ webuicfg.yaml       2023-09-12 19:58:49.315487942 +0900
@@ -3,6 +3,10 @@
   description: WebUI initial local configuration
 
 configuration:
+  webServer:
+    scheme: http
+    ipv4Address: 192.168.0.141
+    port: 5000
   mongodb: # the mongodb connected by this webui
     name: free5gc # name of the mongodb
     url: mongodb://localhost:27017 # a valid URL of the mongodb
```

---
<a id="changelog"></a>

## Changelog (summary)

- [2023.09.12] Added how to set the IP address and port for WebUI.
- [2023.09.02] Updated the Node.js installation procedure.
- [2023.02.12] Initial release.
