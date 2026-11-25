# Arquitetura inicial do SmartHail

> Estado: proposta inicial para o MVP. Os contratos podem evoluir conforme os testes com o produto e os dados disponíveis.

## Fluxo principal

```
Sensor simulado ─────┐
API meteorológica ─────> Coleta e normalização ─> Registro
                                                     |
                                                     v
                                             Avaliação de risco
                                                     |
                                                     v
                                                Decisão segura
                                                     │
                                                     v
                                          Adaptador do dispositivo
                                                     │
                                                     v
                                            Simulador de hardware
                                                     │
                                                     v
                                           confirmação/telemetria
                                                     │
                                                     v
                                                  Registro
```

Uma execução deve poder ser rastreada desde os dados de entrada até a decisão e a resposta do dispositivo. A previsão meteorológica e as leituras locais são fontes distintas; não devem ser tratadas como se fossem a mesma medição.

## Componentes e responsabilidades

| Componente | Responsabilidade | Contrato inicial |
|---|---|---|
| Fonte de sensores | Produzir leituras locais. No MVP, será um simulador com cenários reproduzíveis. | Registro normalizado em `API_confs/weather_schema.json`, com origem `sensor`. |
| Adaptador meteorológico | Consultar um provedor e converter sua resposta para o formato interno. | Interface descrita em `API_confs/weather_provider.md`; formato em `API_confs/weather_schema.json`. |
| Coleta e normalização | Validar campos, unidades, horário e localização; preservar a identificação da origem. | Aceita apenas registros válidos e informa falhas de validação explicitamente. |
| Registro | Persistir entradas, avaliações, decisões, comandos, confirmações e erros com identificadores correlacionáveis. | Tecnologia de armazenamento a definir; manter referência ao dado original quando disponível. |
| Avaliação de risco | Analisar dados válidos e produzir nível, escore e evidências. Começa com regras explicáveis; poderá receber uma IA depois. | Definido em `IA/risk_model.md`. |
| Decisão | Converter a avaliação em recomendação/ação e aplicar as regras de segurança. | Separada da análise; uma previsão de risco não aciona o dispositivo por si só. |
| Adaptador do dispositivo | Enviar comandos e interpretar confirmações, sem conhecer a implementação física. | Definido em `IoT_int/device_contract.md`. |
| Simulador de hardware | Simular recebimento, execução, falha e estado do dispositivo. Não representa acionamento físico real. | Mesmo contrato do dispositivo real. |
| Orquestração | Coordenar coleta, validação, registro, avaliação, decisão e eventual comando. | Associar os eventos de uma execução por um identificador de correlação. |

## Regras de separação

- Adaptadores convertem formatos externos; regras de risco não dependem de um provedor meteorológico específico.
- A análise estima o risco. A decisão controla se uma ação pode ocorrer.
- O dispositivo recebe comandos explícitos e devolve confirmação; enviar um comando não significa que a proteção foi ativada.
- Dados inválidos, ausentes ou desatualizados devem produzir um estado explícito de erro/indisponibilidade, nunca uma autorização implícita de ativação.
- A simulação deve permitir repetir cenários e relacionar cada resultado às entradas usadas.

## Registro mínimo por execução

1. Dados recebidos, horário da fonte, horário de recebimento, origem e localização.
2. Referência à resposta original do provedor, quando houver, além do registro normalizado.
3. Versão da regra/modelo e identificadores dos dados usados na avaliação.
4. Avaliação de risco, decisão e justificativa.
5. Comando enviado, confirmação do dispositivo e estado final conhecido.
6. Falhas de coleta, validação, análise, comunicação ou execução.

Os registros devem permitir auditoria e comparação entre previsão, observação e resultado. O formato e a política de retenção ainda serão definidos; não se escolhe aqui uma tecnologia de banco de dados.

## Fora da primeira etapa

- Treinar ou escolher um modelo de IA.
- Definir limites meteorológicos definitivos de ativação.
- Escolher o provedor meteorológico e o banco de dados.
- Controlar hardware físico ou assumir que o simulador comprova a segurança do produto real.