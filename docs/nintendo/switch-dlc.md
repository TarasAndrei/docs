---
icon: lucide/blocks
---

# Nintendo Switch DLC

Для создания дополнительного контента для Nintendo Switch следуйте инструкции:

> Подробнее в [документации](https://docs.google.com/document/d/1q36-z_g-jhN_gUnT1YjfnIni-DUxoX20iuWTNGVhKME/edit?usp=sharing).

## Создание билда для DLC

Для тестов **DLC** необходимо **установить билд приложения и ключ**, открывающий новый контент. Контент входит в основной билд. Соответственно для проверки устанавливать и удалять можно **только ключ** (файл размером ~ 300-400 KB).

Билд **.nsp** файла-ключа для **AddOnContent** создается через командную строку:

- Нажмите ++win+r++ > введите `CMD` > ++enter++
- Введите команду по шаблону: **версия Unity проекта**\\**AuthoringTool** > **"Путь для билда"** > **"Путь к \_DLC_Project_Name.nmeta"**

``` CSharp title="Пример команды для CMD"
C:\Nintendo\Unity6000.1.15_NXAddon20.5.6-Unity6.1\NintendoSDK\Tools\CommandLineTools\AuthoringTool\AuthoringTool.exe creatensp -o "C:\Builds\_DLC_Project_Name.nsp" --save-adf --type AddOnContent --meta "C:\Work\Project_Name\_DLC_Project_Name.nmeta"
```

!!! warning

    Содержание этого документа является строго конфиденциальным. Обращайтесь с ним соответственно.