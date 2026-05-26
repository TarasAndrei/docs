---
icon: lucide/bug
---

# Quality Assurance (QA)

Для проверки билдов на соответствие стандартам Nintendo у нас есть [чек-лист](https://docs.google.com/spreadsheets/d/1GG1UTbHNJvI4GkP8csxbTQWSp3cIPwW8fKu_pHlFPRA/edit?gid=383483883#gid=383483883).

Для проверки билдов на соответствие стандартам Steam у нас есть [чек-лист](https://docs.google.com/spreadsheets/d/1yphqFk6BQN9Ym3UKWydVDYw9ptzro5JN9ZwKkJoDVtk/edit?usp=sharing).

??? info "Также вы можете ознакомиться:"

	- [Nintendo Switch Testing](https://docs.google.com/document/d/1RVHozeDx2ZtCdIt6FojWdHN_Q2C-5ewRuHTD7FutAmQ/edit?usp=sharing)
	- [Nintendo Switch QA Check List](https://docs.google.com/document/d/1811Xiet4NK3nyVQ2XYssZNi0cxWejFT0GTZz3fYYbSY/edit?usp=sharing)

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