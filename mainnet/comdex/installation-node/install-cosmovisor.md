---
description: Install cosmovisor
---

# Install Cosmovisor

#### Create FHS Cosmovisor

```bash
 mkdir -p ~/.comdex/cosmovisor/{genesis/bin,upgrades}
```

View of tree FHS

```
├── current //will be genrate linking
├── genesis
│   └── bin
└── upgrades
```

#### Create Shell Variable

Create file in **${HOME}/conf/cosmovisor-env,** this config was included in shell profile before ${HOME}/.zhrc or ${HOME}/.bashrc you can refer [.](./ "mention")

```bash
# Setup Cosmovisor
export DAEMON_NAME=comdex
export DAEMON_HOME=/app/comdex/.comdex
export DAEMON_ALLOW_DOWNLOAD_BINARIES=false
export DAEMON_LOG_BUFFER_SIZE=512
export DAEMON_RESTART_AFTER_UPGRADE=true
export UNSAFE_SKIP_BACKUP=true
```

#### Installation Cosmovisor

Install Binary

```bash
go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@v1.0.0
cp cosmovisor ${HOME}/bin
cp ${HOME}/bin/comdex ~/.comdex/cosmovisor/genesis/bin
```

Check Version

```bash
 cosmovisor version
 comdex version
```
