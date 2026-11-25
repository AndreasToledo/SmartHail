# Contrato da avaliação de risco

## Papel do componente

A avaliação de risco recebe registros meteorológicos normalizados e produz uma estimativa rastreável. Ela não envia comandos ao dispositivo. Uma regra determinística será usada primeiro; um modelo de IA poderá substituir ou complementar a lógica depois de haver dados adequados e avaliação de desempenho.

## Entrada

- Um conjunto de registros válidos conforme `API_confs/weather_schema.json`.
- Localização e instante de avaliação.
- Janela temporal considerada, quando aplicável.
- Identificadores de correlação da execução.

O componente deve distinguir observações de previsões, verificar atualidade e sinalizar entradas insuficientes. Não deve preencher silenciosamente valores ausentes.

## Saída lógica

Cada avaliação deve conter:

| Campo | Significado |
|---|---|
| `assessment_id` | Identificador único da avaliação. |
| `evaluated_at` | Horário UTC da avaliação. |
| `risk_level` | `low`, `moderate`, `high` ou `unknown`. `unknown` representa dados insuficientes ou indisponíveis, não baixo risco. |
| `risk_score` | Número entre 0 e 1 quando calculável; omitido quando o nível for `unknown`. |
| `evidence_record_ids` | IDs dos registros efetivamente utilizados. |
| `method_version` | Identificação da regra ou do modelo e sua versão. |
| `explanation` | Resumo legível dos fatores relevantes e limitações dos dados. |

A política que converte nível de risco em alerta ou ativação pertence ao componente de decisão, não a este contrato.

## Requisitos para evolução da IA

- Registrar entradas/IDs, versão do método, saída e resultado observado para permitir avaliação posterior.
- Separar dados históricos de treino e validação para evitar medir desempenho nos mesmos exemplos usados no ajuste.
- Comparar previsões com ocorrências observadas e documentar métricas e limitações antes de usar a saída em decisões automáticas.
- Não ativar o dispositivo com base em uma saída sem dados suficientes ou sem uma política de decisão explícita.

Limiares, conjunto de atributos, algoritmo, métricas-alvo e processo de treinamento permanecem em aberto até a coleta e análise de dados representativos.