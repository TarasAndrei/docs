---
icon: lucide/square-terminal
---

# Refactoring Save/Load

## Рефакторинг Save/Load System

На платформах **Standalone (ПК)** и **PlayStation** сохранение и загрузка прогресса выполняются через `PlayerPrefs` или ассет Easy Save (ES3). Для **Nintendo Switch** все вызовы `PlayerPrefs` должны быть заменены на `SaveBridge` или `SwitchPrefs`.

!!! note

    Иногда ассеты загружаются не прямым вызовом метода, а при помощи [аттрибута](https://docs.unity3d.com/6000.4/Documentation/ScriptReference/RuntimeInitializeOnLoadMethodAttribute.html).

> Подробнее о порядке вызова [системных методов Unity](https://docs.unity3d.com/Manual/execution-order.html).

## Тесты методов в Unity Editor

Чтобы проверить функционал в редакторе Unity, добавьте над методом атрибут [ContextMenu](https://docs.unity3d.com/ScriptReference/ContextMenu.html) (например, `[ContextMenu("SomeMethod")]`). Найдите объект с этим скриптом в окне **Inspector** на сцене, нажмите на три точки у компонента `(Script)` и выберите свое контекстное меню. Оно запустит ваш тестовый метод. Работает вне зависимости от **Play Mode**.