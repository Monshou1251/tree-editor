# Редактор дерева на Vue 3 и Quasar

## Описание проекта
Проект представляет собой веб-приложение с функцией редактора дерева, реализованное с использованием Vue 3 и Quasar. Приложение поддерживает произвольную вложенность узлов дерева, позволяя добавлять, редактировать и удалять узлы, а также сохранять текущее состояние дерева в локальном хранилище (localStorage) для восстановления при перезагрузке страницы. Приложение собрано в APK для мобильных устройств.

## Технические требования
- **Frontend**: Vue 3
- **UI фреймворк**: Quasar
- **Компонент дерева**: Поддержка произвольной вложенности узлов

## Функциональные возможности
- **Добавление узлов**: Возможность добавлять новые узлы к любому существующему узлу.
- **Редактирование узлов**: Функция редактирования имени узла через интерфейс.
- **Удаление узлов**: Возможность удаления узлов и их дочерних элементов.
- **Сохранение состояния**: Текущее состояние дерева сохраняется в локальном хранилище браузера.
- **Восстановление состояния**: При перезагрузке страницы структура дерева восстанавливается из локального хранилища.

## Инструкции по сборке и запуску

### Запуск проекта в режиме разработки
1. Убедитесь, что у вас установлен [Node.js](https://nodejs.org/).
2. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/Monshou1251/tree-editor.git
   cd tree-editor
   ```
3. Установите зависимости:
   ```bash
   npm install
   ```
4. Установите зависимости:
   ```bash
   quasar dev
   ```
5. Перейдите по адресу http://localhost:9000 в браузере для тестирования приложения.

### Запуск APK на Android-устройстве
APK-файл уже создан и находится в папке apk под именем tree-editor.apk. Чтобы установить приложение на Android-устройство:
1. Перенесите apk/tree-editor.apk на устройство.
2. Включите "Установка из неизвестных источников" на устройстве.
3. Установите APK и запустите приложение.
