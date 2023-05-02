[<img src='https://user-images.githubusercontent.com/83868103/215836529-812ac1b8-029f-4f5d-bb72-8539c308b0f4.png' alt='discord'  width='33%'>](https://github.com/romanv1812/Teritori/blob/main/data/mainnet_guide.md)[<img src='https://user-images.githubusercontent.com/83868103/215836572-1ace2f52-bfa5-452a-a9bd-1382169bc8f2.png' alt='discord'  width='33.39%'>](https://restake.app/teritori/torivaloper1qy38xmcrnht0kt5c5fryvl8llrpdwer6atxj5u/stake)[<img src='https://user-images.githubusercontent.com/83868103/215836599-cb1990d2-2e43-4fc2-898a-c373bcb64677.png' alt='discord'  width='33%'>](https://restake.app/teritori/torivaloper1qy38xmcrnht0kt5c5fryvl8llrpdwer6atxj5u/stake)
```python
Services status  🟢
```
```YAML
RPC:      http://65.109.85.170:52657 # Indexer ON
gRPC:     http://65.109.85.170:35090
gRPC Web: http://65.109.85.170:35091
API:      http://65.109.85.170:27317
peer:     14740e6faf16ab85a98ff5911241bb4b926b9c08@65.109.85.170:52656
seed:     3402bc832e2c1635a245b1301f0737b5f46f0ebd@65.109.85.170:10256
```
#
```python
Available RPCs UPD at 06:51 UTC ⏳ 02.05.23 🗓️ 
```
```YAML
INDEXER: 🟢 HEIGHT: 3158435 MONIKER: ZenChainLabs-ToriLive
RPC=65.108.131.62:26657

```
#
```python
Shapshot UPD at 03:32 UTC ⏳ 02.05.23 🗓️ on HIGHT: 3156375 Size: 36G
```
```YAML
http://65.109.85.170:8000/teritori-db.tar.lz4 # Pruning: custom 100\10\100 Indexer kv
```
#
```python
Wasm UPD at 03:32 UTC ⏳ 02.05.23 🗓️ on HIGHT: 3156375
```
```YAML
http://65.109.85.170:8000/teritori-wasm.tar.lz4
```
#
```python
INDEXER: 🔴 HEIGHT: 5 MONIKER: bricks_teri
RPC=65.108.240.52:26657
Addrbook UPD at 06:51 UTC ⏳ 02.05.23 🗓️ 

```
```YAML
http://65.109.85.170:8000/addrbook.json
```
#
```python
Live Peers UPD at 06:51 UTC ⏳ 02.05.23 🗓️  Number Of Active Peers: = 2
```
```YAML
8e9624292123624e4eddc3f43189f08a0424127e@65.108.131.62:26656,a57b53a46e6f473b42a6db6e0c0f216b1611efcb@65.108.240.52:26656
INDEXER: 🔴 HEIGHT: 3158435 MONIKER: teritori
RPC=141.94.193.12:13657

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
peer="14740e6faf16ab85a98ff5911241bb4b926b9c08@65.109.85.170:52656"
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peer\"/" $HOME/.teritorid/config/config.toml
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$RPC,$RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.teritorid/config/config.toml
wget -O $HOME/.teritorid/config/addrbook.json http://65.109.85.170:8000/addrbook.json
sudo systemctl restart teritorid && journalctl -u teritorid -f -o cat
```
#
```python
Download Wasm
```
```bash
curl -s http://65.109.85.170:8000/teritori-wasm.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.teritorid/data
```
#
```python
Download Addrbook
```
```bash
curl -s http://65.109.85.170:8000/addrbook.json > $HOME/.teritorid/config/addrbook.json
```
#
```python
Recovery From Snapshot
```
```bash
sudo systemctl stop teritorid
rm -rf $HOME/.teritorid/data; \
mkdir -p $HOME/.teritorid/data; \
cd $HOME/.teritorid/data
wget -O -  http://65.109.85.170:8000/teritori-db.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.teritorid
sudo systemctl start teritorid; \
sudo journalctl -u teritorid -f
```
