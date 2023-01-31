
[<img src='https://user-images.githubusercontent.com/83868103/215839898-922bfe5d-558c-4f90-99c9-48b559ae7ed3.png' alt='discord'  width='33%'>](https://github.com/romanv1812/Teritori/blob/main/data/mainnet_guide.md)[<img src='https://user-images.githubusercontent.com/83868103/215839977-f59ab67d-e2dc-448b-b509-b762706a5ac9.png' alt='discord'  width='33.65%'>](https://restake.app/teritori/torivaloper1qy38xmcrnht0kt5c5fryvl8llrpdwer6atxj5u/stake)[<img src='https://user-images.githubusercontent.com/83868103/215840036-4804a30d-f30e-4674-849c-a053b5862718.png' alt='discord'  width='33.25%'>](https://github.com/romanv1812/Teritori/blob/main/data/services.md)
```### Links:   
[Explorer](https://www.mintscan.io/teritori) | |

[Faucet](https://faucet.okp4.network/)

[Discord](https://discord.gg/teritori) | [Twitter](https://twitter.com/TeritoriNetwork) | [Telegram](https://t.me/okp4network)

[Github](https://github.com/okp4)  | [Website](https://okp4.network/)  | [Medium](https://blog.okp4.network/) | [Linkedin](https://www.linkedin.com/company/okp4-open-knowledge-platform-for) | [Whitepaper](https://docs.okp4.network/whitepaper/abstract%20)
```

```
## Guide navigation:
* [Prepare](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#prepare)
* [All variables](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#all-variables)
* [Build and configuration](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#build-and-configuration)
* [Change PORT](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#change-port)
* [Memory optimization](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#memory-optimization)
* [Start node](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#start-node)
* [Create a validator](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#create-a-validator)
* [Snapshot](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#snapshot)
* [Update node](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#update-node)
* [Useful commands](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#useful-commands)
* [Delete node](https://github.com/romanv1812/OKP4/blob/main/Sidh/Setup%20your%20node.md#delete-node)
```
## Prepare
### Update if needed and install packages
```bash
sudo apt update && sudo apt upgrade -y && \
sudo apt install curl tar wget clang pkg-config libssl-dev libleveldb-dev jq build-essential bsdmainutils git make ncdu htop screen unzip bc fail2ban htop -y
```

### Installing GO v1.19.4
```bash
cd $HOME && \
ver="1.19.4" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

## Add variables
```bash
# Input your data
MONIKER="<YOUR_NEW_MONIKER>"
WALLET="<YOUR_WALLET_NAME>"
WEBSITE="<YOUR_WEBSITE>"
IDENTITY="<<YOUR_KEYBASE_ID>"
DETAILS="<YOUR_DESCRIPTION>"
SECURITY_CONTACT="<YOUR_EMAIL>"
```

```bash
echo "export MONIKER=$MONIKER" >> $HOME/.bash_profile && \
echo "export WALLET=$WALLET" >> $HOME/.bash_profile && \
echo "export WEBSITE=$WEBSITE" >> $HOME/.bash_profile && \
echo "export IDENTITY=$IDENTITY" >> $HOME/.bash_profile && \
echo "export DETAILS=$DETAILS" >> $HOME/.bash_profile && \
echo "export SECURITY_CONTACT=$SECURITY_CONTACT" >> $HOME/.bash_profile && \
source $HOME/.bash_profile
```
## Build and configuration
```bash 
# Build binary 
git clone https://github.com/TERITORI/teritori-chain.git teritorid && \
cd teritorid && \
make install
```
```bash 
# Initialisation
teritorid init $MONIKER --chain-id teritori-1 && \
teritorid config chain-id teritori-1 && \
teritorid config keyring-backend os
```
```bash
# Add wallet
teritorid keys add $WALLET
# or 
teritorid keys add $WALLET --recover
```
```bash
# Set variables 
VALOPER=$(teritorid keys show $WALLET --bech val -a) && \
ADDRESS=$(teritorid keys show $WALLET --address) && \
echo "export VALOPER=$VALOPER" >> $HOME/.bash_profile && \
echo "export ADDRESS=$ADDRESS" >> $HOME/.bash_profile && \
source $HOME/.bash_profile
```
```bash 
# Peers and seeds
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"26175f13ada3d61c93bca342819fd5dc797bced0@teritori.nodejumper.io:28656,722b63e6c65628b929f22013dcbcde980210cb44@176.9.127.54:26656,8f28518afd31a42ea81bb3232a50ab0cec4dcdf7@51.158.236.131:26656,647bbbc30d26fbbb2f7d19aafe30ed77a92c4748@[2a01:4f9:6b:2e5b::4]:26656,5a98d637a16b16bf425a4a785c9d11a7d1e5b8a0@65.21.131.215:26736,f813a00f52de54a49aea3211b89a65ae6133eac2@88.99.167.148:26686,358f13bd95d91517053a58f4d30205842672837f@104.37.187.214:60656,ce3baba928ae06cd3ff0af20aec888a82ddffef7@54.37.129.171:26656,3bd3a20d7c8a26a20927289a7a6bffecf71de53e@51.81.155.97:10856,48980875839186e08e12ebf0d9a2803b45206833@65.109.92.241:38026,526d8c7c44f59be9a39d7463c576b68c0db23174@65.108.234.23:15956\"/; s/^seeds *=.*/seeds = \"$SEEDS\"/" $HOME/.teritorid/config/config.toml
```
```bash 
# Download ZIP genesis 
wget -O $HOME/.teritorid/config/genesis.json https://media.githubusercontent.com/media/TERITORI/teritori-chain/v1.1.2/mainnet/teritori-1/genesis.json
```
## Change PORT
```bash
NODES_NUM="0"
```
```bash
sed -i.bak -e "\
s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:$((NODES_NUM+26))658\"%; \
s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://0.0.0.0:$((NODES_NUM+26))657\"%; \
s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:$((NODES_NUM+6))060\"%; \
s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:$((NODES_NUM+26))656\"%; \
s%^external_address = \"\"%external_address = \"`echo $(wget -qO- eth0.me):$((NODES_NUM+26))656`\"%; \
s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":$((NODES_NUM+26))660\"%" $HOME/.teritorid/config/config.toml
```
```bash
sed -i.bak -e "\
s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:$((NODES_NUM+1))317\"%; \
s%^address = \":8080\"%address = \":$((NODES_NUM+8))080\"%; \
s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:$((NODES_NUM+9))090\"%; \
s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:$((NODES_NUM+9))091\"%" $HOME/.teritorid/config/app.toml
```
```bash
echo "export NODE=http://localhost:$((NODES_NUM+26))657" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
teritorid config node $NODE
```
## Memory optimization
```bash 
# Memory optimization. Removes unused data from the database.
indexer="null" && \
min_retain_blocks=1 && \
snapshot_interval="100" && \
pruning="custom" && \
pruning_keep_recent="100" && \
pruning_keep_every="0" && \
pruning_interval="10" && \
min_retain_blocks="1" && \
inter_block_cache="false" && \
sed -i.bak -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.teritorid/config/config.toml && \
sed -i.bak -e "s/^min-retain-blocks *=.*/min-retain-blocks = \"$min_retain_blocks\"/" $HOME/.teritorid/config/app.toml && \
sed -i.bak -e "s/^snapshot-interval *=.*/snapshot-interval = \"$snapshot_interval\"/" $HOME/.teritorid/config/app.toml && \
sed -i.bak -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.teritorid/config/app.toml && \
sed -i.bak -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.teritorid/config/app.toml && \
sed -i.bak -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.teritorid/config/app.toml && \
sed -i.bak -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.teritorid/config/app.toml && \
sed -i.bak -e "s/^min-retain-blocks *=.*/min-retain-blocks = \"$min_retain_blocks\"/" $HOME/.teritorid/config/app.toml && \
sed -i.bak -e "s/^inter-block-cache *=.*/inter-block-cache = \"$inter_block_cache\"/" $HOME/.teritorid/config/app.toml
```

## Start node
```bash 
# Create service 
sudo tee /etc/systemd/system/teritorid.service > /dev/null <<EOF
[Unit]
Description=teritorid Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which teritorid) start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
```bash
# Start service 
sudo systemctl daemon-reload && \
sudo systemctl enable teritorid && \
sudo systemctl restart teritorid && \
sudo journalctl -u teritorid -f -o cat
```
```bash
# Check synchronization of your node, if the result is false, the node is synchronized
curl -s $NODE/status | jq .result.sync_info.catching_up
```
## Create a validator
```bash 
teritorid tx staking create-validator \
  --amount=1000000utori \
  --pubkey=$(teritorid tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=teritori-1 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation=1000000 \
  --fees=0utori \
  --from=$WALLET \
  --identity=$IDENTITY \
  --website=$WEBSITE \
  --details=$DETAILS \
  --security-contact=$SECURITY_CONTACT \
  -y
  ```
## Snapshot
If you did not find an open RPC, you can use the [parser](https://github.com/romanv1812/OKP4/blob/main/rpc_parser.sh) to search in the addrbook.
<img width="1078" alt="image" src="https://user-images.githubusercontent.com/83868103/210120043-e662ea3d-5c2e-4d06-9f62-162c6005ec70.png">

```bash
sudo systemctl stop teritorid && \
teritorid tendermint unsafe-reset-all --home $HOME/.teritorid --keep-addr-book
```
```bash
# RPC example: 5.161.106.127:26657
SNAP_RPC=""
```
```bash
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT-100)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
```
```bash
# Check data
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
# Output example (numbers will be different):
# 376080 374080 F0C78FD4AE4DB5E76A298206AE3C602FF30668C521D753BB7C435771AEA47189
```
```bash
# Peer example: 22cd56c20132817d609025f42c5e263e70157e64@5.161.106.127:26656
peers=""
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.teritorid/config/config.toml
```
```bash
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.teritorid/config/config.toml
```
```bash
sudo systemctl restart teritorid && sudo journalctl -u teritorid -f -o cat
```
## Update node
```bash
TAG_NAME=""
```
```bash
sudo systemctl stop teritorid && \
cd teritorid && \
git pull; \
git checkout tags/$TAG_NAME && \
make clean; \
make install && \
sudo systemctl restart teritorid && \
journalctl -u teritorid -f -o cat
```
## Useful commands

### Node status
```bash
# Service logs
journalctl -u teritorid -f -o cat
```
```bash
# Service status
systemctl status teritorid
```
```bash
# Check node status
curl -s $NODE/status
```
```bash
# Check synchronization of your node, if the result is false, the node is synchronized
curl -s $NODE/status | jq .result.sync_info.catching_up
```
```bash
# Check consensus 
curl -s $NODE/consensus_state  | jq '.result.round_state.height_vote_set[0].prevotes_bit_array'
```
```bash
# Connected peers
curl -s $NODE/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr | split(":")[2])"' | wc -l
```

### Validator info
```bash
# Get validator address 
echo $VALOPER
```
```bash
# Get wallet address
echo $ADDRESS
```
```bash
# Jail, tombstoned, start_height, index_offset
teritorid q slashing signing-info $(teritorid tendermint show-validator)
```
```bash
# Get peer (e.g. 72cc19c8435d662677b2ea627e649f39b5bc8abb@5.161.70.110:26656
echo "$(teritorid tendermint show-node-id)@$(curl ifconfig.me):$(curl -s $NODE/status | jq -r '.result.node_info.listen_addr' | cut -d':' -f3)"
```
### Wallet
```bash
# Get balance
teritorid q bank balances $ADDRESS
```

### Voting
```bash
# Vote
teritorid tx gov vote <PROPOSAL_ID> <yes|no> --from $WALLET --fees 5000utori -y
```
```bash
# Check all voted proposals
teritorid q gov proposals --voter $ADDRESS
```

### Actions
```bash
# Edit validator
teritorid tx staking edit-validator --website="<YOUR_WEBSITE>" --details="<YOUR_DESCRIPTION>" --moniker="<YOUR_NEW_MONIKER>" --from=$WALLET --fees 5000utori
```
```bash
# Unjail
teritorid tx slashing unjail --from $WALLET --fees 5000utori
```
```bash
# Bond more tokens (if you want increase your validator stake you should bond more to your valoper address):
teritorid tx staking delegate $VALOPER <TOKENS_COUNT>utori--from $WALLET --fees 5000utori -y
```
```bash
# Undelegate
teritorid tx staking unbond $VALOPER <TOKENS_COUNT>utori --from $WALLET --fees 5000utori -y
```
```bash
# Send tokens. 1 token = 1000000 (Cosmos)
teritorid tx bank send $WALLET <WALLET_TO> <TOKENS_COUNT>utori --fees 5000utori
# e.g. teritorid tx bank send $WALLET cosmos10h3t6rtrjwxqlw0jgwc540rthuclhvrzhndkeg 1000000utori --gas auto
```
```bash
# Change peers and seeds
peers="<PEERS>"
seeds="<SEEDS>"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/; s/^seeds *=.*/seeds = \"$seeds\"/" $HOME/.teritorid/config/config.toml
```
```bash
# Reset private validator file to genesis state and delete addrbook.json
teritorid tendermint unsafe-reset-all --home $HOME/.teritorid
```

### All validators info
```bash
# List of all active validators 
teritorid q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl
```
```bash
# List of all inactive validators 
teritorid q staking validators -o json --limit=1000 \
| jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' \
| jq -r '.tokens + " - " + .description.moniker' \
| sort -gr | nl
```

### Another useful commands
```bash
# Root -> your node
su -l $USER_NAME
```
```bash
# Check internet connection
curl -sL yabs.sh | bash -s -- -fg
```
```bash
# Server load
sudo htop
```
```bash
# File structure
ncdu
```
## Delete node
```bash
sudo systemctl stop teritorid && \
sudo systemctl disable teritorid; \
sudo rm /etc/systemd/system/teritorid.service; \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf .teritorid teritorid; \
sudo rm $(which teritorid)
```
