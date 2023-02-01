[<img src='https://user-images.githubusercontent.com/83868103/215836529-812ac1b8-029f-4f5d-bb72-8539c308b0f4.png' alt='discord'  width='33%'>](https://github.com/romanv1812/Teritori/blob/main/data/mainnet_guide.md)[<img src='https://user-images.githubusercontent.com/83868103/215836572-1ace2f52-bfa5-452a-a9bd-1382169bc8f2.png' alt='discord'  width='33.39%'>](https://restake.app/teritori/torivaloper1qy38xmcrnht0kt5c5fryvl8llrpdwer6atxj5u/stake)[<img src='https://user-images.githubusercontent.com/83868103/215836599-cb1990d2-2e43-4fc2-898a-c373bcb64677.png' alt='discord'  width='33%'>](https://restake.app/teritori/torivaloper1qy38xmcrnht0kt5c5fryvl8llrpdwer6atxj5u/stake)
```python
Services status  🟢
```
```YAML
RPC:      http://65.109.85.170:52657 # Indexer ON
gRPC:     http://65.109.85.170:35090
gRPC Web: http://65.109.85.170:35091
API:      http://65.109.85.170:27317
peer:     3669cd79a690f3e00e6cb87429b9d7b674955d8c@65.109.85.170:52656
seed:     3402bc832e2c1635a245b1301f0737b5f46f0ebd@65.109.85.170:10256
```
#
```python
Available RPCs UPD at 17:22 UTC ⏳ 01.02.23 🗓️ 
```
```YAML
```
#
```python
Shapshot UPD at 13:05 UTC ⏳ 01.02.23 🗓️ on HIGHT: Size: 5.3G
```
```YAML
http://65.109.85.170:8000/teritori-db.tar.lz4 # Pruning: custom 100\10\100 Indexer kv
```
#
```python
Wasm UPD at 11:20 UTC ⏳ 01.02.23 🗓️ on HIGHT: 1814793
```
```YAML
http://65.109.85.170:8000/teritori-wasm.tar.lz4
```
#
```python
Addrbook UPD at 17:22 UTC ⏳ 01.02.23 🗓️ 
```
```YAML
http://65.109.85.170:8000/addrbook.json
```
#
```python
Live Peers UPD at 17:22 UTC ⏳ 01.02.23 🗓️  Number Of Active Peers: = 0
```
```YAML

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
#
```python
Recovery From Snapshot
```
```bash
sudo systemctl stop teritorid
rm -rf $HOME/.teritorid/data; \
mkdir -p $HOME/.teritorid/data; \
cd $HOME/.teritorid/data
wget -O -  http://65.109.85.170:8000/teritori-1.tar | tar xf -
sudo systemctl start teritorid; \
sudo journalctl -u teritorid -f
```
