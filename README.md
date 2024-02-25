# nulink-validator-node
NuLink Validator Node
<p align="center">
  <img height="auto" width="auto" src="https://cdn.publish0x.com/prod/fs/images/7c90f780ca96d06aa9e2627941fb1c3ef8ca73b64e0fa85d6e81c2fc65fa7cbd.png">
</p>

## Referensi

[Source](https://icy-gauge-010.notion.site/NuLink-Validator-Node-17851ba667054db7a1ba78e33e964325)

## Spesifikasi

### Persyaratan perangkat keras

| Komponen | Spesifikasi minimal |
|----------|---------------------|
|CPU|Intel Core 2|
|RAM|4 GB|
|Penyimpanan|30 GB|

### Persyaratan perangkat lunak

| Komponen | Spesifikasi minimal |
|----------|---------------------|
|Sistem Operasi|Debian/Ubuntu 20.04|

## CATATAN
- SAVE PUBLIC ADDRESS DAN ISI SALDO tBNB KE ALAMAT PUBLIC ADDRESS YANG NANTI KAMU DAPATKAN
- SAVE NAMA FILE KEYSTORE UTC YANG NANTI KAMU DAPATKAN
- SAVE PHRASE 
- SAVE PUBLIK KEY YANG NANTI KAMU DAPATKAN
- AMBIL FAUCET NULINK DI [DASHBOARD NULINK](https://dashboard.testnet.nulink.org/)
- KE MENU STAKING DAN STAKE TOKEN NULINK
- BOND WALLET DENGAN PUBLIC ADDRESS YANG KAMU DAPATKAN 

## 1. Setting Firewall

```
sudo ufw allow ssh
sudo ufw allow https
sudo ufw allow http
sudo ufw allow 9151
sudo ufw enable
```
```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz && tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz && cd geth-linux-amd64-1.10.23-d901d853/
./geth account new --keystore ./keystore
```
- Input password dan simpan Public address of the key dan Path of the secret key file

## 2. Install Docker

```
sudo apt-get install ca-certificates curl gnupg
```
```
sudo install -m 0755 -d /etc/apt/keyrings
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
```
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli [containerd.io](http://containerd.io/) docker-buildx-plugin docker-compose-plugin
```
```
docker pull nulink/nulink:latest
```
```
cd /root && mkdir nulink && cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/* /root/nulink
```
```
chmod -R 777 /root/nulink
```

**Ubah "< Isi Passwordmu >" menjadi password yang Kamu gunakan di awal tadi
```
export NULINK_KEYSTORE_PASSWORD=<Isi Passwordmu>
```
```
export NULINK_OPERATOR_ETH_PASSWORD=<Isi Passwordmu>
```
  
## 3. Install screen
```
sudo apt install screen
```

## 4. Buat screen
- Ubah " < nama screen > " menjadi apapun bebas
```
screen -S <nama screen>
```

## 5. Setting sebelum running
- Ubah "UTC--secretkey" dengan nama UTC yang kamu dapatkan diawal
- Ubah "-public address-" dengan public address yang kamu dapatkan diawal

```
docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/UTC--secretkey \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address -public address- \
--max-gas-price 10000000000
```

## 6. Run node

```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```

## 7. Cek log node

```
docker logs -f ursula
```


