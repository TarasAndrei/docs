---
icon: lucide/square-terminal
---

# Refactoring Save/Load

## Save/Load System

На платформах **Standalone (ПК)** и **PlayStation** сохранение и загрузка прогресса выполняются через `PlayerPrefs` или ассет Easy Save (ES3). Для **Nintendo Switch** все вызовы `PlayerPrefs` должны быть заменены на `SaveBridge` или `SwitchPrefs`.

!!! note

    Иногда ассеты загружаются на фоне при помощи [аттрибута][Attribute] `[RuntimeInitializeOnLoadMethod]`.

    > Подробнее о порядке вызова [системных методов Unity](https://docs.unity3d.com/Manual/execution-order.html).

[Attribute]: https://docs.unity3d.com/6000.4/Documentation/ScriptReference/RuntimeInitializeOnLoadMethodAttribute.html

## Проекты на Unity 202X

Для апгрейда c версий Unity 5 до Unity 6 проектов с уже интегрированным пакетом `PlayerPrefs_Switch` рекоммендуется использовать пакет `SwitchPrefs`.

> Подробнее в [документации][SwitchPrefs].

[SwitchPrefs]: https://trello.com/c/ElZ4C5dq/65-switch-player-prefsunity-6-to-switchprefs

## Проекты на Unity 6

Для рефактора новых проектов можно использовать `SwitchPrefs` или `SaveBridge`.

Обратите внимание, что в случае с `SaveBridge` названия методов немного изменятся. Так, `PlayerPrefs.GetInt` станет `SaveBridge.GetIntPP`, `PlayerPrefs.SetInt` > `SaveBridge.SetIntPP` и, по-аналогии, то же самое с `Int/Float/String/Key`.

Пример можно найти в файле `SaveBridge.cs`, в регионе PlayerPrefs Helper.

!!! warning

    Важно знать, что метод `PlayerPrefs.DeleteAll()` не работает на Nintendo Switch. Для удаления сохранений вам придется удалять ранее созданные ключи, либо сбрасывать их значения на `default`.

??? note "Пример метода SaveBridge.DeleteAllPP"

    ``` CSharp title="Добавьте в SaveBridge.cs"
        public static void DeleteAllPP()
        {
    #if UNITY_SWITCH && !UNITY_EDITOR
            cache.Clear();
            ES3.DeleteFile();
    #else
            PlayerPrefs.DeleteAll();
    #endif
            SaveAllData();
        }
    ```

> Подробнее в [документации][SaveBridge].

[SaveBridge]: https://trello.com/c/tJRPsbDi/48-savebridge-unity-6

### Работа в Visual Studio

Удобным методом замены является:

- Открыть любой файл _Assembly_ (например, из редактора Unity)
- Cделать глобальный поиск (++ctrl+shift+f++) по запросу `PlayerPrefs.GetInt`
- Открыть все, кроме скриптов Easy Save (ES3), SaveBridge, Rewired и других библиотек
- Заменить эту строку на новую, поменяв настройку поиска на `All open documents`
- Повторить `поиск` > `замену` с другими методами `PlayerPrefs` (`Float/String/Key`)

???+ example "Find and Replace > Replace in Files > All open documents > Replace All"

    ![Visual Studio Replace](../assets/vs-replace-all.png)

!!! tip

    После завершения проверьте возможные ошибки компиляции нажав ++ctrl+shift+b++.

## Тесты методов в Unity Editor

Чтобы проверить функционал в редакторе Unity, добавьте над методом атрибут [ContextMenu] (например, `[ContextMenu("SomeMethod")]`). Найдите объект с этим скриптом в окне **Inspector** на сцене, нажмите на три точки у компонента `(Script)` и выберите свое контекстное меню. Оно запустит ваш тестовый метод. Работает вне зависимости от **Play Mode**.

[ContextMenu]: https://docs.unity3d.com/ScriptReference/ContextMenu.html