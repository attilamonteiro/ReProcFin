# ReProcFin

## Proposta: Arquitetura de Extração, Armazenamento, Transformação e Visualização de Dados Financeiros

### Visão Geral
Este projeto visa desenvolver uma arquitetura integrada para **extração, armazenamento, transformação e visualização de dados financeiros**, utilizando tecnologias modernas. O sistema será modular, dividido em serviços independentes que colaboram para:

1. **Extrair dados financeiros de APIs externas**.
2. **Centralizar logs de execução e comunicação**.
3. **Transformar dados em formatos processados e utilizáveis**.
4. **Visualizar informações financeiras de forma interativa**.

#### Mensageria
O **RabbitMQ** será utilizado como **sistema central de mensageria** para garantir a comunicação confiável entre os serviços:
- **Bot de Extração** envia mensagens de logs e confirmações de novos dados extraídos.
- **Transformação de Dados** escuta mensagens para processar dados recém-extraídos.
- **Logs Centralizados** mantêm o histórico de execução para monitoramento e auditoria.

---

### 2. **Transformação de Dados**
- **Responsabilidade**: Verificar no serviço de **Logs** se há novos dados e processá-los em formato utilizável.
- **Fluxo de Operação**:
  1. **Monitoramento de Logs**: Verifica o log centralizado para identificar novos dados extraídos.
  2. **Processamento**:
     - Limpeza de dados brutos.
     - Normalização e agregação.
     - Cálculo de métricas e indicadores (ex.: médias, tendências, variações percentuais).
  3. **Armazenamento**: Salva os dados processados em uma camada específica no banco de dados.
- **Exemplo de Saída**:
  - Dados brutos:
    ```json
    {
      "indice": "IBOVESPA",
      "valor": 100000,
      "data": "2024-11-24T00:00:00Z"
    }
    ```
  - Dados transformados:
    ```json
    {
      "indice": "IBOVESPA",
      "variacao_percentual": 1.2,
      "media_mensal": 101200,
      "mes": "11/2024"
    }
    ```

---

### 3. **Armazenamento de Dados**
- **Responsabilidade**: Gerenciar as camadas de dados brutos e processados.
- **Banco de Dados**:
  - **MongoDB**:
    - Camada de **dados brutos** para armazenar informações diretamente extraídas.
    - Camada de **dados processados** para os resultados das transformações.
- **Modelo de Dados Exemplo**:
  - Brutos:
    ```json
    {
      "indice": "DOW JONES",
      "valor": 35000,
      "data": "2024-11-24T10:00:00Z"
    }
    ```
  - Processados:
    ```json
    {
      "indice": "DOW JONES",
      "media_diaria": 34980,
      "variacao": 0.57
    }
    ```

---

### 4. **Logs Centralizados**
- **Responsabilidade**: Registrar logs de extração e transformação, além de facilitar a verificação de novos dados.
- **Fluxo**:
  - O **Bot de Extração** envia logs sobre as operações realizadas (dados extraídos, status, erros).
  - A **Transformação** consulta os logs para identificar novos dados disponíveis para processamento.
- **Mensageria**: **RabbitMQ** será utilizado como sistema de mensageria confiável.

---

### 5. **Visualização de Dados**
- **Responsabilidade**: Exibir os dados processados em painéis interativos para análise e tomada de decisão.
- **Painel**:
  - Gráficos dinâmicos (linha, barra) para monitorar tendências.
  - Tabelas para exibir valores agregados e métricas.
- **Fluxo**:
  - O painel consome os dados transformados diretamente do banco de dados.
- **Tecnologia**: **Next.js** para criação de interfaces rápidas e modernas.

---

## Estrutura de Diretórios do Repositório

A arquitetura será organizada com uma pasta para cada serviço principal:

```plaintext
project/
│
├── extraction/          # Bot de Extração
│   ├── src/             # Código principal do bot
│   └── utils/           # Funções auxiliares
│
├── transformation/      # Serviço de Transformação
│   ├── src/             # Código principal do serviço
│   └── processors/      # Funções para transformação dos dados
│
├── logs/                # Centralização de Logs
│   ├── src/             # Código principal para registro e consumo de logs
│   └── db/              # Configuração do banco de dados para logs
│
├── visualization/       # Visualização de Dados
│   ├── frontend/        # Aplicação Next.js
│   │   └── pages/       # Páginas do painel
│   └── backend/         # API para comunicação com o banco de dados
│
└── db/                  # Banco de Dados
    ├── mongo-config.js  # Configuração inicial do MongoDB
    └── schemas/         # Schemas de dados brutos e processados

![image](https://github.com/user-attachments/assets/d36af3c3-ac1f-493f-b452-b726b8f2dfb9)
