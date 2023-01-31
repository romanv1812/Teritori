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
```python
Available RPCs UPD at 17:00 ⏳ 31.01.2022 🗓️
```

```YAML
MONIKER: cyberG INDEXER: on HEIGHT: 3127244
RPC=135.181.138.160:46657

MONIKER: AlxVoy INDEXER: off HEIGHT: 3127244
RPC=144.76.97.251:26797

MONIKER: web34ever INDEXER: on HEIGHT: 3127244
RPC=135.181.138.161:26677

MONIKER: Munris INDEXER: off HEIGHT: 3127244
RPC=65.21.170.3:27657

MONIKER: landeros INDEXER: off HEIGHT: 3127244
RPC=185.193.66.68:26642

MONIKER: n0ok[MC] INDEXER: off HEIGHT: 3073040
RPC=213.239.217.52:41657
```
```python
Live Peers UPD at 17:00 ⏳ 31.01.2022 🗓️
```
```YAML
"31413c99357d0cfc48a46767ade171db2ea0205e@135.181.138.160:46656,22101a61b235e607d5d0ad51b698d7511ebf87e2@144.76.97.251:26796,c9be05c0ec5fa032cd10bd59bb6173c025b97f17@135.181.138.161:26676,5ae1012f9b0f4672d8152de903d115dd2f1a3ee3@65.21.170.3:27656,15dd94f68c450da2c3b7c60b6364e3dce6f0cbf2@185.193.66.68:26641,07d196ccefcadc548c6cd06cfea425f1544b1495@213.239.217.52:41656"
```
```bash
RPC-LIST: http://65.109.85.170:8000/rpc.txt #UPD every 3hr; 
SNAPSHOT: http://65.109.85.170:8000/teritori-1.tar #Pruning = default; Indexer "ON"; "FULL HISTORY; UPD every day"
Addrbook: http://65.109.85.170:8000/addrbook.json
```
```bash
#Recovery from state-sync
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
