# Automate Create Systemd Cosmos Exporter

```
#!/bin/bash
#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia, 
# Let's drink coffee and talk about blockchain
#
# Chain Informations, visit : https://github.com/chainapsis/keplr-chain-registry/tree/main/cosmos

HOME_EXPORTER="${HOME}/cosmos-exporter"
BINARY_EXPORTER="${HOME_EXPORTER}/bin/cosmos-exporter"


declare -A _denom_exctract=(
    [1]=10
    [2]=100
    [3]=1000
    [4]=10000
    [5]=100000
    [6]=1000000
    [7]=10000000
    [8]=100000000
    [9]=1000000000
    [10]=10000000000
    [11]=100000000000
    [12]=1000000000000
    [13]=10000000000000
    [14]=100000000000000
    [15]=1000000000000000
    [16]=10000000000000000
    [17]=100000000000000000
    [18]=1000000000000000000
    [19]=10000000000000000000
    [20]=100000000000000000000
    [21]=1000000000000000000000
)

function help(){
   echo "Usage: $0 --file <filename> --listen <listen_address> --rpc <rpc_address> --grpc <grpc_address>"
   echo "Example: $0 --file planq_7070-2 --listen 0.0.0.0:1603 --rpc tcp://localhost:16703 --grpc localhost:1303"
}

while [[ $# -gt 0 ]]
do
    key="$1"
    case $key in
        --file)
            _registry_file="$2"
            shift 2
            ;;
        --listen)
            _listen_address="$2"
            shift 2
            ;;
        --rpc)
            _tendermint_rpc="$2"
            shift 2
            ;;
        --grpc)
            _grpc="$2"
            shift 2
            ;;
        *)
            help
            exit 1
            ;;
    esac
done

if [ -z "${_registry_file}" ] || [ -z "${_listen_address}" ] || [ -z "${_tendermint_rpc}" ] || [ -z "${_grpc}" ]; then
    echo "Missing or incomplete parameters."
    help
    exit 1
fi

if [ ! -f "chains/${_registry_file}" ]; then
    echo "Chain registry file '${_registry_file}' not found."
    wget -c https://raw.githubusercontent.com/chainapsis/keplr-chain-registry/main/cosmos/${_registry_file}.json -O chains/${_registry_file}.json
    if [ ! -f "chains/${_registry_file}.json" ]; then
       echo "Chain registry file '${_registry_file}.json' not found."
       exit 1
    fi
    _registry_file="${_registry_file}.json"
fi

mkdir -p chains
_bech_account_prefix=$(jq -r '.bech32Config.bech32PrefixAccAddr' "chains/${_registry_file}")
_bech_account_pubkey_prefix=$(jq -r '.bech32Config.bech32PrefixAccPub' "chains/${_registry_file}")
_bech_consensus_node_prefix=$(jq -r '.bech32Config.bech32PrefixConsAddr' "chains/${_registry_file}")
_bech_consensus_node_pubkey_prefix=$(jq -r '.bech32Config.bech32PrefixConsPub' "chains/${_registry_file}")
_bech_validator_prefix=$(jq -r '.bech32Config.bech32PrefixValAddr' "chains/${_registry_file}")
_bech_validator_pubkey=$(jq -r '.bech32Config.bech32PrefixValPub' "chains/${_registry_file}")
_denom=$(jq -r '.currencies[].coinDenom' "chains/${_registry_file}")
_denom_decimal=$(jq -r '.currencies[].coinDecimals' "chains/${_registry_file}")
_denom_decimal_dozens=${_denom_exctract[$_denom_decimal]}

data="--bech-account-prefix=${_bech_account_prefix} --bech-account-pubkey-prefix=${_bech_account_pubkey_prefix} --bech-consensus-node-prefix=${_bech_consensus_node_prefix} --bech-consensus-node-pubkey-prefix=${_bech_consensus_node_pubkey_prefix} --bech-validator-prefix=${_bech_validator_prefix} --bech-validator-pubkey-prefix=${_bech_validator_pubkey} --denom=${_denom} --denom-coefficient=${_denom_decimal_dozens} --listen-address=${_listen_address} --log-level=debug --tendermint-rpc=${_tendermint_rpc} --node=${_grpc}"

cat > ${HOME_EXPORTER}/systemd/exporter-${_registry_file:0:-5}.service << EOF
[Unit]
Description=Cosmos Exporter
After=network-online.target

[Service]
User=tendermint
TimeoutStartSec=0
CPUWeight=95
IOWeight=95
ExecStart=/app/tendermint/cosmos-exporter/bin/cosmos-exporter ${data}
Restart=always
RestartSec=2
LimitNOFILE=800000
KillSignal=SIGTERM

[Install]
WantedBy=multi-user.target
EOF

ls -ltr ${HOME_EXPORTER}/systemd/exporter-${_registry_file:0:-5}.service
cat ${HOME_EXPORTER}/systemd/exporter-${_registry_file:0:-5}.service

```
