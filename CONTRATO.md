# Contrato de comunicação - Estufa IoT

Este documento define os tópicos MQTT e as rotas da API usados entre
o ESP32, o Node-RED e a aplicação Flutter.

## Convenções

- Nomes em minúsculas e sem acentos.
- Todas as mensagens são JSON.
- Temperatura em °C; humidade e nível da água em % (0 a 100).

## 1. Tópicos MQTT

### Sensores (ESP32 -> Node-RED)

| Tópico | Exemplo |
|---|---|
| `estufa/sensores/temperatura` | `{"valor": 24.5}` |
| `estufa/sensores/humidade_ar` | `{"valor": 61.2}` |
| `estufa/sensores/humidade_solo` | `{"valor": 43}` |
| `estufa/sensores/nivel_agua` | `{"valor": 78}` |

### Comandos (Node-RED -> ESP32)

| Tópico | Exemplo |
|---|---|
| `estufa/atuadores/janela/set` | `{"posicao": 100}` (0 = fechada, 100 = aberta) |
| `estufa/atuadores/bomba/set` | `{"estado": "on"}` ou `{"estado": "off"}` |

### Confirmações (ESP32 -> Node-RED)

| Tópico | Exemplo |
|---|---|
| `estufa/atuadores/janela/estado` | `{"posicao": 100}` |
| `estufa/atuadores/bomba/estado` | `{"estado": "on"}` |

## 2. API HTTP (App -> Node-RED)

| Método e rota | Descrição | Corpo / resposta |
|---|---|---|
| `GET /api/sensores` | Últimos valores dos sensores | `{"temperatura": 24.5, "humidadeAr": 61.2, "humidadeSolo": 43, "nivelAgua": 78}` |
| `GET /api/atuadores` | Estado dos atuadores | `{"janela": 100, "bomba": "off"}` |
| `POST /api/janela` | Define a posição da janela | `{"posicao": 100}` |
| `POST /api/bomba` | Liga ou desliga a bomba | `{"estado": "on"}` |

## 3. Autenticação (a definir)

Cabeçalho `X-API-Key` em todas as rotas, a acrescentar quando o
acesso externo (Cloudflare Tunnel) estiver ativo.