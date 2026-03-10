# 📈 Challenge Telecom X - Análise de Churn de Clientes

##  Sobre o Projeto
Este projeto tem como objetivo analisar os fatores associados ao cancelamento (churn) de clientes da TelecomX. Por meio de uma análise detalhada dos dados, foram identificados padrões relevantes relacionados a características demográficas, tipos de contratos e métodos de pagamento, que podem influenciar a decisão dos clientes de encerrar o serviço.
---

# Relatório Detalhado de Análise de Churn de Clientes

## 1. Introdução
Este relatório apresenta uma análise detalhada da taxa de *churn* (evasão de clientes) de uma empresa de telecomunicações, baseando-se no conjunto de dados `TelecomX_Data.json`. O objetivo principal é identificar os principais fatores que contribuem para a evasão de clientes, fornecendo *insights* acionáveis e recomendações para a retenção de clientes.

## 2. Limpeza e Tratamento de Dados
A fase de limpeza e tratamento de dados envolveu as seguintes etapas:
*   **Extração e Normalização**: Os dados, inicialmente em formato JSON aninhado, foram extraídos e normalizados em um único DataFrame (`df_normalized`) para facilitar a análise. Colunas aninhadas como 'customer', 'phone', 'internet' e 'account' foram expandidas.
*   **Verificação de Valores Nulos e Duplicados**: Foi verificado que não havia valores nulos evidentes inicialmente, nem linhas duplicadas. No entanto, uma verificação mais aprofundada revelou:
    *   11 valores em branco na coluna `Charges.Total`.
    *   224 valores em branco na coluna `Churn`.
*   **Tratamento de Tipos de Dados**:
    *   A coluna `Charges.Total` foi identificada como tipo 'object' e convertida para 'float'. Os valores em branco que resultaram em `NaN` durante a conversão foram preenchidos com a mediana da coluna para evitar distorções por valores extremos.
    *   Os valores em branco na coluna `Churn` foram substituídos por `NaN` e, posteriormente, preenchidos com a moda da coluna ('Não'), considerando que a maioria dos clientes não realiza *churn*. A coluna `Churn` foi convertida para tipo 'category'.
*   **Criação de Novas Features**: Uma nova coluna, `Contas_Diarias`, foi criada dividindo `Cobranca_Mensal` pela média de dias em um mês contando com o ano bissexto a cada quatro anos (30.4375) para uma perspectiva diária de gastos.
*   **Renomeação e Padronização de Colunas e Valores**: As colunas foram renomeadas para termos mais claros e em português (ex: `customerID` para `ID`, `gender` para `Genero`). Além disso, os valores nas colunas categóricas foram padronizados para termos em português (ex: 'Yes' para 'Sim', 'No' para 'Não', 'Female' para 'Feminino', 'Male' para 'Masculino').

## 3. Análise Exploratória de Dados
A análise exploratória revelou a distribuição geral do *churn* e a relação com diversas variáveis categóricas e numéricas.

### 3.1. Distribuição Geral de Churn
A análise inicial da coluna `Churn` mostrou que:
*   **5398 clientes** (74.28%) não realizaram *churn*.
*   **1869 clientes** (25.72%) realizaram *churn*.
Esta proporção indica que a evasão é um problema significativo, afetando aproximadamente um quarto da base de clientes. A visualização desta distribuição foi feita através de um gráfico de barras.
<img src="images/distribuicao de churn por cliente.png" width="500">

### 3.2. Churn por Variáveis Categóricas
A análise de *churn* por categorias chave destacou os seguintes pontos principais:<br>
*  **Idoso_60+**: Clientes idosos (`Idoso_60+ = Sim`) têm uma taxa de churn significativamente maior (40.27%) em comparação com clientes não idosos (22.89%).
*  **Possui_Parceiro/Possui_Dependentes**: Clientes sem parceiro (32.01%) e sem dependentes (30.34%) apresentam taxas de churn mais altas.
*  **Servico_Internet**: Clientes com serviço de internet "Fibra optica" têm a maior taxa de churn (40.56%), enquanto aqueles sem serviço de internet ('Não') têm a menor (7.15%).
*  **Serviços Adicionais (OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport)**: Clientes que *não* possuem serviços adicionais mostram taxas de *churn* mais elevadas. Por outro lado, aqueles que não têm serviço de internet ('Sem Servico de Internet') para estes itens têm uma taxa de churn muito menor, e clientes que *possuem* esses serviços têm taxas menores que a média global.
*  **Contrato**: Clientes com contrato "Mensal" têm uma taxa de *churn* alarmantemente alta (41.32%) em comparação com contratos de "Um Ano" (10.93%) e "Dois Anos" (2.75%).
*  **Fatura_Digital**: Clientes que optam pela "Fatura_Digital" (32.48%) têm uma taxa de *churn* maior do que aqueles que não a utilizam (15.87%).
*  **Metodo_Pagamento**: O método de pagamento "Cheque eletrônico" está associado à maior taxa de *churn* (43.80%).

### 3.3. Churn por Variáveis Numéricas
A análise das variáveis numéricas (`Tempo_Contrato`, `Cobranca_Mensal`, `Cobranca_Total`, `Contas_Diarias`) em relação ao *churn* forneceu *insights* valiosos.

#### Principais Insights de Churn a partir das Variáveis Numéricas
*   **Tempo_Contrato**: Clientes que permanecem ativos mantêm relacionamentos mais longos com a empresa (média de 37 meses), enquanto clientes que churnaram apresentam tempo médio de contrato bem menor (18 meses), indicando que os primeiros meses são críticos para retenção.<br><img src="images/Tendência de Churn por Tempo de Contrato.png" width="1000">
*   **Cobranca_Mensal**: Clientes que cancelaram o serviço apresentam cobranças mensais médias mais altas (74,44) do que clientes que permaneceram (61,35), o que pode indicar maior sensibilidade ao preço ou menor percepção de custo-benefício entre esses usuários.<br><img src="images/distribuicao de cobranca mensal por churn.png" width="500">
*   **Cobranca_Total**: Observa-se que clientes que não cancelaram o serviço acumulam uma cobrança total média significativamente mais alta (2538,10) do que clientes que churnaram (1531,80), refletindo um relacionamento mais duradouro com a empresa.<br><img src="images/distribuicao de cobranca total por churn.png" width="500">
*   **Contas_Diarias**: Assim como observado na **Cobranca_Mensal**, as **Contas_Diarias** também apresentam valores ligeiramente maiores entre clientes que cancelaram o serviço (*churn*). Em média, clientes que churnaram possuem **2,45** de contas diárias, enquanto clientes que permaneceram apresentam média de **2,02**.<br><img src="images/distribuicao de contas diarias por churn.png" width="500">

**Em resumo:** clientes que cancelam o serviço (churn) tendem a apresentar menor tempo de permanência na empresa, enquanto enfrentam cobranças mensais e diárias ligeiramente mais altas. Por outro lado, clientes que permanecem ativos mantêm um relacionamento mais duradouro e, como consequência, acumulam valores totais significativamente maiores ao longo do tempo.
Esse padrão sugere que fatores relacionados ao percepção de custo-benefício dos serviços mensais e à duração do relacionamento com a empresa são fortes indicadores associados ao churn.

## 4. Conclusões e Insights
A análise revelou que o perfil do cliente propenso ao churn é multifatorial e relativamente complexo. Ainda assim, alguns padrões consistentes foram identificados a partir dos dados:
* Duração do Contrato: Clientes com contratos mensais (Month-to-Month) apresentam probabilidade significativamente maior de cancelamento. Em contrapartida, contratos de maior duração demonstram forte associação com a retenção de clientes.
* Serviços de Internet e Serviços Adicionais: O serviço de Fibra Óptica está associado a taxas mais elevadas de churn. Além disso, a ausência de serviços complementares — como Segurança Online, Backup, Proteção de Dispositivo e Suporte Técnico — aumenta consideravelmente a probabilidade de cancelamento.
* Custos Mensais: Clientes com cobranças mensais mais altas apresentam uma leve tendência maior ao cancelamento, o que pode indicar sensibilidade ao preço ou menor percepção de valor do serviço.
* Método de Pagamento: O método Cheque Eletrônico aparece com maior frequência entre clientes que cancelam o serviço, sugerindo uma possível relação entre esse formato de pagamento e um perfil de cliente com maior risco de evasão.
* Perfil Demográfico: Clientes idosos, sem parceiros e sem dependentes demonstram maior propensão ao churn, indicando que fatores demográficos também podem influenciar o comportamento de cancelamento.

## 5. Recomendações
Com base nos insights obtidos durante a análise, foram elaboradas as seguintes recomendações estratégicas para reduzir a taxa de churn e fortalecer a retenção de clientes:

1.  **Incentivar Contratos de Longo Prazo**:
    * Oferecer descontos progressivos ou benefícios exclusivos para clientes que optarem por contratos de 1 ou 2 anos.
    * Implementar programas de fidelidade que recompensem a permanência e o relacionamento contínuo com a empresa.
    * Comunicar de forma clara os benefícios financeiros e operacionais de contratos mais longos, reforçando a economia a longo prazo.

2.  **Fortalecer a Proposta de Valor da Fibra Óptica**:
    * Investigar os fatores específicos de churn entre clientes de fibra óptica, analisando aspectos como qualidade do serviço, suporte técnico e percepção de preço.
    * Desenvolver pacotes mais competitivos, incluindo benefícios adicionais ou serviços complementares que aumentem a percepção de valor do cliente.
 
3.  **Promover e Integrar Serviços de Segurança e Suporte**:
    * Desenvolver campanhas de comunicação e marketing destacando os benefícios de serviços como Segurança Online, Backup, Proteção de Dispositivo e Suporte Técnico.
    * Avaliar a inclusão de serviços básicos de segurança ou suporte técnico como parte de pacotes de internet, aumentando a atratividade da oferta.

4.  **Otimizar a Estratégia de Preços e Pacotes**:
    * Reavaliar a estrutura de preços, especialmente para planos com maior cobrança mensal, buscando melhorar o equilíbrio entre preço e valor percebido.
    * Oferecer pacotes mais flexíveis ou personalizados, adaptados às diferentes necessidades dos clientes.

5.  **Monitorar Clientes que Utilizam "Cheque Eletrônico"**:
    * Avaliar o processo de pagamento por cheque eletrônico, identificando possíveis dificuldades operacionais ou associações com perfis de maior risco de cancelamento.
    * Incentivar a adoção de métodos de pagamento mais estáveis e automáticos, como débito automático ou cobrança recorrente.

6.  **Desenvolver Programas para Segmentos Específicos de Clientes**:
    * Criar ofertas ou serviços direcionados para segmentos como clientes idosos ou clientes sem dependentes.
    * Oferecer atendimento mais personalizado e suporte proativo para esses grupos, aumentando a satisfação e o engajamento.

Essas recomendações não apenas contribuem para reduzir o churn, mas também ajudam a fortalecer o relacionamento com os clientes, aumentar a percepção de valor dos serviços e promover uma base de clientes mais estável e engajada para a TelecomX.
---

## 🛠️ Tecnologias Utilizadas

* ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) Linguagem principal.
* ![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white) Manipulação de dados.
* ![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white) Cálculos numéricos.
* ![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black) Visualizações estáticas.
* ![Seaborn](https://img.shields.io/badge/Seaborn-4D88FF?style=for-the-badge&logo=python&logoColor=white) Visualizações estatísticas refinadas.
* ![Google Colab](https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white) Ambiente de desenvolvimento.

---

## 📂 Estrutura do Repositório

* `images/`: Gráficos gerados para o relatório.
* `TelecomX_BR.ipynb`: Código completo e análises.
* `TelecomX_Data`: Base de dados [Dados Fictícios]
* `TelecomX_dicionario`: Estrutura dos Dados

---

**Desenvolvido por:** Mayara Martins.<br>
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/MayaraMartins95/)[![LinkedInd](https://img.shields.io/badge/LinkedIn-100000?style=for-the-badge&logo=github&logoColor=white)](https://www.linkedin.com/in/mayara-martins-rp/)
