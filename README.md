# Динамическая таблица

[Смотреть на GH Pages](https://slava191.github.io/dynamic-table/)

## Описание

Страница включает в себя сайдбар, хедер, футер и саму таблицу. Футер всегда прижат к низу страницы.

### Функционал таблицы:

* Сортировка по столбцам: при нажатии на название столбца, строки таблицы сортируются по алфавиту, при повторном клике - в обратном порядке.Графическим элементом указывается направление сортировки.

* Фильтрация: вверху таблицы есть текстовое поле, в которое пользователь может ввести текст и строки таблицы, данные которых не содержат подстроку, введённую пользователем, скрываются.

* По клике на строку таблицы значения полей выводятся в дополнительном блоке под таблицей с надписью: Выбран автомобиль Audi A4 2005 года выпуска

Данные для таблицы загружаются с API по адресу https://city-mobil.ru/api/cars 

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
