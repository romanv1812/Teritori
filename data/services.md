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
Available RPCs UPD at 14:30 UTC ⏳ 29.04.23 🗓️ 
```
```YAML
INDEXER: 🔴 HEIGHT: 5 MONIKER: bricks_teri
RPC=65.108.240.52:26657

INDEXER: 🟢 HEIGHT: 3118367 MONIKER: AlxVoy
RPC=65.109.93.152:26797

INDEXER: 🟢 HEIGHT: 3118367 MONIKER: chaos-01
RPC=54.36.62.225:13657

INDEXER: 🟢 HEIGHT: 3118367 MONIKER: geonodes
RPC=75.119.146.181:19657

INDEXER: 🟢 HEIGHT: 3118367 MONIKER: node
RPC=65.108.75.107:15657

INDEXER: 🟢 HEIGHT: 3118367 MONIKER: lesnik_utsa
RPC=65.108.6.45:36657

INDEXER: 🟢 HEIGHT: 3118367 MONIKER: ZenChainLabs-ToriLive
RPC=65.108.131.62:26657

INDEXER: 🔴 HEIGHT: 3118367 MONIKER: teritori
RPC=141.94.193.12:13657

INDEXER: 🟢 HEIGHT: 3118367 MONIKER: node
RPC=65.108.141.109:15657

INDEXER: 🔴 HEIGHT: 3118367 MONIKER: cyberG
RPC=141.95.65.26:27737

INDEXER: 🔴 HEIGHT: 3118367 MONIKER: CA13DA8EA16F56D7F2C64E64D1F82C3D
RPC=116.202.36.240:10657

```
#
```python
Shapshot UPD at 09:50 UTC ⏳ 29.04.23 🗓️ on HIGHT: 3115466 Size: 33G
```
```YAML
http://65.109.85.170:8000/teritori-db.tar.lz4 # Pruning: custom 100\10\100 Indexer kv
```
#
```python
Wasm UPD at 09:50 UTC ⏳ 29.04.23 🗓️ on HIGHT: 3115466
```
```YAML
http://65.109.85.170:8000/teritori-wasm.tar.lz4
```
#
```python
Addrbook UPD at 14:30 UTC ⏳ 29.04.23 🗓️ 
```
```YAML
http://65.109.85.170:8000/addrbook.json
```
#
```python
Live Peers UPD at 14:30 UTC ⏳ 29.04.23 🗓️  Number Of Active Peers: = 11
```
```YAML
a57b53a46e6f473b42a6db6e0c0f216b1611efcb@65.108.240.52:26656,6ef7a8bc7a3cc0856594f12570e8f2282a099dcf@65.109.93.152:26796,10a19941e819a9a89873398b1d52794929d245a0@54.36.62.225:13656,16f90d350de14a596ebdc683ce5e703c14e40bb3@75.119.146.181:19656,4cef2b81f82420434c6ce0dc43ca04ad18ef773f@65.108.75.107:15656,46b7ae20e3cc4264076a91c3601f3894a021a80d@65.108.6.45:36656,8e9624292123624e4eddc3f43189f08a0424127e@65.108.131.62:26656,317d9a102d4a04337c65571c18df0e98269dce87@141.94.193.12:13656,5cabaab828aea4bcc60e20c5a87b469c43023557@65.108.141.109:15656,e3b906fefa58783395fcf72086c698707908a558@141.95.65.26:27736,d40face481bc00a617d9a29c39be412a776e28c2@116.202.36.240:10656
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
