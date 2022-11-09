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
sudo apt install screen make clang curl pkg-config libssl-dev libclang-dev build-essential git jq ncdu bsdmainutils -y < "/dev/null"
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
git clone --branch testnet https://github.com/massalabs/massa.git
```

## rustup Güncelleme
```shell
rustup default nightly 
rustup update
```

## Server IP Adresini Kaydetme
`IP_ADRESINIZ` ip adresinizi yazınız.
```shell
ipadr=IP_ADRESINIZ
echo -e "[network]\nroutable_ip = '$ipadr'" >> massa/massa-node/config/config.toml
```

## Massa Node Başlatma
`CUZDAN_SIFRENIZ` buraya cüzda şifremizi yazıyoruz.
```shell
walletpassword=CUZDAN_SIFRENIZ
screen -S massa-node -d -m bash
screen -r massa-node -X stuff "cd massa/massa-node/ && RUST_BACKTRACE=full cargo run --release -- -p $walletpassword |& tee logs.txt"$(echo -ne '\015')
```

## Massa Client Başlatma

```shell
screen -S massa-client -d -m bash
screen -r massa-client -X stuff "cd massa/massa-client/ && cargo run --release -- -p $walletpassword"$(echo -ne '\015')
```

🔴 **Dosyaların derlenmesi uzun sürebilir. Bir sonraki adıma geçmeden önce yarım saat kadarr bekleyiniz.**
🔴 **Dosya derlenmesinin bitip bitmediğine ve node'un çalışıp çalışmadığına `screen -r massa-node` ekranına girerek bakabilirsiniz. Çıkarken mutlaka `CTRL A D` tuşlayarak çıkınız.**


## Var Olan Cüzdanı İçeri Aktarma
Cüzdan işlemleri için **`screen -r massa-client`** ile ekranına giriş yapıyoruz.

Daha önceki testlerde cüzdan oluşturanlar ya da halihazırda cüzdanı olanlar **`wallet.dat`** dosyasını **`~/massa/massa-client`** dizini altına kopyalıyoruz.
![image](https://user-images.githubusercontent.com/102043225/191854241-3475e65b-5acc-4397-bd73-c5d1410f56a6.png)

Eğer **`wallet.dat`** dosyasını kaydetmediyseniz aşağıdaki kodu giriniz.
`SECRET_KEY` yazan yere cüzdan secret key kodunu giriyoruz.
```shell 
wallet_add_secret_keys SECRET_KEY
```

## Yeni Cüzdan Oluşturma
```shell 
wallet_generate_secret_key 
```
![Massa-3_1](https://user-images.githubusercontent.com/102043225/191856385-8f713e55-9e35-4b10-8a15-f2fb78f1ad09.jpg)

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
