# Telemetria de Interações do Desafio

Este documento apresenta exemplos de payloads JSON para envio ao webhook responsável por registrar eventos comportamentais do aluno durante a execução de um desafio. Esses dados devem ser armazenados na collection `interaction_events` do MongoDB e posteriormente sincronizados com a collection `eeg` por meio de `participant_id`, `session_id` e `timestamp` gerado no servidor n8n.

> Importante: o campo `timestamp` **não deve ser enviado pelo cliente**. Ele deve ser gerado no node `Code` do n8n usando `new Date().toISOString()`, para evitar problemas de sincronização entre a máquina do aluno e o servidor.

---

## Estrutura geral do payload

Todos os eventos devem seguir a mesma estrutura base:

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "answer_submitted",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q003"
  },
  "interaction": {
    "action": "submit_answer",
    "value": "B",
    "is_correct": false,
    "attempt_number": 2,
    "response_time_ms": 8400,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": null,
    "keyboard_event": null,
    "target_element": "answer_option_B",
    "x": null,
    "y": null,
    "text_length": null,
    "score": null,
    "error_type": "conceitual",
    "elapsed_time_ms": 125000
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "medium"
  }
}
```

---

## Campos padronizados de interaction

| Campo | Tipo sugerido | Descrição |
|---|---:|---|
| `action` | string | Ação executada pelo usuário ou pelo sistema. |
| `value` | string/null | Valor associado à ação, como alternativa escolhida ou texto resumido. |
| `is_correct` | boolean/null | Indica se a resposta está correta. |
| `attempt_number` | number/null | Número da tentativa na questão ou desafio. |
| `response_time_ms` | number/null | Tempo de resposta da questão em milissegundos. |
| `idle_seconds` | number/null | Tempo de inatividade detectado. |
| `hint_used` | boolean | Indica se o aluno utilizou dica. |
| `hint_id` | string/null | Identificador da dica utilizada. |
| `mouse_event` | string/null | Tipo de evento de mouse: `click`, `move`, `hover`, etc. |
| `keyboard_event` | string/null | Tipo de evento de teclado: `keydown`, `keyup`, `typing`, etc. |
| `target_element` | string/null | Elemento da interface relacionado à ação. |
| `x` | number/null | Coordenada X do mouse, quando aplicável. |
| `y` | number/null | Coordenada Y do mouse, quando aplicável. |
| `text_length` | number/null | Tamanho do texto digitado, quando aplicável. |
| `score` | number/null | Pontuação parcial ou final. |
| `error_type` | string/null | Tipo de erro identificado: `conceitual`, `atencao`, `procedural`, etc. |
| `elapsed_time_ms` | number/null | Tempo transcorrido desde o início do desafio. |

---

# Exemplos de eventos

## 1. `challenge_started`

Evento usado para registrar o início real da atividade.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "challenge_started",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": null,
    "question_id": null
  },
  "interaction": {
    "action": "start_challenge",
    "value": null,
    "is_correct": null,
    "attempt_number": null,
    "response_time_ms": null,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": "click",
    "keyboard_event": null,
    "target_element": "btn_start_challenge",
    "x": 612,
    "y": 438,
    "text_length": null,
    "score": null,
    "error_type": null,
    "elapsed_time_ms": 0
  },
  "context": {
    "screen": "start_screen",
    "difficulty": "medium"
  }
}
```

---

## 2. `question_viewed`

Evento usado quando uma questão é exibida ao aluno.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "question_viewed",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q001"
  },
  "interaction": {
    "action": "view_question",
    "value": null,
    "is_correct": null,
    "attempt_number": 1,
    "response_time_ms": null,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": null,
    "keyboard_event": null,
    "target_element": "question_panel",
    "x": null,
    "y": null,
    "text_length": null,
    "score": null,
    "error_type": null,
    "elapsed_time_ms": 12000
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "medium"
  }
}
```

---

## 3. `answer_started`

Evento usado quando o aluno inicia uma resposta.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "answer_started",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q001"
  },
  "interaction": {
    "action": "start_answer",
    "value": null,
    "is_correct": null,
    "attempt_number": 1,
    "response_time_ms": null,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": "click",
    "keyboard_event": null,
    "target_element": "answer_option_A",
    "x": 490,
    "y": 512,
    "text_length": null,
    "score": null,
    "error_type": null,
    "elapsed_time_ms": 22000
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "medium"
  }
}
```

---

## 4. `answer_submitted`

Evento usado quando o aluno envia uma resposta.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "answer_submitted",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q001"
  },
  "interaction": {
    "action": "submit_answer",
    "value": "B",
    "is_correct": false,
    "attempt_number": 1,
    "response_time_ms": 8400,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": "click",
    "keyboard_event": null,
    "target_element": "btn_submit_answer",
    "x": 740,
    "y": 610,
    "text_length": null,
    "score": null,
    "error_type": "conceitual",
    "elapsed_time_ms": 30400
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "medium"
  }
}
```

---

## 5. `correct_answer`

Evento usado quando o sistema identifica que a resposta enviada está correta.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "correct_answer",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q002"
  },
  "interaction": {
    "action": "evaluate_answer",
    "value": "C",
    "is_correct": true,
    "attempt_number": 1,
    "response_time_ms": 6200,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": null,
    "keyboard_event": null,
    "target_element": "system_evaluation",
    "x": null,
    "y": null,
    "text_length": null,
    "score": 10,
    "error_type": null,
    "elapsed_time_ms": 78200
  },
  "context": {
    "screen": "feedback_screen",
    "difficulty": "medium"
  }
}
```

---

## 6. `wrong_answer`

Evento usado quando o sistema identifica que a resposta enviada está incorreta.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "wrong_answer",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q003"
  },
  "interaction": {
    "action": "evaluate_answer",
    "value": "B",
    "is_correct": false,
    "attempt_number": 2,
    "response_time_ms": 8400,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": null,
    "keyboard_event": null,
    "target_element": "system_evaluation",
    "x": null,
    "y": null,
    "text_length": null,
    "score": 0,
    "error_type": "conceitual",
    "elapsed_time_ms": 125000
  },
  "context": {
    "screen": "feedback_screen",
    "difficulty": "medium"
  }
}
```

---

## 7. `hint_requested`

Evento usado quando o aluno solicita uma dica.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "hint_requested",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q003"
  },
  "interaction": {
    "action": "request_hint",
    "value": null,
    "is_correct": null,
    "attempt_number": 2,
    "response_time_ms": null,
    "idle_seconds": null,
    "hint_used": true,
    "hint_id": "hint_q003_01",
    "mouse_event": "click",
    "keyboard_event": null,
    "target_element": "btn_hint",
    "x": 856,
    "y": 214,
    "text_length": null,
    "score": null,
    "error_type": null,
    "elapsed_time_ms": 113000
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "medium"
  }
}
```

---

## 8. `idle_detected`

Evento usado quando o sistema detecta ausência de interação por determinado tempo.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "idle_detected",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q003"
  },
  "interaction": {
    "action": "idle",
    "value": null,
    "is_correct": null,
    "attempt_number": 2,
    "response_time_ms": null,
    "idle_seconds": 45,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": null,
    "keyboard_event": null,
    "target_element": null,
    "x": null,
    "y": null,
    "text_length": null,
    "score": null,
    "error_type": null,
    "elapsed_time_ms": 180000
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "medium"
  }
}
```

---

## 9. `mouse_activity`

Evento usado para registrar atividade de mouse. Recomenda-se não enviar cada movimento bruto, mas sim eventos agregados ou relevantes.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "mouse_activity",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q003"
  },
  "interaction": {
    "action": "mouse_interaction",
    "value": null,
    "is_correct": null,
    "attempt_number": 2,
    "response_time_ms": null,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": "click",
    "keyboard_event": null,
    "target_element": "answer_option_B",
    "x": 532,
    "y": 487,
    "text_length": null,
    "score": null,
    "error_type": null,
    "elapsed_time_ms": 121500
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "medium"
  }
}
```

---

## 10. `keyboard_activity`

Evento usado para registrar atividade de teclado. Por privacidade, recomenda-se registrar apenas metadados, como tamanho do texto ou quantidade de teclas, e não o texto completo digitado.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "keyboard_activity",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_01",
    "question_id": "q004"
  },
  "interaction": {
    "action": "keyboard_interaction",
    "value": null,
    "is_correct": null,
    "attempt_number": 1,
    "response_time_ms": null,
    "idle_seconds": null,
    "hint_used": false,
    "hint_id": null,
    "mouse_event": null,
    "keyboard_event": "typing",
    "target_element": "answer_textarea",
    "x": null,
    "y": null,
    "text_length": 128,
    "score": null,
    "error_type": null,
    "elapsed_time_ms": 205000
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "hard"
  }
}
```

---

## 11. `challenge_completed`

Evento usado para registrar a finalização do desafio.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "challenge_completed",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "final",
    "question_id": null
  },
  "interaction": {
    "action": "finish_challenge",
    "value": "completed",
    "is_correct": null,
    "attempt_number": null,
    "response_time_ms": null,
    "idle_seconds": null,
    "hint_used": true,
    "hint_id": null,
    "mouse_event": "click",
    "keyboard_event": null,
    "target_element": "btn_finish_challenge",
    "x": 698,
    "y": 642,
    "text_length": null,
    "score": 70,
    "error_type": null,
    "elapsed_time_ms": 420000
  },
  "context": {
    "screen": "completion_screen",
    "difficulty": "medium"
  }
}
```

---

## 12. `challenge_abandoned`

Evento usado para registrar abandono, fechamento, desistência ou saída antecipada da atividade.

```json
{
  "participant_id": "v0001",
  "session_id": "20260602_095420",
  "activity": "desafio_atencao",
  "event_type": "challenge_abandoned",
  "challenge": {
    "challenge_id": "desafio_001",
    "step_id": "fase_02",
    "question_id": "q005"
  },
  "interaction": {
    "action": "abandon_challenge",
    "value": "user_exit",
    "is_correct": null,
    "attempt_number": 1,
    "response_time_ms": null,
    "idle_seconds": 90,
    "hint_used": true,
    "hint_id": "hint_q005_01",
    "mouse_event": "click",
    "keyboard_event": null,
    "target_element": "btn_exit_challenge",
    "x": 922,
    "y": 110,
    "text_length": null,
    "score": 40,
    "error_type": null,
    "elapsed_time_ms": 310000
  },
  "context": {
    "screen": "question_screen",
    "difficulty": "hard"
  }
}
```

---

# Código JavaScript sugerido para o node Code do n8n

Este código deve ficar após o Webhook e antes do node MongoDB. Ele gera o `timestamp` no servidor e normaliza os campos para armazenamento.

```javascript
const body = $json.body ?? $json;

const interaction = body.interaction ?? {};
const challenge = body.challenge ?? {};
const context = body.context ?? {};

return [
  {
    json: {
      timestamp: new Date().toISOString(),

      participant_id: body.participant_id ?? null,
      session_id: body.session_id ?? null,
      activity: body.activity ?? null,
      event_type: body.event_type ?? null,

      challenge_id: challenge.challenge_id ?? null,
      step_id: challenge.step_id ?? null,
      question_id: challenge.question_id ?? null,

      action: interaction.action ?? null,
      value: interaction.value ?? null,
      is_correct: interaction.is_correct ?? null,
      attempt_number: interaction.attempt_number ?? null,
      response_time_ms: interaction.response_time_ms ?? null,
      idle_seconds: interaction.idle_seconds ?? null,
      hint_used: interaction.hint_used ?? false,
      hint_id: interaction.hint_id ?? null,
      mouse_event: interaction.mouse_event ?? null,
      keyboard_event: interaction.keyboard_event ?? null,
      target_element: interaction.target_element ?? null,
      x: interaction.x ?? null,
      y: interaction.y ?? null,
      text_length: interaction.text_length ?? null,
      score: interaction.score ?? null,
      error_type: interaction.error_type ?? null,
      elapsed_time_ms: interaction.elapsed_time_ms ?? null,

      screen: context.screen ?? null,
      difficulty: context.difficulty ?? null,

      raw_event: body
    }
  }
];
```

---

# Fields sugeridos para o node MongoDB

Collection sugerida:

```text
interaction_events
```

Fields:

```text
timestamp, participant_id, session_id, activity, event_type, challenge_id, step_id, question_id, action, value, is_correct, attempt_number, response_time_ms, idle_seconds, hint_used, hint_id, mouse_event, keyboard_event, target_element, x, y, text_length, score, error_type, elapsed_time_ms, screen, difficulty, raw_event
```

---

# Recomendação para sincronização com EEG

Como a collection `eeg` já está em produção, recomenda-se manter sua estrutura atual e sincronizar os dados usando:

```text
participant_id
session_id
timestamp
```

A análise integrada poderá buscar os eventos comportamentais mais próximos de cada leitura EEG, por exemplo, usando uma janela de tempo de 5, 10, 30 ou 60 segundos.

Exemplo conceitual:

```text
Para cada leitura EEG:
  buscar eventos de interaction_events
  com mesmo participant_id e session_id
  entre timestamp - 30s e timestamp + 30s
```

Isso permitirá identificar situações como:

- EEG indica atenção alta, mas não houve interação com a tarefa;
- EEG indica queda de atenção junto com aumento de inatividade;
- EEG indica esforço cognitivo e há aumento de erros;
- EEG indica possível dispersão após repetidas tentativas incorretas;
- EEG indica recuperação de foco após uma dica ou intervenção.
