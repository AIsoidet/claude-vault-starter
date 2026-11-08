---
description: таблица orders — поля и три ловушки (тестовые заказы, возвраты, валюта)
updated: 2026-11-01
---
# Таблица orders

Один заказ = одна строка. Пример выгрузки за октябрь — [[sources/orders_2026-10.csv]].

## Поля

| Поле | Что это |
|---|---|
| `order_id` | id заказа |
| `user_id` | id покупателя |
| `order_date` | дата заказа |
| `channel` | откуда пришёл: `organic`, `ads`, `email`, `referral` |
| `amount_rub` | сумма в рублях |
| `status` | `paid`, `returned`, `cancelled` |

## Ловушки

1. **Тестовые заказы.** `user_id` начинается с `test_` — это QA. Исключать всегда.
2. **Возвраты.** Возврат — отдельный статус `returned`, сумма остаётся положительной.
   Выручка = только `paid`. `returned` и `cancelled` в выручку не входят.
3. **Валюта.** Всё в рублях, конвертации нет.

```sql
SELECT sum(amount_rub) AS revenue
FROM orders
WHERE status = 'paid'
  AND user_id NOT LIKE 'test_%'
```

## Связи
- [[notes/метрика-активный-пользователь]]
