---
icon: lucide/box
---

# Unity

## Начальный экран при запуске игры

Для кастомизации или отключения [заставки при запуске игры](https://docs.unity3d.com/Manual/class-PlayerSettingsSplashScreen.html) (логотипа Unity):

- Откройте окно **Edit** > **Project Settings** > **Player** > **Splash Image**
- Смените настройки для платформы **PC** (Windows, Mac, Linux settings)
- Билды для консолей унаследуют выставленные параметры

## Настройка Input System

Если в проекте используется **Unity Input System** - добавьте соответствующий платформе пакет в папку `..\Packages` из папки с установленным SDK.

> Подробнее о Input System в [документации Unity](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.19/manual/ActionsEditor.html).

## Рефакторинг Save/Load System

На платформах **PC** и **PlayStation** сохранение и загрузка прогресса выполняются через `PlayerPrefs` или ассет Easy Save (ES3). Для **Nintendo Switch** все вызовы `PlayerPrefs` должны быть заменены на `SaveBridge` или `SwitchPrefs`.

## Адаптация UX/UI

Обратите внимание, что пропорции экранов могут отличаться в зависимости от режима работы устройства. **Nintendo Switch** использует разрешение 1280×720 в Handheld Mode и 1920×1080 в TV Mode. **Проверяйте отображение интерфейса при разных разрешениях** и включайте параметр `Preserve Aspect` у компонентов `Image` с типом `Sprite (2D and UI)`.

Также, можно настроить [пресет](https://docs.unity3d.com/Manual/class-PresetManager.html) чтобы любые загружаемые изображения автоматически становились `Sprite (2D and UI)` с параметром `Single`.

!!! note

    При экспорте изображений из **Figma** применяйте масштабирование **4x** либо **512w** если нужен конкретный размер (например, для трофеев PlayStation).

## Быстрая замена файлов

Для быстрой замены изображения, аудио или 3D-модели **замените файл в папке проекта** новым файлом с теми же **названием, типом и структурой** (например, иерархией FBX).

!!! note

    Такой подход экономит время, снижает вероятность ошибок и упрощает отслеживание изменений в Git по сравнению с изменениями сцен.

## Тесты методов в Unity Editor

Чтобы проверить функционал в редакторе Unity, добавьте над методом атрибут [ContextMenu](https://docs.unity3d.com/ScriptReference/ContextMenu.html) (например, `[ContextMenu("Test")]`). Далее, во время `Play Mode` найдите объект с этим скриптом в окне `Inspector`, нажмите на три точки у компонента (Script) и выберите свое контекстное меню. Оно запустит ваш тестовый метод.

## Смена HDRP на URP

Установка пакета **Universal Render Pipeline**:

- Установите **URP** в **Window** > **Package Management** > **Package Manager** > **Unity Registry** > **Universal Render Pipeline**
- Смените пресет по умолчанию на **URP** в окне **Edit** > **Project Settings** > **Graphics**
- Настройте уровни качества в **Edit** > **Project Settings** > [**Quality**]( https://docs.unity3d.com/Manual/class-QualitySettings.html)

Добавление **URP** в настройки шейдера:

- Зайдите на сцену
- Выберите **3-D модель** со сломанным материалом (розового цвета - #ff00ffff)
- В окне **Inspector** (++ctrl+3++) найдите используемый шейдер
- Если это кастомный шейдер в **Shader Graph** , нажмите **Edit Shader...**
- Добавьте **URP** с настройкой `Alpha Clipping` и Сохраните (++ctrl+s++)

Чтобы перевести материалы, **шейдеры которых не поддерживают URP**:

- В окне **Project** выберите все материалы, которые вы бы хотели поменять (при помощи фильтра)
- Смените шейдер на **Standard (SRP)**
- Используйте встроенный конвертер для апгрйда: **Edit** > **Rendering** > **Materials** > **Convert Selected Build-in Materials to URP**.

!!! note

    Также, вы можете попробовать применить ассет [HDRP to URP Downgrader v1.1](https://trello.com/c/uLKQaVWh/41-convertor-material-hdrp-to-urp).

## Мигание света на сцене

Если при перемещении по сцене некоторые источники света мигают - проверьте количество источников **Realtime** освещения в окне **Window** > **Rendering** > **Light Exploder**. 

Также, проверьте текущие лимиты в **Project Settings** > **Quality** > **Render Pipeline Asset** > `Inspector` > **Lighting** > **Per Object Limit**. В отличие от PC, консоль поддерживает до 8 источников света на одну сетку (компонент `Mesh Renderer`).

Если проблема в лимите или количестве источников света - увеличьте лимит, замените часть источников на модели с эмиссивным материалом и/или запеките освещение на сцене.

## Анализ проекта

Основными инструментами для анализа проекта являются:

- [Project Auditor](https://docs.unity3d.com/Manual/project-auditor/project-auditor-introduction.html)
- [Profiler ](https://docs.unity3d.com/Manual/profiler-introduction.html)
- [Memory Profiler](https://docs.unity3d.com/Packages/com.unity.memoryprofiler@1.1/manual/memory-on-device.html)
- [Visual Studio debugger](https://learn.microsoft.com/ru-ru/visualstudio/debugger/debugger-feature-tour?view=visualstudio)
- [Debug.LogError](https://docs.unity3d.com/ScriptReference/Debug.LogError.html)

!!! note "Unity Debug Styling"

    Unity класс `Debug` поддерживает [Rich-Text форматирование](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/StyledText.html).