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
### 🐅 Servis oluşturalım
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
### 🐅 Başlatalım
```console
sudo systemctl daemon-reload
```
```console
sudo systemctl enable slinkyd.service
```
```console
sudo systemctl restart slinkyd.service
```
### 🐅 Loglara bakmak isterseniz
```console
journalctl -fu slinkyd --no-hostname
```
### 🐅 Yapılandıralım
```console
nano /root/.initia/config/app.toml
```
* Sayfanın sonuna ok tuşları ile gidelim ya da ctrl+w yapıp arama yerine oracle yazıp enterlayalım. 1.ci fotoğraftaki bilgileri 2.ci fotoğraftakiler ile değiştirelim
```console
enabled = "true"
```
```console
oracle_address = "127.0.0.1:8080"
```
```console
client_timeout = "500ms"
```

![image](https://github.com/kaplanbitcoin1/Initia-NODE/assets/98455323/6562a9e1-2da8-436a-848c-f2eb644ab2df)

![image](https://github.com/kaplanbitcoin1/Initia-NODE/assets/98455323/b23eec78-6257-4a49-8c88-9e44f1db41da)

* İşlemleri tamamlayınca Initia'ya restart atmamız gerekiyor
```console
sudo systemctl daemon-reload
sudo systemctl restart initiad
sudo systemctl restart slinkyd.service
sudo journalctl -u initiad.service -f --no-hostname -o cat
```



