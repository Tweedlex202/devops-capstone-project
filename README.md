# DevOps Capstone Project


![Build Status](https://github.com/Tweedlex202/devops-capstone-project/actions/workflows/ci-build.yaml/badge.svg)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python 3.9](https://img.shields.io/badge/Python-3.9-green.svg)](https://shields.io/)

## Описание проекта

Этот проект представляет собой микросервис для управления учётными записями клиентов на платформе электронной коммерции. Микросервис предоставляет REST API для выполнения CRUD-операций (создание, чтение, обновление, удаление) над учётными записями клиентов.

## Функциональность

- **Create** — создание новой учётной записи клиента
- **Read** — получение информации об учётной записи
- **Update** — обновление данных учётной записи
- **Delete** — удаление учётной записи
- **List** — получение списка всех учётных записей

## Технологии

- Python 3.9
- Flask (REST API)
- PostgreSQL (база данных)
- Docker (контейнеризация)
- Kubernetes (оркестрация)

## Модель данных

| Поле | Тип | Обязательное |
|------|-----|--------------|
| id | Integer | Да |
| name | String(64) | Да |
| email | String(64) | Да |
| address | String(256) | Да |
| phone_number | String(32) | Нет |
| date_joined | Date | Да |

## Структура проекта

```
├── service/            <- пакет микросервиса
│   ├── common/         <- общие обработчики логов и ошибок
│   ├── config.py       <- конфигурация Flask
│   ├── models.py       <- модель данных
│   └── routes.py       <- маршруты REST API
├── tests/              <- тесты
│   ├── factories.py    <- фабрики для тестов
│   ├── test_models.py  <- тесты модели
│   └── test_routes.py  <- тесты маршрутов
└── setup.cfg           <- конфигурация инструментов
```

## Установка и запуск

### Инициализация среды разработки


```bash
source bin/setup.sh
```

### Установка зависимостей

```bash
make install
```

### Запуск базы данных

```bash
make db
```

## Автор

Выполнено в рамках курса IBM DevOps and Software Engineering Professional Certificate

## Лицензия

Apache License 2.0
