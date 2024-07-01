### 🐅 Dosyaları çekelim
```console
cd $HOME
rm -rf slinky
git clone https://github.com/skip-mev/slinky.git
cd slinky
git checkout v0.4.3
```
```console
make build
```
```console
mv build/slinky /usr/local/bin/
```
### 🐅 Servis olusturalım
```console
sudo tee /etc/systemd/system/slinkyd.service > /dev/null <<EOF
[Unit]
Description=Initia Slinky Oracle
After=network-online.target

[Service]
User=$USER
ExecStart=$(which slinky) --oracle-config-path $HOME/slinky/config/core/oracle.json --market-map-endpoint 127.0.0.1:15090
Restart=on-failure
RestartSec=30
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
### 🐅vBaşlatalım
```console
sudo systemctl daemon-reload
```
```console
sudo systemctl enable slinkyd.service
```
```console
sudo systemctl restart slinkyd.service
```
### 🐅vLoglara bakmak isterseniz
```console
journalctl -fu slinkyd --no-hostname
```
### 🐅 Yapılandıralım
```console
nano /root/.initia/config/app.toml
```
* 🐅vNot: sayfanın sonuna ok tuşları ile gidin. yada ctrl w basarak arama yerine oracle yazın. enterlayın gider. 1ci resimdeki gibi olanı 2 ci resimdeki gibi düzenleyin.
```console
enabled = "true"
```
```console
oracle_address = "127.0.0.1:8080"
```
```console
client_timeout = "500ms"
```

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/7c3c9f54-dcb3-42c7-bd5a-0c5b81fb85b4)

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/e767f310-efde-4c19-955f-8d2120a918a7)

* 🐅 Not: işlemleri tamamlayınca initiaya restart atmamız gerek
```console
sudo systemctl daemon-reload
sudo systemctl restart initiad
sudo systemctl restart slinkyd.service
sudo journalctl -u initiad.service -f --no-hostname -o cat
```
