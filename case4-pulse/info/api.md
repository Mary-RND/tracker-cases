# Контракт API · Pulse (для этапа 2)

Клиентский MVP работает на localStorage. Ниже — минимальный контракт, который понадобится,
когда появится серверная часть (PHP + React по ТЗ клиента).

## Аутентификация
`POST /api/auth/login` → `{ token, user: { id, name, role } }`
Заголовок: `Authorization: Bearer <token>`.

## Проекты
| Метод | Путь | Описание |
|---|---|---|
| GET | `/api/projects` | список проектов пользователя |
| POST | `/api/projects` | создать (owner/manager) |
| PATCH | `/api/projects/{id}` | изменить |
| DELETE | `/api/projects/{id}` | удалить (owner) |

## Задачи
| Метод | Путь | Описание |
|---|---|---|
| GET | `/api/tasks?project=&assignee=&status=` | фильтры |
| POST | `/api/tasks` | создать |
| PATCH | `/api/tasks/{id}` | изменить поля/статус |
| POST | `/api/tasks/{id}/comments` | комментарий |
| POST | `/api/tasks/{id}/time` | `{ hours, comment }` |

Правила:
- `PATCH status=done` требует, чтобы задача была в `review`.
- WIP-лимит проверяется на сервере: `409 WIP_LIMIT`.
- Исполнитель может менять только свои задачи; наблюдатель — только `GET`.

## Спринты
`GET /api/sprints/{id}` → `{ start, end, capacity, tasks[] }`
`GET /api/sprints/{id}/burndown` → `[{ date, remaining }]`

## Отчёты
`GET /api/reports/utilization?from=&to=` → по исполнителям: `{ planned, actual, done, late }`.

## Коды ошибок
`400` валидация, `401` нет токена, `403` роль, `404` объект, `409` конфликт (WIP), `422` бизнес-правило.
