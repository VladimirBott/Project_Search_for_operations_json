# Проект: Анализ финансовых транзакций с конвертацией валют

## Описание проекта
Проект предназначен для обработки финансовых транзакций из JSON-файлов с возможностью конвертации валют (USD, EUR) в рубли с использованием внешнего API.

## Структура проекта
```
project/
├── data/
│   └── operations.json          # JSON-файл с финансовыми транзакциями
├── src/
│   ├── __init__.py
│   ├── utils.py                 # Утилиты для работы с JSON-файлами
│   └── external_api.py          # Функции для работы с внешним API
├── tests/
│   ├── __init__.py
│   ├── test_utils.py            # Тесты для модуля utils
│   └── test_external_api.py     # Тесты для модуля external_api
├── .env.template                # Шаблон файла переменных окружения
├── requirements.txt             # Зависимости проекта
└── README.md                    # Документация проекта
```

## Установка и настройка

### 1. Клонирование репозитория
```bash
git clone <your-repository-url>
cd project
```

### 2. Установка зависимостей
```bash
pip install -r requirements.txt
```

### 3. Настройка переменных окружения
1. Скопируйте шаблон файла `.env.template` в `.env`:
```bash
cp .env.template .env
```

2. Откройте файл `.env` и добавьте ваш API-ключ от Exchange Rates Data API:
```
EXCHANGE_RATE_API_KEY=ваш_ключ_здесь
EXCHANGE_RATE_API_URL=https://api.apilayer.com/exchangerates_data
```

3. Получите API ключ на сайте [Exchange Rates Data API](https://apilayer.com/marketplace/exchangerates_data-api)

## Использование

### Основные функции

#### 1. Чтение транзакций из JSON-файла
```python
from src.utils import load_transactions

# Загрузка транзакций из файла
transactions = load_transactions('data/operations.json')
```

#### 2. Конвертация суммы транзакции в рубли
```python
from src.external_api import convert_transaction_to_rub

transaction = {
    "id": 1,
    "amount": 100.0,
    "currency": "USD",
    "date": "2024-01-01"
}

amount_in_rub = convert_transaction_to_rub(transaction)
print(f"Сумма в рублях: {amount_in_rub}")
```

### Пример обработки транзакций
```python
from src.utils import load_transactions
from src.external_api import convert_transaction_to_rub

# Загрузка всех транзакций
transactions = load_transactions('data/operations.json')

# Конвертация каждой транзакции в рубли
for transaction in transactions:
    amount_in_rub = convert_transaction_to_rub(transaction)
    print(f"Транзакция {transaction['id']}: {amount_in_rub} руб.")
```

## Тестирование

### Запуск тестов
```bash
# Запуск всех тестов
python -m pytest tests/

# Запуск тестов с детализацией
python -m pytest tests/ -v

# Запуск тестов с покрытием кода
python -m pytest tests/ --cov=src
```

### Тестовые сценарии
- Чтение корректного JSON-файла
- Обработка пустого файла
- Обработка невалидного JSON
- Конвертация транзакций в рублях
- Конвертация транзакций в USD/EUR с моками API
- Обработка ошибок сети при обращении к API

## Формат данных

### Структура транзакции
```json
{
    "id": 1,
    "amount": 100.0,
    "currency": "RUB",
    "description": "Оплата услуг",
    "date": "2024-01-01T12:00:00",
    "category": "services"
}
```

### Поддерживаемые валюты
- `RUB` - российский рубль (без конвертации)
- `USD` - доллар США (требует конвертации)
- `EUR` - евро (требует конвертации)

## Обработка ошибок

### Модуль utils
- Возвращает пустой список если:
  - Файл не найден
  - Файл пустой
  - Данные не являются списком
  - JSON некорректен

### Модуль external_api
- Возвращает сумму без изменений для RUB
- Выполняет конвертацию для USD/EUR через API
- Логирует ошибки при проблемах с API
- Использует кеширование курсов валют (опционально)

## Требования к окружению

### Python версия
- Python 3.8+

### Зависимости
```
requests>=2.28.0
python-dotenv>=0.19.0
pytest>=7.0.0
pytest-mock>=3.10.0
pytest-cov>=4.0.0
```

## Разработка

### Добавление новых функций
1. Создайте функцию в соответствующем модуле
2. Напишите тесты для функции
3. Обновите документацию при необходимости

### Форматирование кода
```bash
# Рекомендуется использовать black для форматирования
black src/ tests/

# И isort для сортировки импортов
isort src/ tests/
```

