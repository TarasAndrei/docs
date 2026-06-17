---
icon: lucide/rocket
---

# Getting Started

## Начало работы

:wave: Добро пожаловать в команду!

**Чтобы начать** работу над проектом, **подготовьтесь**:

- [x] Прочитайте руководства ниже
- [x] Убедитесь, что у вас есть доступ (Trello, GitHub)
- [x] Настройте среду разработки (Unity, IDE, SDK)

Актуальные версии Unity: **6000.1.15** для текущих проектов и **6000.3.17** для новых.

<div class="grid cards" markdown>

-   :fontawesome-solid-gamepad:{ .lg .middle } __Nintendo__

    ---

    [:octicons-arrow-right-24: Nintendo Environment][switch build]

-   :fontawesome-brands-steam:{ .lg .middle } __Steam__

    ---

    [:octicons-arrow-right-24: Steam Build][steam build]

</div>

[switch build]: ../docs/nintendo/switch-environment.md
[steam build]: ../docs/steam/windows-build.md

!!! note "Три важных момента:"

    - Если не уверены - не стесняйтесь спросить
    - Следуйте Agile: создать -> проверить -> убрать лишнее -> доработать. Не улучшайте то, чего не будет в релизе
    - Ассеты, фреймворки и ИИ - всего лишь инструменты. Не тратьте на них лишнее время, используйте необходимый минимум

???+ question "К кому обращаться?"

    - Организационные вопросы: "Кто?", "Что?" и "Когда?" - обращайтесь к PM
    - Если вопрос касается технической реализации: "Как?" - обращайтесь к TL

Будьте готовы ответить на три вопроса:

1. Что сделано?
2. Над чем работаете сейчас?
3. Что будете делать дальше?

## Приоритеты

Приоритет **исправления багов**:

1. Вылеты и ошибки, мешающие прохождению игры
2. Баги механик и проблемы оптимизации
3. Терминология, визуальные баги и полировка

!!! note

    Обсуждайте планы с QA-специалистом. Группируйте исправления для удобства тестирования и контроля версий в Git.

Приоритеты **при написании кода**:

1. Простота ([KISS], [YAGNI])
2. Корректность ([Coding conventions])
3. Универсальность ([DRY])
4. Оптимизация ([SOLID], [TDA])

[Coding conventions]: https://unity.com/how-to/naming-and-code-style-tips-c-scripting-unity

[KISS]: https://en.wikipedia.org/wiki/KISS_principle
[YAGNI]: https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it
[DRY]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself
[SOLID]: https://en.wikipedia.org/wiki/SOLID
[TDA]: https://medium.com/@hlfdev/kiss-dry-solid-yagni-a-simple-guide-to-some-principles-of-software-engineering-and-clean-code-05e60233c79f