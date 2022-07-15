# StafiHub-testnet-TR
## StafiHub Public Testnet 3
#### Herkese selam bu yazıda Stafi Protocol tarafından Cosmos Eco projeleri için Liquid Stake Türev SDK çözümü sağlamak amacıyla geliştirilen bir Cosmos SDK projesi olan StafiHub'ın stafihub-public-testnet-3 isimli son testnetine katılacağız. Testnete katılmak için herhangi bir şart gerekmiyor, daha önceki testnetlere katılmamış olanlar da katılabilir. Ayrıca Stafi Vakfı testnet sürecinde iyi performans gösteren validatörler arasından 20 mainnet validatörü seçecek detaylar: https://medium.com/stafi/stafihub-validator-program-introduction-c419e28896da 

### Sistem gerksinimleri (önerilen):
```
8GB RAM
200 GB SSD
4 vCPU
Ubuntu 20.04
```


### Root yetkisine sahip olduğunuzdan emin olun:
```
sudo su
cd /root
```
### Öncelikle makineyi güncelleyelim:
```
sudo apt-get update && sudo apt upgrade -y
```
### Gerekli olan kütüphanelerin kurulumu:
```
sudo apt install make clang pkg-config libssl-dev build-essential git jq ncdu bsdmainutils -y < "/dev/null"
```

### GO kurulumu:
```
cd $HOME
wget -O go1.18.2.linux-amd64.tar.gz https://go.dev/dl/go1.18.2.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.2.linux-amd64.tar.gz && rm go1.18.2.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bashrc
echo 'export GOPATH=$HOME/go' >> $HOME/.bashrc
echo 'export GO111MODULE=on' >> $HOME/.bashrc
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bashrc && . $HOME/.bashrc
go version
```

### StafiHub'ın github dosyasını indiriyoruz:
```
git clone --branch public-testnet-v3 https://github.com/stafihub/stafihub
```
### stafihubın kurulumu:
```
cd $HOME/stafihub
make install
```
### Moniker init işlemi (NodeName yerine kendi node isminizi girmeyi unutmayın):
```
stafihubd init NodeName --chain-id stafihub-public-testnet-3
```
### Genesis dosyasını indirin:
```
wget -O $HOME/.stafihub/config/genesis.json "https://raw.githubusercontent.com/stafihub/network/main/testnets/stafihub-public-testnet-3/genesis.json"
```
### Node çalışması için config dosyasına peer ekleme ve minumum-gas-prices ayarlanması :
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.01ufis\"/" $HOME/.stafihub/config/app.toml
sed -i '/\[grpc\]/{:a;n;/enabled/s/false/true/;Ta};/\[api\]/{:a;n;/enable/s/false/true/;Ta;}' $HOME/.stafihub/config/app.toml
PEERS="6e9d988bf9812b02c46dcec474591bd10f81916f@45.94.58.160:26656,06c57407aea673fca396b01581a2d92957d48c4a@149.102.143.60:26656,5a6d8e1904c88c9f72d35df63b15d14504aaf030@164.92.159.170:26656,5e88d0d6866cd2f386e885de6eb0a1e3bd4f45c5@38.242.237.130:26656,1eaff7a3defa35de2b29f28d4729317d783f606c@149.102.139.101:26656,724430a2cf42b94f5da6b24d4741c7418fefa24e@194.60.201.153:26656,aae1ac9ef12897d7dc8755240cbdc41ee1171a55@38.242.215.200:26656,a902c21bf157746e64c7d9f66b4f77325f181be7@34.125.209.104:26656,983c02e3caaad92c61c7524d1be477a79e900f3f@38.242.245.159:26656,d2f66fe43062f35e3bb33af2592fc050cbc6f942@149.102.128.160:26656,fcb0d867153a719492a4a92f7b32533aa95fcc8d@147.135.254.37:26656,724430a2cf42b94f5da6b24d4741c7418fefa24e@194.60.201.153:26656,5755e89abcb14b6b622fc22210b9644cddd14a0b@139.59.146.152:26656,bec8dcffade39d537f1f2d45d809d40da333ea9b@64.225.74.220:26656,c75c1a04a605c2915f19dc08d53396053a70abf5@35.228.221.69:26656,285ca83950ff336d198a036bb6195df8cb15ccb8@164.92.247.18:26656,5a6d8e1904c88c9f72d35df63b15d14504aaf030@164.92.159.170:26656,5e88d0d6866cd2f386e885de6eb0a1e3bd4f45c5@38.242.237.130:26656,1eaff7a3defa35de2b29f28d4729317d783f606c@149.102.139.101:26656,724430a2cf42b94f5da6b24d4741c7418fefa24e@194.60.201.153:26656,aae1ac9ef12897d7dc8755240cbdc41ee1171a55@38.242.215.200:26656,06c57407aea673fca396b01581a2d92957d48c4a@149.102.143.60:26656,1bc9b75496ce088daa5672ba801940d0f4adac91@143.198.49.51:26656,a16301e90288d86cac8d47dbb5b00b3b7443d0d4@178.62.211.142:26656,0b753885afa85175371298a4519fe0362f87fbb4@178.62.198.38:26656,6673282c40fc467052106610f62937236b801fcf@212.86.104.250:26656,,139988cc030b1fba1c39634de1d65781ae065d54@34.170.135.66:26656,43f03c363051b0a3b6559e9a0f413c44dcc0cad9@149.102.138.158:26656,fea697d0ab1b2c01a3b9a5505abac019b0871c46@185.229.224.105:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.stafihub/config/config.toml
```
### Screen içinde node'u başlatalım
### screen kurulumu:
```
sudo apt-get install screen
```
### stafinode isminde bir screen oluşturalım:
```
screen -S stafinode
```
### Ve node'u başlatın:
```
stafihubd start
```
### CTRL+A+D ile screenden çıkın
### Pruning Açma. (daha düşük disk kullanımı ve beraberinde daha yüksek cpu ve ram kullanımına sebep olur) (opsiyonel)
```
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.stafihub/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.stafihub/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.stafihub/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.stafihub/config/app.toml
```
### İndexer'i kapatma (daha düşük disk kullanımı) (opsiyonel)
```
indexer="null"
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.stafihub/config/config.toml
```
### Node'un sürekli çalışması için servis dosyası:
```
echo "[Unit]
Description=StaFiHub Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which stafihubd) start
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/stafihubd.service
sudo mv $HOME/stafihubd.service /etc/systemd/system
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
```
### Systemctl yüklemesini yapın:
```
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
```
### Servisi aktifleştirin:
```
sudo systemctl enable stafihubd
```
### Servisi başlatın
```
sudo systemctl restart stafihubd
```
### Servise durumunu kontol için:
```
systemctl status stafihubd
```
### Eğer service dosyasında bir hata alırsanız veya başka bir nedenle çalışmazsa `screen -r`komutu ile screen içinde node kontrol edebilirsiniz. Çalışıyor olması gerekir. Eğer node screen içinde çalışmaya devam ediyorsa service dosyasının çalışmamamsı önemli bir sorun değildir.

### Cüzdan OLuşturun (WalletName yerine cüzdanınıza bir isim verin):
```
stafihubd keys add WalletName
```
### Eğer daha önce testnete katıldıysanız ve bir cüzdanınız varsa geri yükleyebilirsiniz:
```
stafihubd keys add WalletName --recover
```
### Cüzdan oluşturduktan sonra faucet: https://discord.gg/szuCn6t9

### Sync durumunu kontrol için:
```
stafihubd status 2>&1 | jq .SyncInfo
```
#### Sync tamamlanması biraz vakit alacaktır.
#### latest_block_height: exploredaki son bloğu yakalaması gerekiyor ve catching up: true'den false'ye dönmesi gerkiyor

## Sync tamamlandıktan sonra validatör oluşturabilirsiniz

### Validatör oluşturma:
```
stafihubd tx staking create-validator \
--moniker="Monikerisminiz" \
--amount=1000000ufis \
--gas auto \
--fees=5000ufis \
--pubkey=$(stafihubd tendermint show-validator) \
--chain-id=stafihub-public-testnet-3 \
--commission-max-change-rate=0.01 \
--commission-max-rate=0.20 \
--commission-rate=0.10 \
--min-self-delegation=1 \
--from=cüzdanisminiz \
--yes
```
### moniker from ve amount kısımlarını kendinize göre düzenlediğinizden emin olun
### Ve bu komuttan sonra çıkan txhash'ı explorerda kontrol edin. success olması gerekiyor
### Explorer: https://testnet-explorer.stafihub.io/stafi-hub-testnet
[Discord](https://discord.gg/WKEdZQkw)

## Bazı kullanışlı komutlar


### Logları görüntüleyin:
```
journalctl -u stafihubd -fo cat
```

### Sync bilgisi:
```
stafihubd status 2>&1 | jq .SyncInfo
```
### Validatör bilgisi:
```
stafihubd status 2>&1 | jq .ValidatorInfo
```
### Node bilgisi:
```
stafihubd status 2>&1 | jq .NodeInfo
```
### node-id öğrenme:
```
stafihubd tendermint show-node-id
```
### Cüzdanları listeleme:
```
stafihubd keys list
```
### Cüzdandaki bakiyeyi sorgulama
```
stafihubd query bank balances cüzdanadresi
```
### Hapisten çıkma (unjail):
```
stafihubd tx slashing unjail \
  --broadcast-mode=block \
  --from=cüzdanismi \
  --chain-id=stafihub-public-testnet-3 \
  --gas=auto
```
### Delegate:
```
stafihubd tx staking delegate valoperaddress xxxxufis --from cüzdanismi --chain-id stafihub-public-testnet-3 --fees 5000ufis
```

### Sunucudan node'u silme için:
```
sudo systemctl stop stafihubd && \
sudo systemctl disable stafihubd && \
rm /etc/systemd/system/stafihubd.service && \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf .stafihub stafihub && \
rm -rf $(which stafihubd)
```
