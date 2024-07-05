# Laravel API - Хранение и обработка информации об оборудовании
## Требования
- Laravel 11.x требует, как минимум, версию PHP 8.2.
- Composer
## Рекомендуемое вспомогательное ПО
- [PHPStorm](https://www.jetbrains.com/ru-ru/phpstorm/)
- [Postman](https://www.postman.com)
## Установка
1. Скачайте проект:
   - Вариант 1: скачайте [архив](https://github.com/TheKompreso/laravel-showcase-2024-07-05/archive/refs/heads/master.zip) с проектом  и разархивируйте в нужную папку.
   - Вариант 2: с помощью git clone:
```
git clone https://github.com/TheKompreso/laravel-showcase-2024-07-05
```
2. Обновите зависимости проекта. В консоли перейдите в корневую папку с проектом и выполните команду:
```
composer update
```
3. Найдите файл <b>.env.example</b>, переименуйте его в <b>.env</b> и настройте связь с вашей базой данных MySQL. Найдите строчку '<b>DB_CONNECTION=mysql</b>' и введите данные вашей MySQL-базы. Пример:
```
DB_CONNECTION=mysql
DB_HOST=YOUR_database_host
DB_PORT=3306
DB_DATABASE=YOUR_laravel_db
DB_USERNAME=YOUR_laravel_user
DB_PASSWORD=YOUR_password
```
Также можно отключить Debug-мод при выпуске API в открытый доступ, если это требуется:
```
APP_DEBUG=false
```
4. Запустите миграцию базы данных
```
php artisan migrate
```
5. Можно изменить количество элементов отображаемых в пагинированных списоках: папка config, файл api.php
```
'paginate_page_size' => 20,
```
6. Запустите локальный сервер
```
php artisan serve
```
7. Переходите в программу Postman и начинайте тестирование
## SQL наполнение
<details>
<p>Это можно было бы сделать через внутренние инструменты Laravel (seeder или factory), но в данной ситуации быстрее просто вставить записи в базу данных и продублировать их сюда как команды.</p>
   
SQL-запрос:
    
```
INSERT INTO `equipment_types` (`id`, `name`, `mask`, `created_at`, `updated_at`) VALUES
    (1, 'TP-Link TL-WR74', 'XXAAAAAXAA', '2024-07-04 07:32:56', '2024-07-04 07:32:58'),
    (2, 'D-Link DIR-300', 'NXXAAXZXaa', '2024-07-04 07:32:59', '2024-07-04 07:33:00'),
    (3, 'D-Link DIR-300 E', 'NAAAAXZXXX', '2024-07-04 07:33:01', '2024-07-04 07:33:02'),
    (4, 'Space Ultra LINK 3000', 'XNNNNNNNNN', '2024-07-04 07:33:21', '2024-07-04 07:33:21'),
    (5, 'Space Ultra LINK 2000', 'XNNNNNNNNa', '2024-07-04 07:33:21', '2024-07-04 07:33:21');
```
```
INSERT INTO `equipment` (`id`, `equipment_type_id`, `serial_number`, `desc`, `deleted_at`, `created_at`, `updated_at`) VALUES
    (1, 1, '73NITRO5NZ', NULL, NULL, '2024-07-04 02:35:58', '2024-07-04 02:35:58'),
    (2, 1, '55AITRO5NZ', 'Необычный роутер', NULL, '2024-07-04 02:35:58', '2024-07-04 02:35:58'),
    (3, 1, '35NITRO5NZ', 'Обычный роутер', NULL, '2024-07-04 02:35:58', '2024-07-04 02:35:58'),
    (4, 1, '77NITRO5NZ', 'Обычный роутер', NULL, '2024-07-04 02:35:59', '2024-07-04 02:35:59'),
    (5, 1, '38NITRO5NZ', 'foo', NULL, '2024-07-04 02:35:59', '2024-07-04 02:35:59'),
    (6, 1, '21NITRO6NK', 'Обычный роутер', NULL, '2024-07-04 02:35:59', '2024-07-04 02:35:59'),
    (7, 4, 'X101010100', 'Супер роутер', NULL, '2024-07-04 02:36:00', '2024-07-04 02:36:00'),
    (8, 5, 'X10101010a', 'Не Супер роутер', NULL, '2024-07-04 02:36:00', '2024-07-04 02:36:00');
```
</details>

## API-методы
Workspace в Postman: https://www.postman.com/kompreso/workspace/answer-equipment-api/overview (для доступа к localhost требуется скачать и установить приложение Postman)

### GET: /api/equipment
Получить список оборудования.

Query Parameter   | Type   | Description
----------------- |--------| ------------------------------------------------------------------
``equipment_type_id`` | int    | Выполняет поиск по ID типа оборудования.
``serial_number`` | string | Выполняет поиск по серийному номеру оборудования.
``desc`` | string | Выполняет поиск по примечанию/комментарию к оборудованию.
``q``        | string | Выполняет поиск по всем полям (equipment_type_id, serial_number, desc).


Пример запроса:<br>
```GET: /api/equipment?q=NITRO```

Ответ:<br>
```
{
    "equipments": [
        {
            "id": 2,
            "equipment_type": {
                "id": 1,
                "name": "TP-Link TL-WR74",
                "mask": "XXAAAAAXAA"
            },
            "serial_number": "34NITRO6NK",
            "desc": "Необычный роутер",
            "created_at": "2024-07-03T17:45:07.000000Z",
            "updated_at": "2024-07-03T17:48:06.000000Z"
        },
        {
            "id": 3,
            "equipment_type": {
                "id": 1,
                "name": "TP-Link TL-WR74",
                "mask": "XXAAAAAXAA"
            },
            "serial_number": "30NITRO5NZ",
            "desc": "foo",
            "created_at": "2024-07-03T17:45:07.000000Z",
            "updated_at": "2024-07-03T17:45:07.000000Z"
        },
        {
            "id": 4,
            "equipment_type": {
                "id": 1,
                "name": "TP-Link TL-WR74",
                "mask": "XXAAAAAXAA"
            },
            "serial_number": "31NITRO5NZ",
            "desc": "Обычный роутер",
            "created_at": "2024-07-03T17:48:12.000000Z",
            "updated_at": "2024-07-03T17:48:12.000000Z"
        }
    ],
    "links": {
        "first": "http://localhost:8000/api/equipment?page=1",
        "last": "http://localhost:8000/api/equipment?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "active": false
            },
            {
                "url": "http://localhost:8000/api/equipment?page=1",
                "label": "1",
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "active": false
            }
        ],
        "path": "http://localhost:8000/api/equipment",
        "per_page": 20,
        "to": 3,
        "total": 3
    }
}
```


### GET: /api/equipment/{id}
Запрос данных оборудования по ID

Пример запроса:<br>
```GET: /api/equipment/1```

Ответ:<br>
```
{
    "equipment": {
        "id": 1,
        "equipment_type": {
            "id": 1,
            "name": "TP-Link TL-WR74",
            "mask": "XXAAAAAXAA"
        },
        "serial_number": "34NITRO5NX",
        "desc": "Роутер в доме напротив башни",
        "created_at": "2024-07-03T14:51:48.000000Z",
        "updated_at": "2024-07-03T14:51:51.000000Z"
    }
}
```

### POST: /api/equipment
Создание новой(ых) записи(ей). Принимает массив из объектов, которые требуется создать. При невозможности создать объект, просто пропускает его, продолжая создавать другие.


Пример запроса:<br>
```POST: /api/equipment```
```
{
    "equipments": [
        {
                "equipment_type_id": 1,
                "serial_number": "26NITRO6NK",
                "desc": "Обычный роутер"
        },
        {
                "equipment_type_id": 1,
                "serial_number": "31NITRO5NZ",
                "desc": "Обычный роутер"
        },
        {
                "equipment_type_id": 1,
                "serial_number": "30NITRO5NZ",
                "desc": "foo"
        }
    ]
}
```
Ответ:<br>
```
{
    "errors": [
        {
            "serial_number": [
                "The (equipment_type_id, serial_number) has already been taken."
            ]
        }
    ],
    "success": {
        "1": {
            "equipment_type_id": 1,
            "serial_number": "39NITRO5NZ",
            "desc": "Обычный роутер",
            "updated_at": "2024-07-03T17:52:32.000000Z",
            "created_at": "2024-07-03T17:52:32.000000Z",
            "id": 7
        },
        "2": {
            "equipment_type_id": 1,
            "serial_number": "38NITRO5NZ",
            "desc": "foo",
            "updated_at": "2024-07-03T17:52:32.000000Z",
            "created_at": "2024-07-03T17:52:32.000000Z",
            "id": 8
        }
    }
}
```
### PUT: /api/equipment/{id}
Редактирование записи по id.

Пример запроса:<br>
```PUT: /api/equipment/1```
```
{
    "equipment_type_id": 1,
    "serial_number": "34NITRO6NK",
    "desc": "Необычный роутер"
}
```
Ответ:<br>
```
{
    "id": 2,
    "equipment_type_id": 1,
    "serial_number": "34NITRO6NK",
    "desc": "Необычный роутер",
    "deleted_at": null,
    "created_at": "2024-07-03T17:45:07.000000Z",
    "updated_at": "2024-07-03T17:48:06.000000Z",
    "equipment_type": {
        "id": 1,
        "name": "TP-Link TL-WR74",
        "mask": "XXAAAAAXAA"
    }
}
```
### DELETE: /api/equipment/{id}
Удаление записи (мягкое удаление).

Пример запроса:<br>
```DELETE: /api/equipment/1```

Ответ:<br>
```
{
    "success": "Deleted"
}
```

### GET: /api/equipment-type
Вывод пагинированного списка типов оборудования с возможностью поиска путем указания query параметров советующим ключам объекта, либо указанием параметра q.

Query Parameter   | Type   | Description
----------------- |--------| ------------------------------------------------------------------
``name`` | string | Выполняет поиск по имени типа оборудования.
``mask`` | string | Выполняет поиск по маске оборудования.
``q``   | string | Выполняет поиск по всем полям (name, mask).


Пример запроса:<br>
```GET: /api/equipment-type?q=Link```

Ответ:<br>
```
{
    "equipmentTypes": [
        {
            "id": 1,
            "name": "TP-Link TL-WR74",
            "mask": "XXAAAAAXAA"
        },
        {
            "id": 2,
            "name": "D-Link DIR-300",
            "mask": "NXXAAXZXaa"
        },
        {
            "id": 3,
            "name": "D-Link DIR-300 E",
            "mask": "NAAAAXZXXX"
        }
    ],
    "links": {
        "first": "http://localhost:8000/api/equipment-type?page=1",
        "last": "http://localhost:8000/api/equipment-type?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "links": [
            {
                "url": null,
                "label": "&laquo; Previous",
                "active": false
            },
            {
                "url": "http://localhost:8000/api/equipment-type?page=1",
                "label": "1",
                "active": true
            },
            {
                "url": null,
                "label": "Next &raquo;",
                "active": false
            }
        ],
        "path": "http://localhost:8000/api/equipment-type",
        "per_page": 20,
        "to": 3,
        "total": 3
    }
}
```

## Улучшения, которые можно было бы сделать
- Добавить версию API (/api/v1/method)
- Добавить Front (на Vue.js), но ещё не работал с этой библиотекой, поэтому возможно чутка позже (если нужно)
- А ещё можно проиндексировать все поля, которые активно используются для поиска (но над этим надо внимательно подумать)
