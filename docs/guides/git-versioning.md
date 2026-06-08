---
icon: lucide/layers-plus
---

# Git Versioning

## Отсутствие файлов после клонирования

Если **сразу после клонирования** нового проекта с репозитория в приложении **GitHub Desktop** отображаются удаленные файлы с иконкой **(-)** - проверьте длину полного пути к отсутствующим файлам в проводнике.

Рекоммендуется клонировать проекты в короткие пути по типу `C:\Work`.

Иногда достаточно **включить поддержку длинных путей** в настройках **Git** и **Windows**:

- **ПКМ** на название **Current repository** в приложении **GitHub Desktop** > **Show in Explorer**
- Нажмите **ЛКМ** в область пути в проводнике > введите `CMD` > ++enter++
- Запустите команду `git config --global core.longpaths true`
- Далее нажмите ++win+r++ и введите > `gpedit.msc` > ++enter++
- Найдите параметр `Enable Win32 long paths` следуя **Computer Configuration** > **Administrative Templates** > **System** > **Filesystem**
-  **ПКМ** на название > **Edit** > **Enabled** > **Apply** > **OK**
- **Перезагрузите ПК** и проверьте GitHub Desktop

???+ example "Пример примененной настройки"

    ![Texture Importer Preset](../assets/win-long-paths.png)