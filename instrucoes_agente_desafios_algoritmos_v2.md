# Instruções do Agente Gerador de Desafios de Algoritmos

## Rotas Disponíveis

### Gerar desafio

**POST**
https://n8n.inlcuc0de.com.br/webhook/desafio/alg

Responsável por gerar um novo desafio, armazená-lo no MongoDB e retornar o documento completo persistido.

### Consultar desafios

**GET**
https://n8n.inlcuc0de.com.br/webhook/desafios/alg

Responsável por consultar os registros armazenados na collection `desafios`.

---

## Fluxo de Geração

POST /desafio/alg
→ Edit Fields
→ AI Agent
→ Prepara Armazenamento
→ MongoDB Collection Desafios
→ Resposta /desafio/alg

---

## Estrutura de Entrada

```json
{
  "sessionId": "algoritmos",
  "tipo": "desafio_algoritmo",
  "topico": "estruturas de repeticao",
  "nivel_complexidade": "basico | intermediario | avancado",
  "linguagem": "pseudocodigo | portugol | python | javascript | java | c",
  "publico_alvo": "descrição do público",
  "contexto_gamificado": "missao | aventura | torneio | investigacao | simulacao",
  "quantidade_partes": 3,
  "tema": "tema do desafio",
  "incluir_solucao": true,
  "incluir_criterios_avaliacao": true
}
```

---

## Estrutura de Resposta

A resposta retornada pelo endpoint não é mais apenas o JSON produzido pelo agente.

O workflow encapsula o desafio gerado em um documento persistido no MongoDB.

```json
{
  "_id": "6a24a12fc2c29c9d0096bb80",
  "created_at": "2026-06-06T22:37:35.474Z",
  "sessionId": "algoritmos",
  "tipo": "desafio_algoritmo",
  "topico": "variaveis",
  "nivel_complexidade": "basico",
  "linguagem": "c",
  "tema": null,
  "contexto_gamificado": null,
  "incluir_solucao": true,
  "incluir_criterios_avaliacao": true,
  "entrada_original": {},
  "desafio_gerado": {}
}
```

---

## Campo entrada_original

Contém exatamente o JSON enviado pelo cliente.

```json
{
  "sessionId": "algoritmos",
  "tipo": "desafio_algoritmo",
  "topico": "variaveis",
  "nivel_complexidade": "basico",
  "linguagem": "c",
  "quantidade_partes": 3,
  "publico_alvo": "alunos TDAH",
  "incluir_solucao": true,
  "incluir_criterios_avaliacao": true
}
```

---

## Campo desafio_gerado

Contém a resposta produzida pelo agente.

```json
{
  "status": "sucesso",
  "tipo": "desafio_algoritmo",
  "titulo_desafio": "",
  "topico": "",
  "nivel_complexidade": "",
  "linguagem": "",
  "publico_alvo": "",
  "contexto_gamificado": "",
  "tema": "",
  "descricao_geral": "",
  "objetivo_pedagogico": "",
  "partes": [],
  "desafio_integrado": "",
  "entrada_completa": "",
  "processamento_completo": "",
  "saida_completa": "",
  "competencias_trabalhadas": [],
  "criterios_avaliacao": [],
  "solucao": ""
}
```

---

## Collection MongoDB

Collection utilizada:

```text
desafios
```

Campos armazenados:

- created_at
- sessionId
- tipo
- topico
- nivel_complexidade
- linguagem
- tema
- contexto_gamificado
- incluir_solucao
- incluir_criterios_avaliacao
- entrada_original
- desafio_gerado

---

## Observação

O documento retornado pelo endpoint é o mesmo documento persistido na collection MongoDB, permitindo auditoria, analytics, histórico de desafios e futuras integrações com EEG e telemetria comportamental.
