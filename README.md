# [Massa](https://github.com/koltigin/Massa-Turkce) Türkçe Node Kurulumu (Testnet 16)
![image](https://user-images.githubusercontent.com/102043225/191850755-6183c74f-24d3-43f3-930f-3254a28ee332.png)

##  Minimum Sistem Gereksinimleri
* 8GB RAM
* 50-100 GB SSD
* 4 vCPU

## Sistemi Güncelleme
```shell
sudo apt update && sudo apt upgrade -y
```

## Gerekli Kütüphanelerin Kurulması
```shell
sudo apt install make clang curl pkg-config libssl-dev build-essential git jq ncdu bsdmainutils -y < "/dev/null"
```

## Rust Kurulumu
Kurulum sırasında 1 yazıp default kurulumu yapıyoruz.

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
![Massa-1](https://user-images.githubusercontent.com/102043225/191853793-fbd73b8f-62c5-4405-b332-956fb069025b.JPG)

Değişkenleri yüklüyoruz.
```shell
source $HOME/.cargo/env
```

Sürümü kontrol ediyoruz.
```shell
rustc --version
```
![Massa-2](https://user-images.githubusercontent.com/102043225/191853858-fc6ec5d2-7505-4381-a642-462227f93b8c.JPG)

## Zincir Aracı Nightly İndirilmesi ve Varsayılan Olarak Ayarlanması
```shell
sudo apt install make clang pkg-config libssl-dev -y
rustup toolchain install nightly
rustup default nightly
```

## Massa Kurulumu
```shell
wget -O massa.tar.gz https://github.com/massalabs/massa/releases/download/TEST.16.0/massa_TEST.16.0_release_linux.tar.gz
tar -xzf massa.tar.gz
rm massa.tar.gz
```

## Servis Dosyası Oluşturma
* `CUZDAN_SIFRESI` yazan bölüme şifremizi giriyoruz.
```shell
echo "[Unit]
Description=Massa Node
After=network.target

[Service]
User=$USER
WorkingDirectory=$HOME/massa/massa-node/
ExecStart=$HOME/massa/massa-node/massa-node -p CUZDAN_SIFRESI
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/massa-node.service
sudo mv $HOME/massa-node.service /etc/systemd/system
sudo tee <<EOF >/dev/null /etc/systemd/journald.conf
Storage=persistent
EOF
``` 

## Servisi Başlatma ve Logları Kontrol Etme
```shell
sudo systemctl restart systemd-journald
sudo systemctl daemon-reload
sudo systemctl enable massa-node
sudo systemctl restart massa-node
``` 

## Var Olan Cüzdanı İçeri Aktarma
Daha önceki testlerde cüzdan oluşturanlar ya da halihazırda cüzdanı olanlar `wallet.dat` dosyasını ~/massa/massa-client dizini altına kopyalıyoruz.
![image](https://user-images.githubusercontent.com/102043225/191854241-3475e65b-5acc-4397-bd73-c5d1410f56a6.png)

## Yeni Cüzdan Oluşturma
```shell 
./massa-client --wallet_generate_secret_key 
```  
![Massa-3_1](https://user-images.githubusercontent.com/102043225/191856385-8f713e55-9e35-4b10-8a15-f2fb78f1ad09.jpg)

## Massa Node'u Başlatma
`CUZDAN_SIFRESI` bu bölüme şifrenizi gireceksiniz.
```shell
screen -S massa-node
cd $HOME/massa/massa-client/
RUST_BACKTRACE=full cargo run --release -- -p CUZDAN_SIFRESI |& tee logs.txt
``` 
Buradan `ctrl x d` tuşlayarak çıkıyoruz.

## Massa Client Başlatma
`CUZDAN_SIFRESI` bu bölüme şifrenizi gireceksiniz.
```shell
cd $HOME/massa/massa-client/
cargo run --release
```
Buradan da `ctrl x d` tuşlayarak çıkıyoruz.

Eğer screen içerisinde clienti kapatırsanız tekrar açmak için aşağıdaki kodu kullanabilirsiniz.
```shell
cd $HOME/massa/massa-client/
./massa-client --wallet $HOME/massa/massa-client/wallet.dat -p CUZDAN_SIFRESI
```  
Buradan da `ctrl x d` tuşlayarak çıkıyoruz.

## Discord ile Yapılacak İşlemler

1- `#⌠✅⌡testnet-rewards-registration` kanalına herhangi bir mesaj yazıyoruz ve massa bot bize özelden mesaj atıyor.
2- `#⌠💸⌡testnet-faucet` kanalına cüzdan adresimizi gönderiyoruz. (Cüzdan bilgilerinizi göremek için massa clientte `wallet_info` komutunu giriyoruz.
3- Gelen tokenler ile rolls satın alıyoruz. (`CUZDAN_ADRESI` kısmına cüzdan adresimizi giriyoruz.)
```shell
buy_rolls walletAddress 1 0
```
4- Stake için aşağıdaki kodu giriyoruz. `SecretKey` bölümüne clientte `wallet_info` kodunu yazarak cüzdan bilgilerimizden secret kodumuzu kopyalayıp yazıyoruz. 
```shell
node_add_staking_secret_keys SecretKey 
```
5- Bot'a sunucu ip adresimizi gönderiyoruz. Daha sonra bize discord id'miz ile cliente girmemiz gereken kodu aşağıdaki gibi yazıyor. Bu kodda DISCORD_ID bölümünü size bot mesaj ile göndermiş olacaktır.
```shell
node_testnet_rewards_program_ownership_proof staking_address DISCORD_ID
```
6- Yukarıdaki kodu girdikten sonra çıkan kodu bota gönderip kaydımızı tamamlıyoruz.


# Hesaplar

[Anatolian Team](https://anatolianteam.com)

[Twitter](https://twitter.commehmetkoltigin)

[Medium](https://medium.com/@mehmetkoltigin)

[YouTube](https://www.youtube.com/channel/UCmLgaftx5e38BE0E7gpY2dA)

[Discord](https://discordapp.com/users/837933958280904737)

[Telegram](https://t.me/mehmetkoltigin)
