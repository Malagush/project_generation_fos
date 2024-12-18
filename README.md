# Проект: Динамическая форма

## Описание
Этот проект представляет собой динамическую форму обратной связи (ФОС). 
Данные формы сохраняются в `sessionStorage`.

## Технологии
- Vue.js
- TypeScript
- CSS
- JSON

## Установка

1. Установите зависимости:
npm install

## Запуск проекта
npm run serve

## Структура проекта
src/
│
├── assets/
│   └── formData.json          # JSON файл с харкодными данными
│
├── components/ 
│    ├── DynamicForm.vue    # Основной компонент 
│    ├── DynamicFormTemplate.vue # Шаблон 
│    └── DynamicFormTemplate.css  # CSS стили
│
└── App.vue


## Лицензия
Этот проект лицензирован под MIT License. Подробности смотрите в файле [LICENSE](LICENSE).
