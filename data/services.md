[<img src='https://user-images.githubusercontent.com/83868103/215836529-812ac1b8-029f-4f5d-bb72-8539c308b0f4.png' alt='discord'  width='33%'>](https://github.com/romanv1812/Teritori/blob/main/data/mainnet_guide.md)[<img src='https://user-images.githubusercontent.com/83868103/215836572-1ace2f52-bfa5-452a-a9bd-1382169bc8f2.png' alt='discord'  width='33.39%'>](https://restake.app/teritori/torivaloper1qy38xmcrnht0kt5c5fryvl8llrpdwer6atxj5u/stake)[<img src='https://user-images.githubusercontent.com/83868103/215836599-cb1990d2-2e43-4fc2-898a-c373bcb64677.png' alt='discord'  width='33%'>](https://restake.app/teritori/torivaloper1qy38xmcrnht0kt5c5fryvl8llrpdwer6atxj5u/stake)
```python
Services status  🟢
```
```YAML
RPC:      http://65.109.85.170:52657 # Indexer "ON" 
gRPC:     http://65.109.85.170:35090  
gRPC Web: http://65.109.85.170:35091  
API:      http://65.109.85.170:27317  
peer:     3669cd79a690f3e00e6cb87429b9d7b674955d8c@65.109.85.170:52656  
seed:     3402bc832e2c1635a245b1301f0737b5f46f0ebd@65.109.85.170:10256  
```
#
```python
Available RPCs UPD at 17:00 ⏳ 31.01.2022 🗓️
```

```YAML
INDEXER: 🟢 HEIGHT: 3127244 MONIKER: AlxVoy
RPC=http://135.181.138.160:46657

INDEXER: 🔴 HEIGHT: 3127244 MONIKER: web34ever
RPC=http://144.76.97.251:26797

INDEXER: 🔴 HEIGHT: 3127244 MONIKER: Munris
RPC=http://135.181.138.161:26677

INDEXER: 🟢 HEIGHT: 3127244 MONIKER: AlxVoy
RPC=http://65.21.170.3:27657

INDEXER: 🔴 HEIGHT: 3127244 MONIKER: landeros 
RPC=http://185.193.66.68:26642

INDEXER: 🟢 HEIGHT: 3127244 MONIKER: AlxVoy
RPC=http://213.239.217.52:41657
```
#
```python
Shapshot UPD at 17:00 ⏳ 31.01.2022 🗓️ on HIGHT: 2000000
```
```YAML
http://65.109.85.170:8000/teritori-1.tar # Pruning: custom 100\10\100 Indexer "on"
```
#
```python
Addrbook UPD at 17:00 ⏳ 31.01.2022 🗓️ 
```
```YAML
http://65.109.85.170:8000/addrbook.json 
```
#
```python
Live Peers UPD at 17:00 ⏳ 31.01.2022 🗓️ Count Of Peers = 50 
```
```YAML
"31413c99357d0cfc48a46767ade171db2ea0205e@135.181.138.160:46656,22101a61b235e607d5d0ad51b698d7511ebf87e2@144.76.97.251:26796,c9be05c0ec5fa032cd10bd59bb6173c025b97f17@135.181.138.161:26676,5ae1012f9b0f4672d8152de903d115dd2f1a3ee3@65.21.170.3:27656,15dd94f68c450da2c3b7c60b6364e3dce6f0cbf2@185.193.66.68:26641,07d196ccefcadc548c6cd06cfea425f1544b1495@213.239.217.52:41656"
```
---

# HOW TO USE?
```python
Recovery From State-Sync
```
```bash
sudo systemctl stop teritorid && \
teritorid tendermint unsafe-reset-all --home $HOME/.teritorid
RPC=http://65.109.85.170:52657
LATEST_HEIGHT=$(curl -s $RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 100)); \
TRUST_HASH=$(curl -s "$RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
peer="3669cd79a690f3e00e6cb87429b9d7b674955d8c@65.109.85.170:52656"
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peer\"/" $HOME/.teritorid/config/config.toml
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$RPC,$RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.teritorid/config/config.toml
wget -O $HOME/.teritorid/config/addrbook.json http://65.109.85.170:8000/addrbook.json
sudo systemctl restart teritorid && journalctl -u teritorid -f -o cat
```
```bash
#Recovery from snapshot
sudo systemctl stop teritorid
rm -rf $HOME/.teritorid/data; \
mkdir -p $HOME/.teritorid/data; \
cd $HOME/.teritorid/data

wget -O -  http://65.109.85.170:8000/teritori-1.tar | tar xf -

sudo systemctl start teritorid; \
sudo journalctl -u teritorid -f
```
