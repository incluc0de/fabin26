# API EEG NeuroSky - Documentação das Rotas

## Visão Geral

Esta API foi implementada em n8n para receber leituras EEG, armazenar na collection `eeg` do MongoDB e gerar análises de atenção e engajamento.

## 1. POST /eeg

Endpoint:

```http
POST /webhook/eeg
```

Objetivo: receber uma leitura EEG e armazená-la no MongoDB.

### Resposta

```json
{
  "ok": true,
  "message": "Leitura NeuroSky recebida"
}
```

---

## 2. GET /eeg

Endpoint:

```http
GET /webhook/eeg
```

Objetivo: retornar todos os registros armazenados na collection `eeg`.

---

## 3. POST /eeg/status

Endpoint:

```http
POST /webhook/eeg/status
```

Objetivo: gerar relatório analítico baseado nos registros armazenados.

### Exemplo de requisição

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_092824"
}
```

### Critérios avaliados

- Atenção < 40 por mais de 60 segundos.
- Theta/Beta > 5 por mais de 60 segundos.
- Queda de atenção > 30% em relação à linha de base.

### Índice de foco

```text
IndiceFoco = (Atencao × BetaTotal) / Theta
```

### Níveis de risco

- Baixo
- Leve
- Moderado
- Alto

### Exemplo de resposta

```json
{
  "status": "ok",
  "criteria": {
    "A_low_attention_more_than_60s": true,
    "B_theta_beta_more_than_5_more_than_60s": false,
    "C_attention_drop_more_than_30_percent": true
  },
  "interpretation": {
    "risk_level": "moderado"
  }
}
```

## Campos armazenados na collection eeg

```text
received_at
participant_id
age
gender
session_id
activity
session_start_time
timestamp
device_type
signal_quality
poorSignalLevel
attention
meditation
blinkStrength
delta
theta
lowAlpha
highAlpha
lowBeta
highBeta
lowGamma
midGamma
theta_beta_ratio
alpha_total
beta_total
gamma_total
```
