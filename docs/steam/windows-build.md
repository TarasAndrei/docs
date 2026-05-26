---
icon: lucide/hammer
---

# Steam Build

Для сборки и публикации Demo-версии на платформе Steam необходимо выполнить следующие шаги:

> Подробнее в [документации](https://docs.google.com/document/d/1i7jyrohkGkgwtduaQ6BKRPGikQrQqcnjrXe1MbKy6iY/edit?usp=sharing).

## Удаление сохранений

Если ваше приложение использует запись прогресса через `PlayerPrefs`:

- Проверьте поле **Company Name** в настройках **Unity Editor** > **Project Settings** > **Player**
- Нажмите ++win+r++ > введите `regedit` > ++enter++
- Найдите и удалите соответствующую папку в `Computer\HKEY_CURRENT_USER\SOFTWARE\Company Name\Product Name`

!!! warning

    Содержание этого документа является строго конфиденциальным. Обращайтесь с ним соответственно.