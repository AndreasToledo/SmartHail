# Contrato do adaptador meteorológico

## Objetivo

Isolar as particularidades de cada API meteorológica do restante do SmartHail. O adaptador consulta o provedor, valida a resposta e a converte para o formato interno definido em `weather_schema.json`.

## Responsabilidades

- Receber localização e intervalo temporal solicitados.
- Consultar o provedor e respeitar seus limites e requisitos de autenticação.
- Converter unidades e nomes específicos do provedor para os nomes e unidades canônicos do esquema interno.
- Identificar se cada registro é observação ou previsão e preservar os horários fornecidos pela fonte.
- Preservar, quando possível, a resposta original e a identificação do provedor para auditoria.
- Informar falhas de rede, autenticação, limite de requisições e resposta inválida de forma explícita.

## Interface lógica

**Entrada:** localização (latitude e longitude) e janela temporal desejada.

**Saída:** zero ou mais registros normalizados, cada um com origem `weather_api`, localização, horário de registro, tipo (`observation` ou `forecast`) e medidas disponíveis.

Uma resposta vazia não deve ser convertida em risco baixo nem em autorização para acionar o dispositivo. O chamador deve conseguir distinguir resposta vazia de falha de consulta.

## Regras de conversão

- Temperatura em graus Celsius (`temperature_c`).
- Umidade relativa em porcentagem (`relative_humidity_pct`).
- Probabilidade de precipitação em porcentagem (`precipitation_probability_pct`).
- Intensidade de precipitação em milímetros por hora (`precipitation_mm_h`).
- Velocidade do vento em metros por segundo (`wind_speed_m_s`).
- Condição textual é informativa e não substitui medidas numéricas.
- Não inventar ou inferir medidas que o provedor não retornou.

## Decisões pendentes

O provedor, plano de acesso, frequência de consulta, política de cache e estratégia de credenciais serão escolhidos separadamente. Segredos não devem ser armazenados no repositório nem incluídos nos registros.