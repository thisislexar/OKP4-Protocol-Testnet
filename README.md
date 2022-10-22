<h1 align="center">OKP4 Testneti Kurulum Rehberi

## Merhabalar, bugün OKP4 testnetine katılıyor olacağız. Sağ üstten yıldızlayıp forklamayı unutmayalım. Sorularınız olursa: [LossNode Chat](https://t.me/LossNode)

![image](https://user-images.githubusercontent.com/101462877/196537883-d142a287-c206-46d3-8ffa-8fb766414ddb.png)

## Sistem gereksinimleri:
NODE TİPİ | CPU     | RAM      | SSD     |
| ------------- | ------------- | ------------- | -------- |
| Testnet | 4          | 8         | 200  |

## OKP4 için önemli linkler:
- [Website](https://okp4.network/)
- [Explorer](https://explore.okp4.network/okp4%20testnet/)
- [Twitter](https://twitter.com/OKP4_Protocol/)
- [Discord](https://discord.gg/wPtHgWeRUv)

# 1a-1) Snapshot'lı script ile kurulum.

```
wget -O okp4.sh https://raw.githubusercontent.com/thisislexar/OKP4-Protocol-Testnet/main/okp4.sh && chmod +x okp4.sh && ./okp4.sh
```

# 1a-2) Snapshot'sız script ile kurulum.

```
wget -O okp4-noSS.sh https://raw.githubusercontent.com/thisislexar/OKP4-Protocol-Testnet/main/okp4-noSS.sh && chmod +x okp4-noSS.sh && ./okp4-noSS.sh
```

# 1b) Manuel kurulum.

Node bilginizi geliştirmek adına dilerseniz [Manuel Kurulum](https://github.com/thisislexar/OKP4-Protocol-Testnet/blob/main/okp4_manual.md) da yapabilirsiniz.


# 2) Devam edelim. 

## Sync durumunu kontrol etmek için loglara bakalım, [explorer](https://explore.okp4.network/okp4%20testnet/)'da son blok ile eşitse devam edelim:

```
sudo journalctl -u okp4d -f -o cat
``` 

## Cüzdan oluşturalım.
```
okp4d keys add <CÜZDANADI>
``` 
Var olan bir cüzdanı kullanmak isterseniz:

```
okp4d keys add <CÜZDANADI> --recover
``` 

## Validator oluşturmak için [faucet](https://faucet.okp4.network/) alalım.

![image](https://user-images.githubusercontent.com/101462877/196545209-a66c229d-c28e-46c6-8184-c7786bc8432e.png)


## [Explorer](https://explore.okp4.network/okp4%20testnet/)'dan cüzdanımıza token gelip geldiğini kontrol edelim.

![image](https://user-images.githubusercontent.com/101462877/196545734-076fd76e-5cb3-4fc9-8799-488e7cf1999e.png)

## Explorer'da cüzdan arattığınızda  aşağıdaki gibi görünüyorsa, adresinizi alttaki url'nin sonuna ekleyerek aratın.


![image](https://user-images.githubusercontent.com/101462877/196958202-44c9f352-ab5f-444f-acdd-7af5dcb8c382.png)

```
https://explore.okp4.network/okp4%20testnet/account/okp41my2k20ll3pd7xasdrxyxxxxxxxxxx
```

![image](https://user-images.githubusercontent.com/101462877/196958431-092fa974-78a1-4ca9-8b3f-258cc9719588.png)



## Validator oluşturalım.


```
okp4d tx staking create-validator \
  --amount 1000000uknow \
  --from <CÜZDANADI> \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(okp4d tendermint show-validator) \
  --moniker $NODENAME \
  --chain-id okp4-nemeton \
  --website="http://linktr.ee/LossNode" \
  --details="Testing the OKP4"
```


# Bazı komutlar:

Log kontrolü

```
journalctl -fu okp4d -o cat
```


Servisi durdurma

```
sudo systemctl stop okp4d
```

Servisi tekrar başlatma

```
sudo systemctl restart okp4d
```

Token delege etme

```
okp4d tx staking delegate okp4valoper1my2k20ll3pd7xasdrxyxw7hrnxxxxxx 1000000uknow --chain-id=okp4-nemeton  --from <CÜZDANADI>
```

Validator düzenleme

```
okp4d tx staking edit-validator \
  --moniker=$NODENAME \
  --identity="<KEYBASE ID'NİZ>" \
  --website="<WEBSİTE LİNKİ>" \
  --details="AÇIKLAMA" \
  --chain-id=okp4-nemeton  \
  --from=<CÜZDANADI>
``` 


# Node silmek için:

```
sudo systemctl stop okp4d
sudo systemctl disable okp4d
sudo rm /etc/systemd/system/okp4* -rf
sudo rm $(which okp4d) -rf
sudo rm $HOME/.okp4* -rf
sudo rm $HOME/okp4 -rf
sed -i '/OKP4_/d' ~/.bash_profile
``` 
