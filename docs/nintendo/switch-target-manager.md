---
icon: lucide/cable
---

# Nintendo Connection

Для подключения консоли к ПК, установки билдов, просмотра логов и отладки требуется Nintendo Target Manager 2.

> Подробнее в [документации](https://docs.google.com/document/d/1HV6m-sbHxcaxTkMmuLgYDzqUn6KpCzG5/edit?usp=sharing&ouid=108020388252141246965&rtpof=true&sd=true).

## Проверка количества сохранений в минуту

Приложение **не должно** выполнять запись данных в память консоли чаще **32** раз в минуту. Перед отправкой финального билда проверьте этот пункт при помощи **FsAccessLogChecker**:

- Нажмите ++win+r++ > введите `CMD` > ++enter++
- Перетащите в окно командной строки **FsAccessLogChecker** из папки `C:\Nintendo\Unity6000.1.15_NXAddon20.5.6-Unity6.1\NintendoSDK\Tools\FsAccessLogChecker`
- Поставьте пробел нажав клавишу ++space++
- Перетащите в окно командной строки ваш файл **FsAccessLog.txt**
- Нажмите ++enter++

**Ожидаемый результат**: Все поля Result отображают `[PASS]`

!!! warning

    Содержание этого документа является строго конфиденциальным. Обращайтесь с ним соответственно.