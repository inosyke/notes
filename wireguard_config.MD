# Налаштування wireguard на сервері

Створюємо користувача та ключі для нього

В окремій папці генеруємо ключі

```shell
wg genkey | tee <користувач1>_privatekey | wg pubkey | tee <користувач1>_publickey
```

Додаємо конфігурацію користувача на сервері в файл wg0.conf

```shell
[Peer]
PublicKey = <віміст_файлу_користувач1_publickey>
AllowedIPs = <IP_в_віртуальній_локальній_мережі_клієнта>
```

Перезагружаємо wireguard сервер

```shell
systemctl restart wg-quick@wg0 && systemctl status wg-quick@wg0
```

Тепер скачуємо [wireguard клієнт](https://www.wireguard.com/install/)


Створюємо конфіг для клієнта client.conf

```shell
[Interface]
PrivateKey = <віміст_файлу_користувач1_publickey>
Address = 10.0.0.2/32
DNS = 8.8.8.8

[Peer]
PublicKey = <SERVER-PUBKEY>
Endpoint = <SERVER-IP>:51830
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 20
```
