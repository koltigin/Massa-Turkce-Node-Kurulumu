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

## Config.toml Dosyasını Düzenleme
Buradaki işlemleri terminalden ya da isterseniz dosyayı indirerek dosya üzerinden yapabilirsiniz. Aşağıda terminalden düzenleme anlatılmıştır.
```shell 
nano $HOME/massa/massa-node/base_config/config.toml
```  

Açılan dosyada `[bootstrap]` kısmındaki kodları aşağıdakilerle değiştiriyoruz.
```shell 
[bootstrap]
    # list of bootstrap (ip, node id)
    bootstrap_list = [
	["149.202.86.103:31245", "P12UbyLJDS7zimGWf3LTHe8hYY67RdLke1iDRZqJbQQLHQSKPW8j"],
        ["149.202.89.125:31245", "P12vxrYTQzS5TRzxLfFNYxn6PyEsphKWkdqx2mVfEuvJ9sPF43uq"],
        ["158.69.120.215:31245", "P12rPDBmpnpnbECeAKDjbmeR19dYjAUwyLzsa8wmYJnkXLCNF28E"],
        ["158.69.23.120:31245", "P1XxexKa3XNzvmakNmPawqFrE9Z2NFhfq1AhvV1Qx4zXq5p1Bp9"],
        ["198.27.74.5:31245", "P1qxuqNnx9kyAMYxUfsYiv2gQd5viiBX126SzzexEdbbWd2vQKu"],
        ["198.27.74.52:31245", "P1hdgsVsd4zkNp8cF1rdqqG6JPRQasAmx12QgJaJHBHFU1fRHEH"],
        ["54.36.174.177:31245", "P1gEdBVEbRFbBxBtrjcTDDK9JPbJFDay27uiJRE3vmbFAFDKNh7"],
        ["51.75.60.228:31245", "P13Ykon8Zo73PTKMruLViMMtE2rEG646JQ4sCcee2DnopmVM3P5"],
  	["45.88.223.222:31245", "P12Au4nMFoRqsaD18qwrn8sYFhraL7ZAT7BbTuev3CnRpPmFp2uR"],
  	["144.24.192.28:31245", "P12DrR3pYRboEYS8tShJgnqmzUUPR65V7V5cxi8QTzA5EZ9kEXCC"],
  	["193.201.15.197:31245", "P12EhtVvSYapW7oUjqPYX2pbDDxNLpzuoTZbjTjAHLBLHAVwyYdT"],
  	["65.21.188.134:31245", "P12JQBExAH1Pa4REEGZwUUEhhN2z9fLMDqP3oVoWPKtZTVdftSZU"],
  	["194.242.57.186:31245", "P12JUjFKAyemCRJoKL5NG9a8rajA8G9Xg3kMKG4nnwyiT6HQBzsb"],
  	["85.237.32.204:31245", "P12QzKY4SyfSeueNdHRECbrdX3cULpznDy2nVGArHfqKpebREMmj"],
  	["185.132.42.25:31245", "P12SJrecaYXNhJYmgACJtA1oqf4NGrQTEgbH8KzanytgfxBAjCKE"],
  	["185.208.206.250:31245", "P12ZjyyWwQo6czrPZfd89Lz9eCT9LZE7WjWgqnmAHJJUedR6WPPx"],
  	["83.202.151.15:31245", "P12as6Aszw4sbNkMwF55dpeBDK1CsRRoya6U2bwWkNfZEHT8uZDZ"],
  	["87.255.10.66:31245", "P12chg21n9EuHaPVZTdatDCgfLz2xM88GtgGQfjMtgrjEgPATqBV"],
  	["185.211.5.228:31245", "P12fFgVJ2Hn7PXjHh6HxbwP7NL1wnjTB2qBf5UgnSGmRH1DXvngY"],
  	["149.154.66.132:31245", "P12gm9WXGYsQQqtXFjZ3aSo2zNdhmHZN4YAvgW4nYsdW5cqm1AgH"],
  	["167.86.82.241:31245", "P12p21SMUKyneuW9Gn92jwXu3mRhX4S1Fy9N6gmqrJdt2179ZaLP"],
  	["157.90.25.139:31245", "P18mUKe4ctUKEShLE84GExXJWrH7rTDBFHdicMedYZyz8G8DDom"],
  	["65.108.225.5:31245", "P1DDmtWYm8YLiinrmLxcKzDXf883GqPHkQ9uugTCRWyJMAD6MxE"],
  	["45.159.251.104:31245", "P1Kd63qufSkMME2cAinHonY8s7ETVZGygA1Uj59mn5sbyibKyMj"],
  	["135.181.101.199:31245", "P1SHWd8U4hj4ev352ccrnDsrmw611Rpo2px1nCivh5feFmmHEor"],
  	["91.65.70.74:31245", "P1gpCEsPvPgnxGNCfcHaYRdgvfR6mKu3j1oz35jndzfUYxfcgBN"],
	["161.97.139.175:31245", "P1oSA94hw95FkDr19V7GXVYZH5VqYRXCVo4jZ3TnsMcHuXqVtva"],
  	["65.21.193.247:31245", "P1ohsrMLJBmcn8y3GydhfmZhNsJWfegT8VQ7Bi7j1SH2b6oUKd5"],
  	["38.242.236.198:31245", "P1r95DeMifWQs6Ku322QSqP6dXTcFD9VvyEMWE5n4HZE49fmQCw"],
  	["78.107.234.44:31245", "P1tBD958SEeRN9tL3BaNNf8s9L7dimwM9fuGH25nihrUhkMrQ72"],
  	["185.250.241.37:31245", "P1vMf7iWUmxXhJzYBzURhPKYrrUjF6VJR4C8qjSTwCnJdSjFoAr"],
  	["75.119.152.192:31245", "P1vYbJXsroSTLSf1kKHqkLu2N2wB8eT5dkGw4JRcazsarQQDkbn"],
  	["94.130.54.253:31245", "P1vranNFHuif17n31JDuBDmxP6V1CtkqDE7r3mp5GQNUZVA7HE"],
	["95.216.2.232:31245", "P12icw9DcUD6rBixYwbg44dSgFDYVQM3r8Lv4f9LvcmE9S2JDmkL"],
	["38.242.128.175:31245", "P124o9ouf2EUDXDrGsECKr2WSxmAS1d9CcpgUEVNun6s9hfT4oiU"],
	["149.102.157.187:31245", "P12mc4TdXRiYdWCsYFQwym6MCg94ENM3zYEdK21rPEg1cLhumvDv"],
	["77.232.42.168:31245", "P12f4xJ79rU7v5wEPQhFJgeCoRr54HbBYuEPiuHSEgWHqEKi9Hcm"],
	["65.21.191.82:31245", "P1xDpopcoA6zofpkWNDNtkPePQjqLy3iAmP6NrNKoA4y8eggU5c"],
	["94.130.170.203:31245", "P1yPTSmu58NRYKVshe4Jn61bhyjiXeyarayJADrV7X34YnW8Aep"],
	["161.35.194.212:31245", "P1Pzgepr2EEL1mk7ruL2vZtZ3JiUV1tYBmyHz6a3eMV2WdtyYuX"],
	["178.206.234.10:31245", "P12ERUpkNxjkroH9zF2n2tF4Dj2QV9X6DH4RePFFP2g1FvE9HUqW"],
	["80.87.202.42:31245", "P16D8K1QBcDTr2h4C7qQ3ap2LUkkqZWa1jCdA9zuFp8MsZt69rj"],
	["184.163.42.223:31245", "P1Ewsp1HYoU8gAeAYs7zP2Hm9ak2RzwPsTQXK9uAdBV7D8AYFd8"],
	["94.181.45.219:31245", "P12jgnHCZEfhHbi8ZTgLo4FCbFGxCEt4KfpMYwmRvjyYAukv45o2"],
	["147.182.191.215:31245", "P12k716iL58GKndcZU2G4CyFtJszzqTJ2YPRTG3MzX24T1XrVsy8"],
	["162.55.184.48:31245", "P1PnLYRE7wEK7t7Kuu2v4DDMvpnRtj5h5mF5QPKEv2NPQ8sSHCJ"],
	["65.108.234.11:31245", "P1gPQzvRjMHyXaVfeWMQPRDfxqCVuXHwtWPYfwLbEoGLHdL3GPR"],
	["157.90.172.210:31245", "P122iHawSXefCMCv1PN8VanyM1DQYoEKDo8j5iUCebMbr7DAw55y"],
	["78.47.224.176:31245", "P12TJ1u5GsfUb2wSfN7843E3zHKVwqKzzqU6Z7y9xgv7gUADae4x"],
	["54.36.174.177:31245", "P1gEdBVEbRFbBxBtrjcTDDK9JPbJFDay27uiJRE3vmbFAFDKNh7"],
	["154.26.132.217:31245", "P1gAKYH55UJHyv5V8vePSdLwUBVGtimwTn6tvF29K8GmPAjHWkK"],
	["82.223.10.164:31245", "P12mqWgZdS3qYQcgYXD4PPtL128U7bfTSSsZRWHn4mQCcaYc93y3"],
	["207.180.221.86:31245", "P1HEGx8JMUjTKiPNVf6KPsCa3cvrV3Bfh61CJXwqKRUeyhBDADd"],
	["95.216.148.136:31245", "P1SDGHkgMKbzceHMZitrAsySJiwpYnkERErXaMGW6HvLuNCfzFf"],
	["65.108.221.203:31245", "P12WuMCLswtzJWNaVmveXsf6Zf5jnNBx9r4Bq69rW2Ey91Lzpt9G"],
	["158.69.120.215:31245", "P12rPDBmpnpnbECeAKDjbmeR19dYjAUwyLzsa8wmYJnkXLCNF28E"],
	["83.202.151.15:31245", "P12as6Aszw4sbNkMwF55dpeBDK1CsRRoya6U2bwWkNfZEHT8uZDZ"],
	["78.107.234.44:31245", "P1tBD958SEeRN9tL3BaNNf8s9L7dimwM9fuGH25nihrUhkMrQ72"],
	["193.201.15.244:31245", "P12mK3XtaDovD386EKMFgUPSdV1nndycgwBc36Fj3jA8tsHSpqoN"],
	["207.180.215.214:31245", "P1ZoC3y6qyj3c5H6ToDFcR8AcZY5YJgb1y1HLHMUaZVzHdGNg3Y"],
	["194.187.110.19:31245", "P1tK6BYtxrcRqDr693WUSHeZRuwRQzLxiuSLmZGXGGqLXVo777P"],
	["88.198.215.110:31245", "P1uvS2kmmEhRNbwQDVZtfmbXNqHYfJ75FdAkgoA1MzDkQ9WAW2D"],
	["154.26.132.182:31245", "P137JJeU7dt6MbTX87rB9s9geX3gYRUmMmZFMuoo8Hru5pzKRcm"],
	["82.64.84.25:31245", "P1gvaKFbbm9o9Cq9P7AJeeqnpWZ1wqvAoXp21nfPvvrhgPhkcai"],
	["157.90.172.210:31245", "P122iHawSXefCMCv1PN8VanyM1DQYoEKDo8j5iUCebMbr7DAw55y"],
	["88.201.196.152:31245", "P1bUFyZYvwhgy7D61pHZTYXZYjJHEqLLoxP5Eq9kQ1CE19KuvoU"],
	["185.196.20.94:31245", "P127MgAxqXEVQMXKYWGWBr157rK4TNJuLMK9oeJNDXDdTh33YWoB"],
	["185.146.51.224:31245", "P1jvF4Kgr1M17BRYJRHB2L4W3BXKP5xwKcmZjpL8uXwzAWG61SH"],
	["185.205.244.120:31245", "P122Y3ET5uKaUEtf1yBamiSVba4eqco2wCREe6GJzUjNCGeGxi4D"],
	["91.203.81.148:31245", "P1MvApgRYuJdvAV2UcmFV3h8WertuoncrujjjsahtGTj4Pu574R"],
	["142.132.199.236:31245", "P1ui2mwRJw4QDRXLEaSvMXkR52gxHvpbTh5NiyBHqWXn5kZfwH5"],
	["154.26.132.181:31245", "P12BVQeGQHZNsz1RC3W3QMteRBAfsvovep5oLZDPaf4xuJMgNZwU"],
	["62.183.96.174:31245", "P12w5eRUaNfzcRYCx51Dj7dsHre6kR5p65RZs5tA667iAJ5PKRMZ"],
	["167.114.145.158:31245", "P12Kkrj1ReYtTzapYC2fFzZStUSPmAz7qo4gXf3RC18nnFofdYko"],
	["161.97.141.80:31245", "P12jpADZRE2jiXF29X9fzULiRa9YfjmrHqTVoXFMTQh3brEQvUXJ"],
	["51.38.194.32:31245", "P12BFNZSKZJznpPGNs7eMpziuKxzRUHMzfrKeC7d91w9UoPaa7Et"],
	["95.216.148.136:31245", "P1SDGHkgMKbzceHMZitrAsySJiwpYnkERErXaMGW6HvLuNCfzFf"],
	["194.163.160.246:31245", "P124ejBRB6VGNWE9JdF6qspom6xeZb94yavfnkuhWZCJajqm7DNp"],
	["80.11.168.199:31245", "P12DLs1C7YKBmLWa2rbtbapabsRxmK7FrfYstzm5ERQrG1nNKu6a"],
	["185.209.230.60:31245", "P12MzbJDwYuW9HNCT5CtDvEaSCUJrLfjMCPj7EFkSA7Kc35J922m"],
	["194.187.110.19:31245", "P1tK6BYtxrcRqDr693WUSHeZRuwRQzLxiuSLmZGXGGqLXVo777P"],
	["185.216.75.176:31245", "P12fTqygwvpKu7g9ToQizSPbWcDTaePqCrsQwHdpqbqvKP5Sp62s"],
	["138.201.139.207:31245", "P1dwY5JtfdNMVrsbkdrcWu3GN4VDDzqpPsXnURae9ikiuLreBUq"],
	["38.242.142.247:31245", "P1Wc51WpLE6ej45mgKw3XZGkWb2U4aH3pbiUvz8iqhZ87g48rem"],
	["91.65.70.74:31245", "P1gpCEsPvPgnxGNCfcHaYRdgvfR6mKu3j1oz35jndzfUYxfcgBN"],
	["167.86.71.232:31245", "P12J9bpRyKB4HACJVXX2s2Crjx2ESwVnnPj5znR8e19C3BYvs7TF"],
	["149.102.157.65:31245", "P17s6DRBRPhsrM9dYao4MhdgYZAdj74BaxoRG8QpZ7J36dP8m3p"],
	["193.201.15.244:31245", "P12mK3XtaDovD386EKMFgUPSdV1nndycgwBc36Fj3jA8tsHSpqoN"],
	["207.180.242.141:31245", "P12fErdTX9aiwNMbk2TG4jbKKovLHrWRwD63wJKnXksJjLJxYcoz"],
	["135.181.111.221:31245", "P12m1G3oKLc8coj5txeB2URtFFDNnMqRz6nXAaGnYGbhtVwb8j2N"],
	["95.216.198.216:31245", "P15CydY6WPVTHkdMjLovvRRWx4W4vpjzWfh7oSTUVwFprwoeB4K"],
	["176.163.175.28:31245", "P1Lc1GcYdhrKNuT8MCte81dCior4mGJSNgbU9ytz4bqAG9RPju6"],
	["46.4.71.222:31245", "P1UTDZRssv2x9ikrHrZ9YD6atE68EPLEjenSTsVKLeqwMUVZP4V"],
	["185.69.52.194:31245", "P19pFgS9c9wAoM59jo97TTeChGD7zDgE7mMmPPvJp9HX2VocUsd"],
	["164.68.114.72:31245", "P12utqQrCLQArHD8sNzNXpWwkig8S9xKAWUiNZ2E5pA6ArkdgJWi"],
	["88.201.196.152:31245", "P1bUFyZYvwhgy7D61pHZTYXZYjJHEqLLoxP5Eq9kQ1CE19KuvoU"],
	["92.94.69.70:31245", "P12srgSqw9zwVZq8c7iixN9ZP782xW7o8x91tr9BUaaURSswQBrd"],
	["89.58.37.5:31245", "P1mpPVbrMPJgEPLhS3oKai7D72UTqQ68nyLAwbBvkK3hTwiNNvX"],
	["95.217.166.220:31245", "P12C2VmSSJVBPEkNDzCx7XfNncFpo57GoDX2QhkgPMbNiyRsmH4u"],
	["45.88.106.199:31245", "P12Yr9DLJwfp5wF7DvrYHnAmfQmLCgv3E1zHTcWbbCwkP74mrv2w"],
	["144.76.194.76:31245", "P1KxtZTuzH9eXsP7D8M3whqPazy6NGjvqQR1zqKqBSExMUT1Uyg"],
	["5.255.104.42:31245", "P14ejVgWxjzLwfh7W2Gq2fiqGUFqwqaywHiqxuGYiBhn6RuPZ3M"],
	["176.124.202.60:31245", "P12gXCtzyaJcKLhsofvQQavV6igB2eoSFCYLgo1x1u1WFxk9ooLv"],
	["45.8.145.244:31245", "P121KYQYAdo5hheM6uyL3ioVV7hMooRo36WpadgVkXMjshBZqqSi"]
    ]
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
