# Contrato de comunicação - Estufa IoT

## Convenções

- Nomes em minúsculas e sem acentos nos tópicos.
- Todas as mensagens são JSON.
- Temperatura em °C; humidade e chuva em % (0 a 100).

## 1. Tópicos MQTT

### Sensores (ESP32 -> Node-RED)

| Tópico | Exemplo |
|---|---|
| `estufa/sensores/temperatura_interior` | `{"valor": 24.5}` |
| `estufa/sensores/temperatura_exterior` | `{"valor": 18.2}` |
| `estufa/sensores/humidade_ar` | `{"valor": 61.2}` |
| `estufa/sensores/humidade_solo` | `{"valor": 43}` |
| `estufa/sensores/chuva` | `{"valor": 0}` (0 = seco, 100 = chuva forte) |
| `estufa/sensores/nivel_tanque` | `{"valor": 78} (0 = vazio, 100 = cheio)` |

### Comandos (Node-RED -> ESP32)

| Tópico | Exemplo |
|---|---|
| `estufa/atuadores/bomba/set` | `{"estado": "on"}` ou `{"estado": "off"}` |
| `estufa/atuadores/ventoinha/set` | `{"estado": "on"}` ou `{"estado": "off"}` |
| `estufa/atuadores/deposito/set` | `{"estado": "aberto"}` ou `{"estado": "fechado"}` |

### Confirmações (ESP32 -> Node-RED)

| Tópico | Exemplo |
|---|---|
| `estufa/atuadores/bomba/estado` | `{"estado": "on"}` |
| `estufa/atuadores/ventoinha/estado` | `{"estado": "off"}` |
| `estufa/atuadores/deposito/estado` | `{"estado": "fechado"}` |

## 2. API HTTP (App -> Node-RED)

| Método e rota | Descrição | Corpo / resposta |
|---|---|---|
| `GET /api/sensores` | Últimos valores dos sensores | `{"temperaturaInterior": 24.5, "temperaturaExterior": 18.2, "humidadeAr": 61.2, "humidadeSolo": 43, "chuva": 0, "nivelTanque" = 78}` |
| `GET /api/atuadores` | Estado dos atuadores | `{"bomba": "off", "ventoinha": "off", "deposito": "fechado"}` |
| `POST /api/bomba` | Liga/desliga a bomba | `{"estado": "on"}` |
| `POST /api/ventoinha` | Liga/desliga a ventoinha | `{"estado": "on"}` |
| `POST /api/deposito` | Abre/fecha o depósito | `{"estado": "aberto"}` |
| `GET /api/modo` | Modo atual | `{"modo": "auto"}` |
| `POST /api/modo` | Muda o modo | `{"modo": "manual"}` (ou `"auto"`) |

## 3. Autenticação (a definir)

Cabeçalho `X-API-Key` em todas as rotas, a acrescentar quando houver acesso externo.