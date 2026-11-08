# Выгрузка схемы из DWH: events (вымышленные данные)

Выгружено 2026-10-15, ClickHouse, база `analytics`.

| column | type | comment |
|---|---|---|
| event_date | Date | дата события по МСК |
| event_ts | DateTime | время события |
| user_id | String | id пользователя; test_* — QA-аккаунты |
| event_name | LowCardinality(String) | app_open, push_open, screen_view, product_view, add_to_cart, order_created |
| platform | LowCardinality(String) | ios, android, web |
| channel | LowCardinality(String) | канал первого привлечения: organic, ads, email, referral |
| props | String | JSON с параметрами события |

Партиционирование: toYYYYMM(event_date). TTL 400 дней.
