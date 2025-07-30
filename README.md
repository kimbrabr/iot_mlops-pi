# IoT-MLops «Кибернетика»

🌡️ IoT-система, предсказывающая температуру с помощью ONNX-модели, визуализирует данные в Grafana и присылает Telegram-оповещения.

## 📌 Цель

Собрать данные с датчика DHT22, предсказывать температуру на 10 минут вперёд с помощью ML-модели, хранить и визуализировать данные, а также оповещать при сбоях.

## ⚙️ Архитектура

DHT22 → Python (collector.py) → InfluxDB → Python (predictor.py, ONNX) → InfluxDB → Grafana → Telegram

## 🧱 Стек

- Python 3 (сбор данных и ML-инференс)
- InfluxDB (временные ряды)
- ONNX (линейная регрессия, <100KB)
- Grafana (дашборд)
- Prometheus + AlertManager (алерты)
- Telegram-бот
- Helm + Argo CD (деплой)

## 📁 Структура проекта


## ✅ Acceptance-критерии

- Дашборд доступен: `http://<pi>:3000/d/iot`
- Telegram-бот отправляет сообщение `MAE ok`
- `python3 collector.py` стабильно читает с GPIO PA0 раз в минуту
- `influx query "from(bucket:\"iot\")"` возвращает данные с `retention: 7 дней`
- `model.onnx` < 100 КБ, `pytest` показывает `MAE < 1°C`
- Предиктор пишет `pred_temp` обратно в InfluxDB
- Telegram-алерт при `MAE > 1°C` или отсутствии данных >5 минут
- Helm-чарт `iot-ml` синхронизируется через Argo CD (`argocd app list → Synced`)

## 🔍 Команды

Сбор данных:
```bash
python3 collector.py
