---
icon: lucide/box
---

# Unity

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

## Начальный экран при запуске игры

Для кастомизации или отключения [заставки при запуске игры](https://docs.unity3d.com/Manual/class-PlayerSettingsSplashScreen.html) (логотипа Unity):

- Откройте окно **Edit** > **Project Settings** > **Player** > **Splash Image**
- Смените настройки для платформы **PC** (Windows, Mac, Linux settings)
- Проверьте результат нажав кнопку **Preview**
- Билды для консолей унаследуют выставленные параметры

## Настройка Input System

Если в проекте используется **Unity Input System** - добавьте соответствующий платформе пакет в папку `..\Packages` из папки с установленным SDK:

- Откройте `C:\Nintendo\Unity6000.1.15_NXAddon20.5.6-Unity6.1\UnityForNintendoSwitch\Packages`
- Скопируйте папку `com.unity.inputsystem.switch@0.1.7-pre`
- Откройте `C:\Work\Project_Name\Packages`
- Вставьте папку и дождитесь рекомпилляции проекта
- Проверьте чтобы в **Project Settings** > **Player** > **Other Settings** > **Controls** > **Supported Npad Styles** > были настройки **Full Key, Handheld, Joy Dual**

!!! note
    Если в проекте созданы глобальные настройки ввода в **Project Settings** > **Input System Package** > **Settings** > Добавьте в **Supported Devices** строки **Switch Controller (on Switch)** и **Switch Pro Controller**. Они будут отображаться в списке как **NPad** и **SwitchProControllerHID**.

    **Опционально**: добавьте по-необходимости **Gamepad**, **Keyboard**, **Mouse** и **Virtual Mouse**.

Если вы не работали с этим пакетом или не знакомы с основами кросс-платформенного ввода рекоммендую посмотреть официальный туториал от Unity: :fontawesome-brands-youtube:{ .youtube } [YouTube](https://youtu.be/5tOOstXaIKE?si=GkKBFIcOye7RFGBV).

> Подробнее о Input System в [документации Unity](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.19/manual/ActionsEditor.html).

## Вибрация для контроллера

Вибрацию в играх с поддержкой **Unity Input System** как правило добавляют так:

``` CSharp
Gamepad.current.SetMotorSpeeds(0.25f, 0.75f);
```

Самые частые проблемы:

- **Бесконечная вибрация** при включении Паузы, когда `Time.timeScale = 0` (не срабатывает остановка вибрации через `Invoke`)
- Завершение вибрации не срабатывает в `Coroutine`, если для отсчета интервалов используются `WaitForSeconds` или `Time.deltaTime`
- `NullReferenceException` в **Unity Editor** при отсутствии подключенного контроллера

Решением является замена `Time.deltaTime` на `Time.unscaledDeltaTime`, `WaitForSeconds` на  `WaitForSecondsRealtime` или `Invoke` на `Coroutine`, с учетом сказанного ранее.

В некоторых случаях, когда зависимость от игрового времени не обязательна - можно сделать метод остановки вибрации асинхронным:

``` CSharp
private async void ResetHaptics()
{
    await Task.Delay(500);

    if (Gamepad.current == null) return;

    Gamepad.current.ResetHaptics();
}
```

> Подробнее в [документации Unity](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.19/manual/Gamepad.html#rumble).

## Рефакторинг Save/Load System

На платформах **PC** и **PlayStation** сохранение и загрузка прогресса выполняются через `PlayerPrefs` или ассет Easy Save (ES3). Для **Nintendo Switch** все вызовы `PlayerPrefs` должны быть заменены на `SaveBridge` или `SwitchPrefs`.

## Адаптация UX/UI

Обратите внимание, что пропорции экранов могут отличаться в зависимости от режима работы устройства. **Nintendo Switch** использует разрешение 1280×720 в Handheld Mode и 1920×1080 в TV Mode. **Проверяйте отображение интерфейса при разных разрешениях** и включайте параметр `Preserve Aspect` у компонентов `Image` с типом `Sprite (2D and UI)`.

Также, можно настроить [пресет](https://docs.unity3d.com/Manual/class-PresetManager.html) чтобы любые загружаемые изображения автоматически становились `Sprite (2D and UI)` с параметром `Single`.

???+ example "Пример настроенного пресета"

    ![Texture Importer Preset](../assets/texture-importer-preset.png)

!!! note

    При экспорте изображений из **Figma** применяйте масштабирование **4x** либо **512w** если нужен конкретный размер (например, для трофеев PlayStation).

!!! tip

    Для добавления UI-подсказок для кнопок рекоммендуется использовать ассет [UI Hints Universal v3.0](https://trello.com/c/kbFEZagK/52-ui-hints-universal-auto-hints).

## Быстрая замена файлов

Для быстрой замены изображения, аудио или 3D-модели **замените файл в папке проекта** новым файлом с теми же **названием, типом и структурой** (например, иерархией FBX).

!!! note

    Такой подход экономит время, снижает вероятность ошибок и упрощает отслеживание изменений в Git по сравнению с изменениями сцен.

## Тесты методов в Unity Editor

Чтобы проверить функционал в редакторе Unity, добавьте над методом атрибут [ContextMenu](https://docs.unity3d.com/ScriptReference/ContextMenu.html) (например, `[ContextMenu("SomeMethod")]`). Найдите объект с этим скриптом в окне **Inspector** на сцене, нажмите на три точки у компонента `(Script)` и выберите свое контекстное меню. Оно запустит ваш тестовый метод. Работает вне зависимости от **Play Mode**.

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

???+ example "Пример настроек из Shader Graph"

    ![Texture Importer Preset](../assets/shader-graph-urp.png)

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

## Оптимизация

Обеспечение стабильных **30+** кадров в секунду (FPS) требует **балансировки** нагрузки **между CPU и GPU**.

Выберите подходящий инструмент и проверьте как работает билд **на целевой платформе**. Исходя из полученных данных примените соответствующие [методы по оптимизации][Analysis]:

- **Снижение нагрузки на CPU**: настройка Матрицы физики ([Layer Collision Matrix][Layer-based collision]); удаление лишних компонентов `Rigidbody`; замена [коллайдеров][Physics Debug] на примитивы (`Sphere`, `Box`) и/или включение параметра `Convex`/`Delaunay`; смена покадровых проверок вроде `Update()` на `Events`; ограничение количества одновременно включённых AI Agents (`NavMesh`); замена `Instantiate()`/`Destroy()` на Object pooling; оптимизация кода и UI (например, система вложенных `Canvas` для разделения фона и анимаций -> [prevent Canvas Rebuild][Frame Debugger])
- **Снижение нагрузки на GPU**: `Render scale` 0.8-0.9; уменьшение количества рисуемых объектов в одном кадре (Draw Calls): уменьшение [дальности прорисовки][Far clipping plane] (`Camera` > `Clipping Planes` > `Far`), настройка рендеринга (основная нагрузка - Тени, Bloom, SSAO); замена тяжёлых кастомных шейдеров на стандартные; меньше объектов с прозрачностью (Overdraw) -> например, убрать часть слоев у компонента `Terrain`
- **Перенос с GPU на RAM**: запечка [Occlusion culling][Occlusion culling]; запечка освещения ([Baked Light][Baked Light]); система [Level of Detail (LOD)][Level of Detail]; конвертация `Terrain` в 3-D модель ассетом [Terrain To Mesh][Terrain To Mesh]
- **Освобождение RAM**: [сжатие текстур][Compressor Texture]; стриминг аудио; удаление лишних уровней LOD (достаточно хорошо настроенных 2-3); использование AssetBundles или Addressables вместо прямой [загрузки ассетов][Runtime asset management]; кэширование данных; очистка памяти (например, в пустой сцене с загрузочным экраном) при помощи `Resources.UnloadUnusedAssets()`

[Analysis]: https://docs.unity3d.com/Manual/analysis.html
[Layer-based collision]: https://docs.unity3d.com/Manual/LayerBasedCollision.html
[Physics Debug]: https://docs.unity3d.com/Manual/PhysicsDebugVisualization.html
[Frame Debugger]: https://docs.unity3d.com/Manual/FrameDebugger-debug.html
[Far clipping plane]: https://docs.unity3d.com/Manual/UnderstandingFrustum.html
[Occlusion culling]: https://docs.unity3d.com/Manual/OcclusionCulling.html
[Baked Light]: https://docs.unity3d.com/Manual/LightModes-introduction.html
[Level of Detail]: https://docs.unity3d.com/Manual/LevelOfDetail.html
[Terrain To Mesh]: https://trello.com/c/I964mG8n/37-terrain-optimization
[Compressor Texture]: https://trello.com/c/6PvnJtNf/27-new-test-tools-convertor-material-compressor-texture-srp-urp-hdrp
[Runtime asset management]: https://docs.unity3d.com/Manual/assets-managing-introduction.html

???+ example "Профилирование на подключенной консоли"

    ![Nintendo Switch Profiling](../assets/switch-profiling.png)

???+ example "Включите `Mouse Select` для удобного выделения"

    ![Physics Debug Rendering](../assets/physics-debug-rendering.png)

???+ example "Отключите `Show Static Colliders` чтобы найти триггер-зоны"

    ![Physics Debug Filtering](../assets/physics-debug-filtering.png)

???+ example "Проверка покадровой отрисовки для оптимизации UI"

    ![Frame Debugger Window](../assets/frame-debugger.png)

## Анализ проекта

Основными инструментами для анализа проекта являются:

- [Project Auditor](https://docs.unity3d.com/Manual/project-auditor/project-auditor-introduction.html)
- [Profiler ](https://docs.unity3d.com/Manual/profiler-introduction.html)
- [Memory Profiler](https://docs.unity3d.com/Packages/com.unity.memoryprofiler@1.1/manual/memory-on-device.html)
- [Visual Studio debugger](https://learn.microsoft.com/ru-ru/visualstudio/debugger/debugger-feature-tour?view=visualstudio)
- [Debug.LogError](https://docs.unity3d.com/ScriptReference/Debug.LogError.html)

!!! tip "Unity Debug Styling"

    Unity класс `Debug` поддерживает [Rich-Text форматирование](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/StyledText.html).