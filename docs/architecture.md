# StationQuoin — proposed AWS architecture

StationQuoin uses AWS to collect charger telemetry securely, train forecasting models and serve dashboards to operations teams — scaling from a single site to a national network.

## Request flow

1. Chargers
2. AWS IoT Core
3. AWS Lambda
4. Amazon S3
5. SageMaker AI
6. DynamoDB
7. Ops dashboard

## Services

### AWS IoT Core

**Role:** Collect charger telemetry

Receives session starts, energy readings and status updates from chargers through OCPP gateways, with per-device certificates and secure MQTT connections.

### Amazon SageMaker AI

**Role:** Forecast demand and waiting times

Trains per-location time-series models on session history, weather and calendar effects, and produces hourly and monthly forecasts with confidence intervals.

### Amazon S3

**Role:** Keep the session history

Stores raw and processed session data as the long-term history that models train on, encrypted and partitioned by operator and site.

### Amazon DynamoDB

**Role:** Serve live state and forecasts

Holds current charger status, latest forecasts and scenario results, giving dashboards millisecond reads across thousands of connectors.

### AWS Lambda

**Role:** Process events and alerts

Processes incoming telemetry, detects anomalies and downtime, publishes forecasts and sends peak-hour alerts to operations teams.

## Principles

- **Secure by device** — Every charger connects with its own certificate.
- **Forecasts with ranges** — Every prediction shows its uncertainty.
- **Scale per connector** — Serverless processing grows with your network.

## Safeguards

- **Planners decide** — Forecasts inform investment decisions; your team makes them.
- **Transparent models** — Each forecast shows its range and the factors that shaped it.
- **No control of chargers** — StationQuoin reads telemetry. It does not start, stop or throttle charging.
- **Driver privacy** — Sessions are analysed without storing driver identities.

## MVP scope

Send us 6–12 months of session history for a set of locations. We build per-site demand and waiting-time forecasts and a utilisation dashboard your operations team uses every week.

- Historical session import
- Per-site demand forecasts
- Waiting-time predictions
- Utilisation dashboard
- Weekly operations report
