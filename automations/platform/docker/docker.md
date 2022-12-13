---
description: Installation Docker
---

# Installation Docker

{% hint style="info" %}
assume you was create user, refer [user-and-group-management.md](../../../security/user-and-group-management.md "mention")\
and\
you running in root
{% endhint %}

Create File setup.sh\


```bash
#!/bin/bash
USER="salinem"
apt-get update -y
apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release -y
mkdir -p /etc/apt/keyrings
if [ -f /etc/apt/keyrings/docker.gpg ]
then
    echo "GPG Available"
else
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
fi
if [ -f /etc/apt/sources.list.d/docker.list ]
then
   echo Directory Already Created
else
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
fi
apt-get update -y
apt-get install unzip docker-compose zsh docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
groupadd admin
useradd -m -G admin,docker -U  -s /bin/zsh ${USER}


```

Execute

```
bash setup.sh
```
