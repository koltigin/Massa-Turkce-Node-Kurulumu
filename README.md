# [Massa](https://github.com/koltigin/Massa-Turkce) Türkçe Node Kurulumu (Testnet 14)
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
source $HOME/.cargo/env
```
![Massa-1](https://user-images.githubusercontent.com/102043225/191853793-fbd73b8f-62c5-4405-b332-956fb069025b.JPG)

Sürümü kontrol ediyoruz.
```shell
rustc --version
```
![Massa-2](https://user-images.githubusercontent.com/102043225/191853858-fc6ec5d2-7505-4381-a642-462227f93b8c.JPG)

## Zincir Aracı Nightly İndirilmesi ve Varsayılan Olarak Ayarlanması
```shell
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

## Config.toml Dosyasını Düzenleme
Buradaki işlemleri terminalden ya da isterseniz dosyayı indirerek dosya üzerinden yapabilirsiniz. Aşağıda terminalden düzenleme anlatılmıştır.
```shell 
nano $HOME/massa/massa-node/base_config/config.toml
```  

Açılan dosyada `[bootstrap]` kısmındaki kodları aşağıdakilerle değiştiriyoruz.
```shell 
[bootstrap]
bootstrap_list = [["159.223.108.41:31245", "P15ukCyNQyZsXx1TUeXcAiecuFNKFfKQFnQndsCYVFpFydubCba"],
	["198.27.74.52:31245", "P1hdgsVsd4zkNp8cF1rdqqG6JPRQasAmx12QgJaJHBHFU1fRHEH"],
	["54.36.174.177:31245", "P1gEdBVEbRFbBxBtrjcTDDK9JPbJFDay27uiJRE3vmbFAFDKNh7"],
	["157.90.151.33:31245", "P1R29hgnheGSLWes54bJNvDrH95rpGJXuHszfYrtSSyQVGG99i8"],
	["49.12.240.51:31245", "P1UEJ4sYfcK6GsbFDb9iyA6FujCUruX5QtYPPBooNy8Uh7DyDVi"],
	["149.202.89.125:31245", "P12vxrYTQzS5TRzxLfFNYxn6PyEsphKWkdqx2mVfEuvJ9sPF43uq"],
	["176.65.61.156:31245", "P1MjCotR68EafSLn1F9zfPu3kRvGzoEBRu9t1r5UUPbfSsLbsaw"],
	["78.107.234.44:31245", "P1tBD958SEeRN9tL3BaNNf8s9L7dimwM9fuGH25nihrUhkMrQ72"],
	["185.248.24.34:31245", "P13p69gyuWHZaSnJAu4TwSBW1NwSqYDZFMCc2GX2KYb5XrjpDjj"],
	["5.189.160.72:31245", "P1NRSwmrQkdKQaBuWHbNrVFwgUZAr9qmmtq4gAefzKJvFQ8aANs"],
	["149.202.86.103:31245", "P12UbyLJDS7zimGWf3LTHe8hYY67RdLke1iDRZqJbQQLHQSKPW8j"],
	["51.75.60.228:31245", "P13Ykon8Zo73PTKMruLViMMtE2rEG646JQ4sCcee2DnopmVM3P5"],
	["198.27.74.5:31245", "P1qxuqNnx9kyAMYxUfsYiv2gQd5viiBX126SzzexEdbbWd2vQKu"],
	["120.48.78.195:31245", "P1SB9tLWXtGnJjKkajmtWmR7N6K5F9Ko8N8ypGKHc3rEmg4BkSk"],
	["65.108.215.110:31245", "P15sRpmAgqjHpnMFPa317xxPDxezU8KMggz6US3JtrNi6GxL6Jx"],
	["158.69.23.120:31245", "P1XxexKa3XNzvmakNmPawqFrE9Z2NFhfq1AhvV1Qx4zXq5p1Bp9"],
	["95.142.47.133:31245", "P1VFa2VhGs4PUdF5N3SZjCLJyYpG1WSoNjTRR3nYtbQrLo8sqY3"],
	["141.94.218.103:31245", "P1VRyXjUaHeJd4Rmr3waVmpZDFzzH5ARRi3f5ye5BYgxBmxHC7X"],
	["80.234.37.83:31245", "P1BqrZD1YaH7BzFcwYC7PU6Urkq5RxANdVTtDiYPqX6AnqUCADB"],
	["137.74.199.24:31245", "P1EbEKmr3MFScrFxAWgptz3ty4obBQLFdJxG6PUUvZMuufhN8JQ"],
	["158.69.120.215:31245", "P12rPDBmpnpnbECeAKDjbmeR19dYjAUwyLzsa8wmYJnkXLCNF28E"],
	["46.101.12.241:31245", "P1f3GJjeXpjBVHUZZ4rZDBkTBYswuJgvyGkb7mCuh5UjcjK8Vro"],
	["157.90.249.192:31245", "P1k5P4veWQ95wNKfnfU6551h8QZRq5eC1NG7U6UTVpKAPeXj7Vg"],
	["38.242.216.60:31245", "P1HU9j2J38KLXFQDqCvGJSpAoRUa5YJoKLaREdkfUuDkhXikUAS"]]
```shell 

## Servisi Başlatma
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

## Massa Client Başlatma
`CUZDAN_SIFRESI` bu bölüme şifrenizi gireceksiniz.
```shell
cd $HOME/massa/massa-client/
./massa-client --wallet $HOME/massa/massa-client/wallet.dat -p CUZDAN_SIFRESI
```  

## Discord ile Yapılacak İşlemler

1- `#⌠✅⌡testnet-rewards-registration` kanalına herhangi bir mesaj yazıyoruz ve massa bot bize özelden mesaj atıyor.
2- `#⌠💸⌡testnet-faucet` kanalına cüzdan adresimizi gönderiyoruz. (Cüzdan bilgilerinizi göremek için massa clientte `wallet_info` komutunu giriyoruz.
3- Gelen tokenler ile rolls satın alıyoruz. (`CUZDAN_ADRESI` kısmına cüzdan adresimizi giriyoruz.)
```shell
buy_rolls walletAddress 1 0
```
4- Stake için aşağıdaki kodu giriyoruz. `SecretKey` bölümüne clientte `wallet_info` kodunu cüzdan bilgilerimizden secret kodumuzu kopyalayıp yazıyoruz. 
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
