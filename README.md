# ZaMany API

API для запросов к нейросети ZaMany. Авторизация через TronLink-кошелёк.

## Авторизация

1. Зарегистрируйтесь в личном кабинете сайта через TronLink.
2. API-ключ = адрес кошелька (tron_wallet_address, начинается с T).
3. Используйте ключ в параметре `tron_wallet_address` всех запросов.

## Endpoints

Базовый URL: `http://138.124.35.201/wp-json/zamany/v1/`

### 1. Создать запрос (/save) — POST

**Параметры:**
- Query: `tron_wallet_address`.
- Body (JSON): `{"message": "Текст", "prompt_prefix": "Префикс (опц.)"}`.

**Пример:**
```
curl -X POST "http://138.124.35.201/wp-json/zamany/v1/save?tron_wallet_address=THAx9VZ...................ncZRjy" \
-H "Content-Type: application/json" \
-d "{\"message\": \"Сколько времени?\", \"prompt_prefix\": \"Ответь одним предложением будто ты на марсе:\"}"
```

**Ответ:**
```json
{"status":"ok","message_id":11,"prompt":"Ответь одним предложением будто ты на марсе: Сколько времени?"}
```

### 2. Проверить статус (/status) — GET

Поллинг до "processed".

**Параметры:**
- Query: `id` (message_id), `tron_wallet_address`.

**Пример:**
```
curl -X GET "http://138.124.35.201/wp-json/zamany/v1/status?id=11&tron_wallet_address=THAx9VZ...................ncZRjy"
```

**Ответ:**
```json
{"status":"processed"}
```

### 3. Получить ответ (/response) — GET

**Параметры:**
- Query: `id` (message_id), `tron_wallet_address`.

**Пример:**
```
curl -X GET "http://138.124.35.201/wp-json/zamany/v1/response?id=11&tron_wallet_address=THAx9VZ...................ncZRjy"
```

**Ответ:**
```json
{"ai_response":"На Марсе время — понятие относительное: закат тянется дольше, день длится чуть больше земного, а я всё так же измеряю его не часами — а колличеством сделанных шагов к будущему."}
```

## Цикл

1. POST /save → message_id.
2. GET /status (поллинг) до "processed".
3. GET /response → ответ.

Prefix по умолчанию: "Сформулируй в стиле мистического предсказания одним предложением...".
