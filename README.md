# Install MongoDB 6.0 and free5GC WebUI

---

<h2 id="toc">Table of Contents</h2>

- [Install MongoDB 6.0](#install_mongodb)
  - [For Ubuntu 20.04](#ubuntu2004_mongodb)
  - [For Ubuntu 22.04](#ubuntu2204_mongodb)
- [Install free5GC WebUI](#install_webui)
  - [For Ubuntu 20.04](#ubuntu2004_webui)
  - [For Ubuntu 22.04](#ubuntu2204_webui)
- [Changelog (summary)](#changelog)

---
<h2 id="install_mongodb">Install MongoDB 6.0</h2>

<h3 id="ubuntu2004_mongodb">For Ubuntu 20.04</h3>

```
# apt update
# apt install wget gnupg software-properties-common ca-certificates lsb-release
# wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | apt-key add -
# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list
# apt update
# apt install -y mongodb-org
```
```
# systemctl enable mongod
# systemctl start mongod
```

<h3 id="ubuntu2204_mongodb">For Ubuntu 22.04</h3>

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

<h2 id="install_webui">Install free5GC WebUI</h2>

It is assumed that MongoDB 6.0 and [Go](https://github.com/free5gc/free5gc/wiki/Installation) has been installed already.

<h3 id="ubuntu2004_webui">For Ubuntu 20.04</h3>

First, install Yarn and Node.js.
```
# apt install wget
# wget -qO - https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
# echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
# apt update
# apt install yarn
```
```
# wget -qO - https://deb.nodesource.com/setup_18.x | bash -
# apt install nodejs
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

<h3 id="ubuntu2204_webui">For Ubuntu 22.04</h3>

First, install Yarn and Node.js.
```
# apt install wget
# wget -qO - https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/yarn.gpg
# echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
# apt update
# apt install yarn
```
```
# wget -qO - https://deb.nodesource.com/setup_18.x | bash -
# apt install nodejs
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

---
<h2 id="changelog">Changelog (summary)</h2>

- [2023.02.12] Initial release.
