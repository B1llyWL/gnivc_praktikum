   # GNIVC Receipts Analytics

   ## Описание
   Потоковый ETL-пайплайн для обработки чеков:
   - Источник: Apache Kafka (топик receipts-topic)
   - Обработка: Apache Spark Structured Streaming
   - Хранилище: PostgreSQL (таблица basic_metrics)

   ## Метрики
   - Общая выручка (total_revenue)
   - Количество чеков (receipts_count)
   - Средний чек (avg_check)
   - Уникальные клиенты (unique_clients)

   ## Запуск
   1. Запусти инфраструктуру: docker compose up -d
   2. Запусти Jupyter: jupyter notebook
   3. Отправь тестовые данные в Kafka и проверь PostgreSQL.
