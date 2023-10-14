

### Update & Docker kurulum ( docker kurulumu için )
```
sudo apt-get update && sudo apt install jq git && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
```
### Mina
```
sudo rm /etc/apt/sources.list.d/mina*.list
echo "deb [trusted=yes] http://packages.o1test.net/ focal rampup" | sudo tee /etc/apt/sources.list.d/mina-rampup.list
sudo apt-get update
```
```
sudo apt-get install -y mina-berkeley=2.0.0rampup5-55b7818
```



* gelen zipli dosyayı mailden pcye indirin ve açın 216_ZaGpcRDs9gGYTExUNwu.zip
* Sunucuda bir keys dosyası oluşturuyoruz ~/keys .
```
mkdir ~/keys
```
*	dosyanın yetkisini düzenliyoruz
```
chmod 700 ~/keys
```
* Nano ile my-wallet içini düzenliyoruz
```
nano ~/keys/my-wallet
```
* community-216-key içindekileri kopyalıyoruz. indirdiğiniz dosyada metin belgesiyle falan açın
*	ctrl x +y ile kaydedelim
*	wallet dosyasının yetkisini düzenliyelim
```
chmod 600 ~/keys/my-wallet
```
*	 my-wallet.pub dosyasını nano ile düzenle deyip yine pcmize indirip açtığımız dosyadaki community-216.pub içindekileri buraya kopyalıyoruz ctrl x y ile kaydediyoruz
```
nano ~/keys/my-wallet.pub
```
### Çalıştırma

```
mina daemon --peer-list-url https://storage.googleapis.com/seed-lists/testworld-2-0_seeds.txt \
            --block-producer-key ~/keys/my-wallet
```

--------------------

### DOCKER
```
docker pull gcr.io/o1labs-192920/mina-daemon:2.0.0rampup5-55b7818-focal-berkeley
```
```
cd ~
chmod 700 /root/keys
chmod 600 /root/keys/my-wallet
mkdir /root/.mina-config
nano /root/.mina-env
```
not: aşağıdakini içerisine kopyalayın ctrl x y enterla kaydedin
---------------
export MINA_PRIVKEY_PASS="şifre"
LOG_LEVEL=Info
FILE_LOG_LEVEL=Debug
EXTRA_FLAGS=" --block-producer-key /root/keys/my-wallet"
PEER_LIST_URL=https://storage.googleapis.com/seed-lists/testworld-2-0_seeds.txt
----------
```
cd
```
```
docker run --name mina -d \
-p 8302:8302 \
--restart=always \
--mount "type=bind,source=$(pwd)/.mina-env,dst=/entrypoint.d/mina-env,readonly" \
--mount "type=bind,source=$(pwd)/keys,dst=/keys,readonly" \
--mount "type=bind,source=$(pwd)/.mina-config,dst=/root/.mina-config" \
minaprotocol/mina-daemon:2.0.0rampup5-55b7818-focal-berkeley \
daemon
```


```
docker logs -f mina
```
```
docker exec -it mina mina client status
```








