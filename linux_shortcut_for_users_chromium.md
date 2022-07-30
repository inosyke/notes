# Вирішив у GNU/Linux Mint створити декількох користувачів у Chromium.

Проте зустрівся з проблемою відсутності кнопки для створення ярлику шоткату на робочий стіл, ну що ж, зробимо його самі.

| Як має бути: | Як у мене: |
|--------------|------------|
|![](https://github.com/inosyke/notes/blob/master/img/linux_shortcut_for_users_chromium_2.png)|![](https://github.com/inosyke/notes/blob/master/img/linux_shortcut_for_users_chromium_1.png)|

У Chromium киристувачі створюються з такими профілями --profile-directory:
* Перший користувач - Default
* Другий користувач - "Profile 1"
* Третій користувач - "Profile 2"
* ...

Щоб запустити потрібний нам профіль:
```bash
сhromium --profile-directory="необхідний_нам_профіль"
```
Тепер створимо шоткат, для цьго на робочому столі клікаємо правою клавішею миші та вставляємо команду.
![](https://github.com/inosyke/notes/blob/master/img/linux_shortcut_for_users_chromium_3.png?raw)
