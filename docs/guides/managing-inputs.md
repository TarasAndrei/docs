---
icon: lucide/file-cog
---

# Managing Inputs

## Настройка Input System

Если в проекте используется **Unity Input System** - добавьте **соответствующий** платформе и **версии Unity проекта** пакет в папку `..\Packages` из папки с установленным SDK:

- Например, откройте `C:\Nintendo\Unity6000.1.15_NXAddon20.5.6-Unity6.1\UnityForNintendoSwitch\Packages`
- Скопируйте папку `com.unity.inputsystem.switch@0.1.7-pre`
- Откройте `C:\Work\Project_Name\Packages`
- Вставьте папку и дождитесь рекомпилляции проекта
- Проверьте чтобы в **Project Settings** > **Player** > **Other Settings** > **Controls** > **Supported Npad Styles** > были настройки **Full Key, Handheld, Joy Dual**

!!! note
    Если в проекте созданы глобальные настройки ввода в **Project Settings** > **Input System Package** > **Settings** > Добавьте в **Supported Devices** строки **Switch Controller (on Switch)** и **Switch Pro Controller**. Они будут отображаться в списке как **NPad** и **SwitchProControllerHID**.

    **Опционально**: добавьте по-необходимости **Gamepad**, **Keyboard**, **Mouse** и **Virtual Mouse**.

??? note "Пример вызова метода по нажатию на кнопку (связь с действием из Input Action)"

    ``` CSharp title="InputActionSample.cs"
    using UnityEngine;
    using UnityEngine.InputSystem;

    public sealed class InputActionSample : MonoBehaviour
    {
        [SerializeField] private InputActionReference _inputAction;
        
        private void OnEnable()
        {
            if (_inputAction == null) return;
            _inputAction.action.performed += SomeMethod;
            _inputAction.action.Enable();
        }
        
        private void OnDisable()
        {
            if (_inputAction == null) return;
            _inputAction.action.performed -= SomeMethod;
            _inputAction.action.Disable();
        }

        /// <summary>
        /// Receives the <see cref="InputAction.CallbackContext"/> from the bound action.
        /// Replace this body with your own logic.
        /// </summary>
        private void SomeMethod(InputAction.CallbackContext context)
        {
            Debug.LogError("<color=cyan>SomeMethod triggered</color>");
        }
    }
    ```

Если вы не работали с этим пакетом или не знакомы с основами кросс-платформенного ввода рекоммендую посмотреть официальный туториал от Unity: :fontawesome-brands-youtube:{ .youtube } [YouTube](https://youtu.be/5tOOstXaIKE?si=GkKBFIcOye7RFGBV).

> Подробнее о Input System в [документации Unity](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.19/manual/ActionsEditor.html).

## Вибрация для контроллера

Вибрацию в играх с поддержкой **Unity Input System** как правило добавляют так:

``` CSharp
Gamepad.current.SetMotorSpeeds(0.25f, 0.75f);
```

> Подробнее в [документации Unity](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.19/manual/Gamepad.html#rumble).

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