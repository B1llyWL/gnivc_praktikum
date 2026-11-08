<div align="center">
<h1>GNIVC Receipts Analytics</h1>
<p>
 <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white" alt="Apache Spark">
  <img src="https://img.shields.io/badge/Apache%20Kafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white" alt="Apache Kafka">
  <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter">
</p>
<p>
A real-time streaming ETL pipeline for processing and aggregating retail receipt data.
</p>
</div>

## 🏗 Architecture
`Apache Kafka (JSON)` ➔ `PySpark Structured Streaming` ➔ `PostgreSQL`

## 🛠 Tech Stack
- **Language:** Python 3.14
- **Data Processing:** Apache Spark 3.5.0 (Structured Streaming)
- **Message Broker:** Apache Kafka 7.5.0 (Zookeeper mode)
- **Database:** PostgreSQL 15
- **Infrastructure:** Docker & Docker Compose

## 📊 Implemented Metrics
The pipeline calculates sliding aggregates within specified time windows (e.g., 1 day):
1. **`total_revenue`** — Total revenue (sum of `CheckSum`)
2. **`receipts_count`** — Number of processed receipts
3. **`avg_check`** — Average transaction check
4. **`unique_clients`** — Number of unique clients (using `approx_count_distinct` on `User`)

## Screenshots

<details>
<img width="1920" height="1080" alt="Spark Streaming Output" src="https://github.com/user-attachments/assets/a2ccaa2f-5df8-4fbe-af84-17e1af9b82a9" />
<img width="1920" height="1080" src="https://github.com/user-attachments/assets/78df7970-a11b-4d78-9b4d-5942f70584af" alt="PostgreSQL Database">
</div>
</details>

## ⚡️ Quick Start

### 0. Clone the repository 
```bash
git clone https://github.com/B1llyWL/gnivc_praktikum.git
cd gnivc_praktikum
```
### 1. Start Infrastructure (1 Terminal)
Ensure Docker is running, then execute:
```bash
docker compose up -d
```
### 2. Initialize Database Table
```bash
docker exec -it postgres psql -U postgres -d gnivc -c "
CREATE TABLE IF NOT EXISTS basic_metrics (
    window_start TIMESTAMP,
    window_end TIMESTAMP,
    total_revenue DOUBLE PRECISION,
    receipts_count BIGINT,
    avg_check DOUBLE PRECISION,
    unique_clients BIGINT
);"
```
### 3. Run the Spark Application
```bash
source venv/bin/activate
jupyter notebook
```

### 4. Test the Pipeline (2 Terminal)
```bash
docker exec -it kafka kafka-console-producer --broker-list localhost:9092 --topic receipts-topic
```

### 5. EXAMPLE
```bash
{"Id": 525242,"User": 2017,"OrganizationId": 104,"Organization":"ООО \"Уильям и Кейт\"","LegalForm":"ООО","InvoiceId": 88301,"InvoiceType": 2,"InvoiceSum": 2450.00,"PaymentType":"CARD","CheckSum": 999.00,"CreatedAt":"2025-08-06T21:00:00+03:00","Items":[{"ItemId": 1,"Name":"Пиво London Pride стаут 0.5л","Price": 420.00,"Quantity": 2.000,"Sum": 840.00,"ProductType":"ALCOHOL"},{"ItemId": 2,"Name":"Традиционный Фиш-энд-Чипс 350г","Price": 290.00,"Quantity": 1.000,"Sum": 290.00,"ProductType":"FOOD"}]}
```

### 6. Verify Results
```bash
docker exec -it postgres psql -U postgres -d gnivc -c "SELECT * FROM basic_metrics;"
```
