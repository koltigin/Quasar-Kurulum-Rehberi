# Quasar Kurulum Rehberi

[Kaynak](https://docs.quasar.fi/) / [Explorer](https://quasar.explorers.guru/)
![Quasar-GitHub](https://user-images.githubusercontent.com/102043225/221850482-6be68968-63a4-493a-99d0-29497b4ce976.jpg)

## Bağlantılar
 ✔️ [Website](https://www.quasar.fi/)<br>
 ✔️ [Blockchain Explorer](https://quasar.explorers.guru/)<br>
 ✔️ [Doküman](https://docs.quasar.fi/)<br>
 ✔️ [Discord](https://discord.gg/quasarfi)<br>
 ✔️ [Testnet Platfrom](https://testnet.quasar.fi/)<br>
 ✔️ [Crew3](https://crew3.xyz/c/quasar/invite/mIVQ2sIaT_Xk7kdREddMc)

## Gereksinimler 
| Bileşenler | Minimum Gereksinimler | **Tavsiye Edilen Gereksinimler** | 
| ------------ | ------------ | ------------ |
| CPU |	4 | 4 |
| RAM	| 8 GB | 16 GB |
| Storage	| 250 GB SSD | 1 TB SSD |

## Sistemi Güncelleme
```shell
apt update && apt upgrade -y
```

## Gerekli Kütüphanelerin Kurulması
```shell
apt install make clang pkg-config libssl-dev libclang-dev build-essential git curl ntp jq llvm tmux htop screen gcc lz4 -y < "/dev/null"
```

## Go Kurulumu
```shell
ver="1.20"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
rm -rf /usr/local/go
tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm -rf "go$ver.linux-amd64.tar.gz"
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

## Değişkenleri Yükleme
aşağıda değiştirmeniz gereken yerleri yazıyorum.
* `$QSR_NODENAME` validator adınız
* `$QSR_WALLET` cüzdan adınız
*  Eğer portu başka bir node kullanıyorsa aşağıdan değiştirebilirsiniz.
```shell
echo "export QSR_NODENAME=$QSR_NODENAME"  >> $HOME/.bash_profile
echo "export QSR_WALLET=$QSR_WALLET" >> $HOME/.bash_profile
echo "export QSR_PORT=11" >> $HOME/.bash_profile
echo "export QSR_CHAIN_ID=qsr-questnet-04" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Örnek
Node ve Cüzdan adımızın `Mehmet` olduğunu varsayalım. Kod aşağıdaki şekilde düzenlenecektir. 
```shell
echo "export QSR_NODENAME=Mehmet"  >> $HOME/.bash_profile
echo "export QSR_WALLET=Mehmet" >> $HOME/.bash_profile
echo "export QSR_PORT=18" >> $HOME/.bash_profile
echo "export QSR_CHAIN_ID=qsr-questnet-04" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Quarar'ın Kurulması

```shell
curl -L https://github.com/quasar-finance/binary-release/raw/main/v0.0.2-alpha-11/quasarnoded-linux-amd64 > quasard
chmod +x quasard
sudo mv quasard /usr/local/bin/
quasard version
```
Versiyon çıktısı `0.0.2-alpha-11` olacak.

## Uygulamayı Yapılandırma ve Başlatma
```shell
quasard config keyring-backend test
quasard config chain-id $QSR_CHAIN_ID
quasard init --chain-id $QSR_CHAIN_ID $QSR_NODENAME
```

## Genesis Dosyasının Kopyalanması
```shell
curl -s https://raw.githubusercontent.com/quasar-finance/questnet/main/v04/definitive-genesis.json > $HOME/.quasarnode/config/genesis.json
curl -s https://snapshots2-testnet.nodejumper.io/quasar-testnet/addrbook.json > $HOME/.quasarnode/config/addrbook.json
```

## Minimum GAS Ücretinin Ayarlanması
```shell
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0uqsr"|g' $HOME/.quasarnode/config/app.toml
```

## Indexer'i Kapatma (Opsiyonel)
```shell
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.quasarnode/config/config.toml
```

## SEED ve PEERS Ayarlanması
```shell
SEEDS="7ed8e233e5fdb21bf70ac7f635130c7a8b0a4967@quasar-testnet-seed.swiss-staking.ch:10056"
PEERS="d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:48656,fdc1babb7ad4d97a911d32b0545220c8ceca57a8@128.199.8.206:53656,11d9e9d25cc78d2a0270a3d5a7e849775b110e64@185.249.225.63:48656,b1197bd0946b3d2d462fcc7548a79e87101d2389@65.108.141.109:38656,5265b02d7a5e43275f3383e6385cdc0506b99e1a@65.109.28.177:28656,3c8afd3c39b7ab28cdb801e45ea4d9249a51e22b@88.99.161.162:20656,18134130ea3156767191d89e9654b0117f54460b@43.156.246.92:26656,881db78e40385d87614cb847c2a19e8ead25b52c@43.159.47.25:26656,966acc999443bae0857604a9fce426b5e09a7409@65.108.105.48:18256,02e9ca11b64c2c6710f9642a79d576d7134ea215@43.159.54.23:26656,45848bc173bddbf7c685938dfada535ee5a1895b@65.109.23.114:18256,b122b1d76f5d676233ebbd0011c2fd7bf5960e53@43.156.10.155:26656,0bc5253d4db2af78fb7c96fa77e5f0734ea10331@43.156.61.70:26656,e339401b40f12aaf9efca323214040f51f3ff4b6@65.109.87.135:18656,d7b332b225b27a0c3338e9bce1e3ef1dd37d0c10@43.156.36.141:26656,dcf78ede935a42361895928d35119ed4789abb9c@65.109.85.225:8090,eea117634dd5e280e94e931ecf5d3f2b462bcd9d@43.156.69.134:26656,ed3bb97860ef0197a00b27127e0aaa9dd7af2817@94.130.177.114:29656,b3299d6ad3ff7452cba7d651d2c678e565fdd281@43.156.72.55:26656,afcfb038b1235a9a41128e73c4ac2bc6838b5f04@129.226.216.213:26656,9e55c6920edce61ea2a7328e437a650e8884f090@209.126.2.83:26656,fafd24c060f625a610d632a314ae916555b3d11a@43.156.98.245:26656,46ab1cfab36eeebc9b073612d69fee1c634b22d4@147.182.244.154:26656,8df102b790607051362abacf34ec671c37d096bf@43.153.203.222:26656,5271226f8a6a0f981720b7f8656cf424db0ce580@129.226.201.224:26656,f79f912153840caf703393d784b94b2e50371c61@43.156.118.199:26656,231b35d147fdd2bc9027106eeef63b448f1f404b@43.156.225.47:26656,01234fe8e5aa29abe6b5d30764f9b50ca5cdeb98@139.180.139.191:26656,c5b0e2e7ac4b16a6bd7619e9335f687028cb1d5e@43.156.137.165:26656,b5fcb5c89e5ec40188be886625acd349df52795a@43.156.137.130:26656,ecaba1301b48b32d8c97bb6a2eef6b9fb27169c0@64.176.45.149:26656,3a5480bfdf27c1b103f0257056b000175e3e1a06@176.124.220.21:26656,e2bfc397cdfd70fd731cb97d568d869b36b97456@43.153.205.74:26656,23b3f4a6d894400664f464613971da60465a4a36@43.156.120.96:26656,b26391f18fe3a4b23f478f04157072907e5df3c5@43.153.205.91:26656,b82edb8acc9f7d486de3b4fcd857d7c588d6956b@43.154.17.254:26656,a23f002bda10cb90fa441a9f2435802b35164441@38.146.3.203:18256,674166550258de01e46207e565598e856aac6f62@43.154.168.128:26656,b35f3493df8c3be232fe75ef7f4d0cb9d0f59668@65.109.70.23:18256,15b2df2c900a0d1a7625ccf9bc15e7c043a9044c@43.154.143.254:26656,bfa59196c109932786885c97ccd7df7dd434d26a@43.156.233.200:26656,7490a9690d82d43f8bcfa257cdf798e8e75a4d46@38.242.130.23:29656,e3b45f7be0b6e109d16458f79a84a434bb85430f@212.118.52.14:29656,4ad7ce03e53f0edb2a1debb2d69ff754a0cbb029@142.132.158.158:23656,7d57a0d05e0a4069cb0e7125a7da9cfd3a397880@108.166.201.96:26656,d21319cfc5fff19810ae8797b4749b50018df365@94.130.36.149:26666,955ee8d360e80a7baecc0ee3ea8afa436a7aee23@43.154.73.226:26656,089763be3736463c507427b37752a0d8d465b8c6@149.102.139.80:29656,c3c648ff7683273d85c0d8e24b823b39587e38e3@178.128.85.30:53656,08f409ee63de194847ea3da6b9c593cdb3f9692d@176.124.220.124:26656"
sed -i 's|^seeds *=.*|seeds = "'$SEEDS'"|; s|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.quasarnode/config/config.toml
```

## Prometheus'u Aktif Etme
```shell
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.quasarnode/config/config.toml
```

## Pruning'i Ayarlama
```shell
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.quasarnode/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.quasarnode/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.quasarnode/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.quasarnode/config/app.toml
```

## Portları Ayarlama
```shell
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${QSR_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${QSR_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${QSR_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${QSR_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${QSR_PORT}660\"%" $HOME/.quasarnode/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${QSR_PORT}317\"%; s%^address = \":8080\"%address = \":${QSR_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${QSR_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${QSR_PORT}091\"%" $HOME/.quasarnode/config/app.toml
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:${QSR_PORT}657\"%" $HOME/.quasarnode/config/client.toml
```

## Servis Dosyası Oluşturma
```shell
sudo tee /etc/systemd/system/quasard.service > /dev/null << EOF
[Unit]
Description=Quasar Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which quasard) start
Restart=on-failure
RestartSec=10
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target
EOF
```

## Snapshot Yükleme (NodeJumper)
```shell
SNAP_NAME=$(curl -s https://snapshots2-testnet.nodejumper.io/quasar-testnet/info.json | jq -r .fileName)
curl "https://snapshots2-testnet.nodejumper.io/quasar-testnet/${SNAP_NAME}" | lz4 -dc - | tar -xf - -C "$HOME/.quasarnode"
```

## Servisi Başlatma
```shell
sudo systemctl daemon-reload
sudo systemctl enable quasard
sudo systemctl start quasard
```

## Logları Kontrol Etme
```shell
journalctl -u quasard -f -o cat
```  

## Cüzdan Oluşturma

### Yeni Cüzdan Oluşturma
`$QSR_WALLET` bölümünü değiştirmiyoruz kurulumun başında cüzdanımıza değişkenler ile isim belirledik.
```shell 
quasard keys add $QSR_WALLET
```  

### Var Olan Cüzdanı İçeri Aktarma
```shell
quasard keys add $QSR_WALLET --recover
```

## Cüzdan ve Valoper Bilgileri
Burada cüzdan ve valoper bilgilerimizi değişkene ekliyoruz.

```shell
QSR_WALLET_ADDRESS=$(quasard keys show $QSR_WALLET -a)
QSR_VALOPER_ADDRESS=$(quasard keys show $QSR_WALLET --bech val -a)
```

```shell
echo 'export QSR_WALLET_ADDRESS='${QSR_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export QSR_VALOPER_ADDRESS='${QSR_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Faucet
[Quasar](https://discord.gg/quasarfi) adresine giderek `#🚰⎮testnet-faucet` kanalından `$request cuzdan-adresi` şeklinde mesaj atarak token istiyoruz. 

## Faucet 2
[stakely.io](https://stakely.io/en/faucet/quasar-testnet) adresine giderek tweet atıp token istiyoruz.

🔴 **BU AŞAMADAN SONRA NODE'UMUZUN EŞLEŞMESİNİ BEKLİYORUZ.**

## Senkronizasyonu Kontrol Etme
`false` çıktısı almadıkça bir sonraki yani validator oluşturma adımına geçmiyoruz.
```shell
quasard status 2>&1 | jq .SyncInfo
```

🔴 **Eşleşme tamamlandıysa aşağıdaki adıma geçiyoruz.**


## Validator Oluşturma
 Aşağıdaki komutta aşağıda berlirttiğim yerler dışında bir değişiklik yapmanız gerekmez;
   - `identity`  burada `XXXX1111XXXX1111` yazan yere [keybase](https://keybase.io/) sitesine üye olarak size verilen kimlik numaranızı yazıyorsunuz.
   - `details` `Always forward with the Anatolian Team 🚀` yazan yere kendiniz hakkında bilgiler yazabilirsiniz.
   - `website`  `https://anatolianteam.com` yazan yere varsa bir siteniz ya da twitter vb. adresinizi yazabilirsiniz.
   - `security-contact`  E-posta adresiniz.
 ```shell 
quasard tx staking create-validator \
--amount=1000000uqsr \
--pubkey=$(quasard tendermint show-validator) \
--moniker=$QSR_NODENAME \
--chain-id=$QSR_CHAIN_ID \
--commission-rate=0.10 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.05 \
--min-self-delegation="1" \
--gas-prices=0.1uqsr \
--gas-adjustment=1.5 \
--gas=auto \
--from=$QSR_WALLET \
--details="Always forward with the Anatolian Team 🚀" \
--security-contact="xxxxxxx@gmail.com" \
--website="https://anatolianteam.com" \
--identity="XXXX1111XXXX1111" \
--yes
```

## YARARLI KOMUTLAR

## Logları Kontrol Etme 
```
journalctl -fu quasard -o cat
```

### Sistemi Başlatma

```
systemctl start quasard
```

### Sistemi Durdurma
```
systemctl stop quasard
```

### Sistemi Yeniden Başlatma
```
systemctl restart quasard
```

### Node Senkronizasyon Durumu
```
quasard status 2>&1 | jq .SyncInfo
```
```
curl -s localhost:26657/status | jq .result.sync_info
```

### Validator Bilgileri
```
quasard status 2>&1 | jq .ValidatorInfo
```

### Node Bilgileri

```
quasard status 2>&1 | jq .NodeInfo
```

### Node ID Öğrenme

```
quasard tendermint show-node-id
```


### Node IP Adresini Öğrenme

```
curl icanhazip.com
```

### Cüzdanların Listesine Bakma

```
quasard keys list
```

### Cüzdan Adresini Görme

```
quasard keys show $QSR_WALLET --bech val -a
```

### Cüzdanı İçeri Aktarma

```
quasard keys add $QSR_WALLET --recover
```

### Cüzdanı Silme

```
quasard keys delete $QSR_WALLET
```

### Cüzdan Bakiyesine Bakma

```
quasard query bank balances $QSR_WALLET_ADDRESS
```

### Bir Cüzdandan Diğer Bir Cüzdana Transfer Yapma

```
quasard tx bank send $QSR_WALLET_ADDRESS GONDERILECEK_CUZDAN_ADRESI 100000000uqsr
```

### Proposal Oylamasına Katılma
```
quasard tx gov vote 1 yes --from $QSR_WALLET --chain-id=$QSR_CHAIN_ID
```


### Validatore Stake Etme / Delegate Etme

```
quasard tx staking delegate $VALOPER_ADDRESS 100000000uqsr --from=$QSR_WALLET --chain-id=$QSR_CHAIN_ID --gas-prices=0.1uqsr --gas-adjustment=1.5 --gas=auto
```

### Mevcut Validatorden Diğer Validatore Stake Etme / Redelegate Etme
<srcValidatorAddress>: Mevcut Stake edilen validatorün adresi
<destValidatorAddress>: Yeni stake edilecek validatorün adresi 
```
quasard tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 100000000uqsr --from=$QSR_WALLET --chain-id=$QSR_CHAIN_ID --gas-prices=0.1uqsr --gas-adjustment=1.5 --gas=auto
```

### Ödülleri Çekme

```
quasard tx distribution withdraw-all-rewards --from=$QSR_WALLET --chain-id=$QSR_CHAIN_ID --gas=auto
```

### Komisyon Ödüllerini Çekme

```
quasard tx distribution withdraw-rewards $VALOPER_ADDRESS --from=$QSR_WALLET --commission --chain-id=$QSR_CHAIN_ID
```

### Validator İsmini Değiştirme
NEWNODENAME yazan yere yeni validator/moniker isminizi yazınız. TR karakçer içermemelidir.

```
quasard tx staking edit-validator \
--moniker=NEWNODENAME \
--chain-id=$QSR_CHAIN_ID \
--from=$QSR_WALLET
```

### Validator Komisyon Oranını Degiştirme
```
quasard tx staking edit-validator --commission-rate "0.02" --moniker=$QSR_NODENAME --chain-id=$QSR_CHAIN_ID --from=$QSR_WALLET
```

### Validator Bilgilerinizi Düzenleme
Bu bilgileri değiştirmeden önce https://keybase.io/ adresine kayıt olarak aşağıdaki kodda görüldüğü gibi 16 haneli (XXXX0000XXXX0000) kodunuzu almalısınız. Ayrıca profil resmi vs. ayarları da yapabilirsiniz. 
$NODENAME: Yeni node adınız (moniker)
$QSR_WALLET: Cüzdan adınız, değiştirmeniz gerekmez. Değişkenlere ekledik çünkü.
```
quasard tx staking edit-validator \
--moniker=$NODENAME \
--identity=XXXX0000XXXX0000 \
--website="VARSA WEBSITENIZI YAZABILIRSINIZ" \
--details="BU BOLUME KENDINIZI TANITAN BIR CUMLE YAZABILIRSINIZ" \
--chain-id=$QSR_CHAIN_ID \
--from=$QSR_WALLET
```

### Validatoru Jail Durumundan Kurtarma 

```
quasard tx slashing unjail \
  --broadcast-mode=block \
  --from=$QSR_WALLET \
  --chain-id=$QSR_CHAIN_ID \
  --gas=auto
  --gas-adjustment=1.4
```

### Node'u Tamamen Silme 

```
systemctl stop quasard && \
systemctl disable quasard && \
rm /etc/systemd/system/quasard.service && \
systemctl daemon-reload && \
cd $HOME && \
rm -rf .quasarnode quasar && \
rm -rf $(which quasard) \
sed -i '/QSR_/d' ~/.bash_profile
```


# Hesaplar:

[Anatolian Team](https://anatolianteam.com)

[Twitter](https://twitter.commehmetkoltigin)

[Medium](https://medium.com/@mehmetkoltigin)

[YouTube](https://www.youtube.com/channel/UCmLgaftx5e38BE0E7gpY2dA)

[Discord](https://discordapp.com/users/837933958280904737)

[Telegram](https://t.me/mehmetkoltigin)
