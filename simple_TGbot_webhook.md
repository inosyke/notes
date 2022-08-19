# Компіляція різноманітних туторіалів для запуску простого бота з вебхуком

Встановлюємо ssl сертефікати
```bash
pip3 install certbot
sudo certbot
```
Додаємо дозвіл на читання сертифікатів
```bash
groupadd letsencrypt
usermod -a -G letsencrypt userforbot
chgrp -R letsencrypt /etc/letsencrypt/
chown -R userforbot:letsencrypt /etc/letsencrypt/
```
Встановлюємо необхідні бібліотеки
```bash
pip install aiohttp
pip install cchardet
pip install aiodns
pip install pyTelegramBotAPI
pip3 install --upgrade pyTelegramBotAPI
```

Реєструємо свого бота https://t.me/botfather/ та надсилаємо про нього інформацію:
`https://api.telegram.org/botYOUR-TOKEN/setWebhook?url=https://YOUR_DOMAIN:8443/YOUR-TOKEN/`

Відкриваємо порт для боту `sudo ufw allow 8443`

Файл `config.py`
```python
token = 'YOUR_TOKEN'
```

Файл `main.py`
```python
import config
import telebot
from aiohttp import web
import ssl

WEBHOOK_LISTEN = "0.0.0.0"
WEBHOOK_PORT = 8443

WEBHOOK_SSL_CERT = "/etc/letsencrypt/live/YOUR_DOMAIN/fullchain.pem"
WEBHOOK_SSL_PRIV = "/etc/letsencrypt/live/YOUR_DOMAIN/privkey.pem"

API_TOKEN = config.token
bot = telebot.TeleBot(API_TOKEN)

app = web.Application()

# process only requests with correct bot token
async def handle(request):
    if request.match_info.get("token") == bot.token:
        request_body_dict = await request.json()
        update = telebot.types.Update.de_json(request_body_dict)
        bot.process_new_updates([update])
        return web.Response()
    else:
        return web.Response(status=403)

app.router.add_post("/{token}/", handle)

help_string = []
help_string.append("*Some bot* - just a bot.\n\n")
help_string.append("/start - greetings\n")
help_string.append("/help - shows this help")

# - - - messages

@bot.message_handler(commands=["start"])
def send_welcome(message):
    bot.reply_to(message, "Ololo, I am a bot")

@bot.message_handler(commands=["help"])
def send_help(message):
    bot.reply_to(message, "".join(help_string), parse_mode="Markdown")

# - - -

context = ssl.SSLContext(ssl.PROTOCOL_TLSv1_2)
context.load_cert_chain(WEBHOOK_SSL_CERT, WEBHOOK_SSL_PRIV)

# start aiohttp server (our bot)
web.run_app(
    app,
    host=WEBHOOK_LISTEN,
    port=WEBHOOK_PORT,
    ssl_context=context,
)
```

