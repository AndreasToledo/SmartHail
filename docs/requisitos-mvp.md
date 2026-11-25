# Requisitos do MVP

> Escopo inicial: validar o fluxo ponta a ponta com sensores e dispositivo simulados, mantendo contratos que permitam trocar os simuladores por integrações reais.

## Componentes do MVP

- Um produtor de leituras locais simuladas, com cenários repetíveis.
- Uma entrada para dados meteorológicos. A integração real com provedor será adicionada por adaptador; testes podem usar respostas controladas.
- Validação e normalização das entradas para um contrato interno comum.
- Uma avaliação de risco inicial, determinística e explicável.
- Uma decisão separada da avaliação, com condições de segurança explícitas.
- Um simulador de dispositivo que recebe comandos e devolve confirmações e estado.
- Registro correlacionado dos dados, avaliações, decisões, comandos, respostas e erros.

## Requisitos funcionais

1. Cada leitura deve indicar origem, horário e localização, além das medidas disponíveis.
2. Observações locais e previsões devem permanecer identificáveis como tipos de dado distintos.
3. O sistema deve rejeitar ou sinalizar dados inválidos, incompletos para a operação ou desatualizados.
4. Uma avaliação deve apontar quais registros de entrada foram usados e qual versão da regra/modelo foi aplicada.
5. Uma decisão deve registrar sua justificativa e não deve ser confundida com a avaliação de risco.
6. Um comando deve ter identificador único; a resposta deve referenciar esse comando e informar o resultado.
7. Uma falha ou ausência de confirmação deve ser registrada como tal, sem ser apresentada como ativação concluída.
8. Deve ser possível executar o mesmo cenário novamente e comparar os resultados.

## Critérios de aceite

- Cenário de baixo risco: entradas e avaliação são registradas, sem comando de ativação.
- Cenário de risco elevado: a decisão segue a regra configurada e, se autorizada, envia um comando ao simulador.
- Dados ausentes, inválidos ou antigos: o sistema sinaliza indisponibilidade e não ativa por padrão.
- Falha simulada do dispositivo: o comando permanece distinguível de uma ativação concluída e a falha fica registrada.
- Para cada execução, é possível recuperar entradas, avaliação, decisão, comando e confirmação pelo identificador de correlação.

Os limites de risco, periodicidade de coleta, retenção de dados, provedor meteorológico e tecnologia de armazenamento permanecem decisões em aberto; devem ser definidos com base em requisitos e dados reais, não presumidos pelo MVP.