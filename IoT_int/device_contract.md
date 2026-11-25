# Contrato do dispositivo de proteção

## Objetivo

Definir a comunicação entre o sistema e o dispositivo de proteção, independentemente de o destino ser o simulador ou o hardware real. O simulador deve implementar o mesmo contrato e nunca afirmar que um comando foi concluído sem produzir a respectiva resposta.

## Comando

Campos obrigatórios:

| Campo | Significado |
|---|---|
| `command_id` | Identificador único; permite correlacionar e deduplicar tentativas. |
| `device_id` | Dispositivo de destino. |
| `action` | `deploy`, `retract` ou `get_status`. |
| `issued_at` | Horário UTC de emissão. |
| `expires_at` | Prazo após o qual um comando de atuação não deve mais ser executado. |
| `requested_by` | `system` ou `user`. |
| `reason` | Justificativa legível. |
| `assessment_id` | ID da avaliação associada, quando a origem for uma decisão automática. |

Exemplo:

```json
{
  "command_id": "cmd-00042",
  "device_id": "device-01",
  "action": "deploy",
  "issued_at": "2026-01-15T02:10:00Z",
  "expires_at": "2026-01-15T02:15:00Z",
  "requested_by": "system",
  "reason": "Risco alto conforme política configurada",
  "assessment_id": "risk-00017"
}
```

## Confirmação

Toda resposta deve referenciar `command_id` e `device_id`, registrar `reported_at` em UTC e incluir:

- `status`: `accepted`, `rejected`, `in_progress`, `completed` ou `failed`.
- `device_state`: `deployed`, `retracted`, `deploying`, `retracting`, `unknown` ou `fault`.
- `error_code` e mensagem, quando houver falha ou rejeição.

`accepted` significa apenas que o comando foi recebido. Apenas `completed` acompanhado do estado correspondente indica conclusão reportada pelo dispositivo.

## Regras de segurança e operação

- Comandos repetidos com o mesmo `command_id` não devem causar execuções duplicadas.
- Comandos de atuação expirados devem ser rejeitados.
- O dispositivo deve reportar estado desconhecido/falha quando não puder confirmar a posição da proteção.
- Rejeições, falhas e ausência de resposta são resultados explícitos e devem ser registrados.
- Este contrato não substitui intertravamentos físicos nem validação de segurança do hardware real.

## Simulador

O simulador deve aceitar cenários configuráveis de sucesso, rejeição, atraso e falha, e emitir confirmações no mesmo formato. Ele deve possibilitar testes repetíveis do fluxo, sem representar segurança ou funcionamento do produto físico.