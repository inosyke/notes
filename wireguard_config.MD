# Додавання користувача wireguard

Створюємо ключі для нового користувача

В окремій папці генеруємо ключі

```shell
wg genkey | tee <користувач1>_privatekey | wg pubkey | tee <користувач1>_publickey
```

Додаємо конфігурацію користувача на сервері в файл wg0.conf 
(зазвичай знаходить за шляхом `/etc/wireguard`)

```shell
[Peer]
PublicKey = <віміст_файлу_користувач1_publickey>
AllowedIPs = <IP_в_віртуальній_локальній_мережі_клієнта>
```

Перезагружаємо wireguard сервер

```shell
systemctl restart wg-quick@wg0 && systemctl status wg-quick@wg0
```

## Створюємо конфіг для клієнта client.conf

```shell
[Interface]
PrivateKey = <віміст_файлу_користувач1_privatekey>
Address = 10.0.0.2/32
DNS = 8.8.8.8

[Peer]
PublicKey = <SERVER-PUBKEY>
Endpoint = <SERVER-IP>:51830
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 20
```

Тепер скачуємо [wireguard клієнт](https://www.wireguard.com/install/) та підтягуємо конфіг.


Або ж для cli кидаємо конфіг у `sudo nano /etc/wireguard/wg0.conf`

Підняти тунель ```sudo wg-quick up wg0```, погасити: ```sudo wg-quick down wg0```
