# SmartHail

Sistema inteligente para monitoramento meteorológico e proteção automatizada de veículos contra geada e outras condições climáticas adversas.

## Sobre o projeto

O SmartHail é um projeto de Iniciação Científica que combina **Inteligência Artificial**, **Internet das Coisas (IoT)** e **dados meteorológicos** para ajudar proprietários de veículos a proteger seus carros.

A proposta é desenvolver um sistema capaz de analisar as condições climáticas atuais e previstas, identificar situações de risco e acionar automaticamente produtos de proteção para veículos, como capas infláveis contra geada.

O projeto está relacionado a um produto desenvolvido por uma empresa italiana especializada em capas de proteção automotiva contra geadas.

## Objetivos

- Coletar dados meteorológicos de diferentes fontes.
- Monitorar temperatura, clima e condições atmosféricas.
- Consultar dados históricos e previsões meteorológicas.
- Estimar a probabilidade de ocorrência de geada e outros eventos relevantes.
- Apoiar a tomada de decisão sobre a proteção do veículo.
- Enviar notificações aos usuários.
- Permitir o acionamento remoto dos produtos de proteção.
- Desenvolver uma solução baseada em hardware e IoT.

## Funcionamento previsto

O sistema deverá seguir um fluxo semelhante ao seguinte:

1. **Coleta de dados**
   - Consulta a APIs meteorológicas.
   - Recebimento de dados de sensores instalados no ambiente ou no veículo.
   - Armazenamento de dados atuais, históricos e previstos.

2. **Análise meteorológica**
   - Avaliação da temperatura e das condições climáticas.
   - Comparação com registros históricos.
   - Análise da previsão do tempo e da probabilidade de geada.

3. **Sistema de decisão**
   - Identificação de situações que possam oferecer risco ao veículo.
   - Definição do momento adequado para recomendar ou ativar a proteção.
   - Possível uso de modelos de Inteligência Artificial para melhorar as previsões e decisões.

4. **Notificação do usuário**
   - Envio de alertas sobre risco meteorológico.
   - Aviso sobre a necessidade de proteger o veículo.
   - Informações sobre o status da capa e do sistema.

5. **Acionamento IoT**
   - Envio de comandos remotos para o hardware.
   - Ativação do mecanismo responsável por inflar ou posicionar a capa.
   - Retorno do status do dispositivo para o sistema.

## Arquitetura inicial

```text
APIs meteorológicas       Sensores IoT
          |                    |
          +--------+-----------+
                   v
          Coleta e armazenamento
                   |
                   v
          Análise de dados e IA
                   |
                   v
             Sistema de decisão
              /              \
             v                v
      Notificação        Comando IoT
        ao usuário             |
                              v
                 Hardware de proteção veicular
```

## Componentes principais

### Dados meteorológicos

O sistema poderá utilizar APIs para obter:

- Temperatura atual e prevista.
- Condições climáticas.
- Umidade e outros dados atmosféricos relevantes.
- Histórico meteorológico de diferentes dias.
- Previsões futuras.
- Probabilidade de ocorrência de geada.

### Inteligência Artificial

A IA poderá ser utilizada para analisar os dados coletados e auxiliar na identificação de padrões meteorológicos. Entre as possibilidades estão:

- Classificação de risco de geada.
- Estimativa da necessidade de ativação da capa.
- Comparação entre previsão e ocorrência real.
- Aprendizado com dados históricos.
- Redução de acionamentos desnecessários.

### Internet das Coisas

A camada IoT será responsável pela comunicação entre o sistema e o hardware instalado no local do veículo. Ela deverá permitir:

- Monitoramento remoto.
- Recebimento de comandos.
- Acionamento da capa de proteção.
- Verificação do estado do dispositivo.
- Comunicação segura entre o servidor e o hardware.

### Hardware

O protótipo deverá utilizar um dispositivo embarcado capaz de receber comandos e controlar o mecanismo da capa. A definição dos componentes dependerá dos requisitos do produto, mas poderá envolver:

- Microcontrolador ou computador embarcado.
- Sensores de temperatura e umidade.
- Módulo de comunicação sem fio.
- Atuadores para inflação ou posicionamento da capa.
- Fonte de energia e sistema de controle.

## Requisitos iniciais

- Cadastro e identificação do veículo e de sua localização.
- Integração com pelo menos uma API meteorológica.
- Armazenamento de dados meteorológicos.
- Serviço de análise e decisão.
- Sistema de notificações.
- Comunicação remota com o dispositivo IoT.
- Controle do hardware de proteção.
- Registro das ações executadas e dos eventos meteorológicos.
- Mecanismos de segurança para evitar acionamentos indevidos.

## Resultados esperados

Espera-se desenvolver um protótipo capaz de:

- Monitorar as condições meteorológicas de uma determinada localização.
- Identificar situações com potencial de formação de geada.
- Alertar o proprietário do veículo.
- Acionar remotamente o sistema de proteção.
- Registrar os dados e as decisões tomadas.
- Servir como base para a evolução do produto comercial.

## Status

Projeto em fase de definição e levantamento de requisitos.

## Observações

A arquitetura, os componentes de hardware, as APIs e os modelos de Inteligência Artificial serão escolhidos após a análise dos requisitos técnicos do produto de proteção veicular e das condições de operação esperadas.
