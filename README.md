# Predição de morte neonatal no Maranhão utilizando Aprendizado de Máquina

## Introdução

A morte neonatal, definida como o óbito que ocorre até os 28 dias de vida, é amplamente reconhecida em muitos países como um importante indicador da qualidade de vida, do desenvolvimento socioeconômico e do acesso da população aos serviços de saúde. Suas causas estão fortemente ligadas à qualidade dos cuidados de saúde prestados durante a gestação, o parto e o período pós-natal. A mortalidade neonatal desempenha um papel significativo nas taxas gerais de mortalidade infantil. De acordo com a Organização Mundial da Saúde (OMS), as mortes neonatais representam 47% dos óbitos em crianças menores de cinco anos. Embora continue a ser um fator central na mortalidade infantil, as taxas globais de morte neonatal diminuíram de 37 por mil nascidos vivos em 2000 para 18 por mil nascidos vivos em 2017. No Brasil, a taxa passou de 26 óbitos por mil nascidos vivos em 1990 para 16 por mil em 2015.

Apesar dessa redução, a morte neonatal permanece um problema crítico, não apenas no Brasil, mas em diversos outros países. Diante desse cenário, a predição de óbitos neonatais, utilizando bases de dados públicas do Brasil e aplicando técnicas de Aprendizado de Máquina, mostra-se altamente relevante para melhorar a saúde pública e reduzir ainda mais essas taxas.

Portanto o objetivo deste trabalho é desenvolver um modelo preditivo de óbitos neonatais utilizando algoritmos de Aprendizado de Máquina.

## Materiais e metodologia

Foram utilizadas duas bases de dados públicas disponibilizadas pelo DataSUS: o Sistema de Informação de Mortalidade ([SIM](https://opendatasus.saude.gov.br/dataset/sim)) e o Sistema de Informação de Nascidos Vivos ([SINASC](https://datasus.saude.gov.br/transferencia-de-arquivos/#)). As bases abrangem os casos de nascimento no Maranhão e os óbitos ocorridos tanto dentro quanto fora do estado, no período de 2017 a 2022. No total, foram analisados 12 arquivos no formato CSV.

Todas as etapas foram implementadas em Python, utilizando a biblioteca `pandas` para manipulação e análise de dados, e o ambiente Jupyter Notebook, que permite a execução do código de forma interativa através do navegador.

Inicialmente, foi realizada a análise, limpeza e normalização dos dados provenientes do SIM e SINASC. A base de dados do SINASC foi utilizada como principal fonte de informações sobre os nascimentos, enquanto o SIM serviu para adicionar uma nova variável chamada `VIVO`, que indica se o recém-nascido está vivo ou veio a óbito. Essa coluna foi criada com base em comparações de múltiplas variáveis entre as duas bases de dados.

A base final, que contém 496.996 linhas — cada uma representando um recém-nascido — é utilizada para análise e modelagem preditiva e inclui as seguintes informações:

| Variável        | Descrição                                                                 |
|------------------|---------------------------------------------------------------------------|
| **APGAR1**       | Apgar no primeiro minuto                                                  |
| **APGAR5**       | Apgar no quinto minuto                                                   |
| **CODMUNNASC**   | Código do município de ocorrência                                          |
| **CODOCUPMAE**   | Código conforme a Classificação Brasileira de Ocupações                  |
| **CONSULTAS**    | Número de consultas de pré-natal: 1: Nenhuma, 2: de 1 a 3, 3: de 4 a 6, 4: 7 e mais, 9: Ignorado |
| **DTNASC**       | Data do nascimento, no formato ddmmaaaa                                   |
| **ESCMAE**       | Escolaridade da mãe, anos de estudo concluídos: 1: Nenhuma, 2: 1 a 3 anos, 3: 4 a 7 anos, 4: 8 a 11 anos, 5: 12 e mais, 9: Ignorado |
| **ESTCIVMAE**    | Estado civil da mãe: 1: Solteira, 2: Casada, 3: Viúva, 4: Separado judicialmente/Divorciado, 9: Ignorado |
| **GESTACAO**     | Semanas de gestação: 1: Menos de 22 semanas, 2: 22 a 27 semanas, 3: 28 a 31 semanas, 4: 32 a 36 semanas, 5: 37 a 41 semanas, 6: 42 semanas e mais, 9: Ignorado |
| **GRAVIDEZ**     | Tipo de gravidez: 1: Única, 2: Dupla, 3: Tripla e mais, 9: Ignorado    |
| **IDADEMAE**     | Idade da mãe em anos                                                    |
| **IDANOMAL**     | Anomalia congênita: 1: Sim, 2: Não, 9: Ignorado                        |
| **PARTO**        | Tipo de parto: 1: Vaginal, 2: Cesáreo, 9: Ignorado                     |
| **PESO**         | Peso ao nascer, em gramas                                               |
| **QTDFILMORT**   | Número de filhos mortos                                                  |
| **QTDFILVIVO**   | Número de filhos vivos                                                   |
| **QTDGESTANT**   | Número de gestações anteriores                                           |
| **QTDPARTCES**   | Número de partos cesáreos                                               |
| **QTDPARTNOR**   | Número de partos vaginais                                               |
| **RACACOR**      | Raça/Cor: 1: Branca, 2: Preta, 3: Amarela, 4: Parda, 5: Indígena       |
| **RACACORMAE**   | Raça/Cor: 1: Branca, 2: Preta, 3: Amarela, 4: Parda, 5: Indígena       |
| **SEXO**         | Sexo: 0: Ignorado, 1: Masculino, 2: Feminino                           |
| **TPROBSON**     | Código do Grupo de Robson, gerado pelo sistema                         |
| **KOTELCHUCK**   | Índice que avalia o número de consultas de pré-natal                         |
| **VIVO**         | Recém-nascido viveu 28 dias ou mais? Valores: 0: Não, 1: Sim          |

Após a criação da base de dados, foram aplicados os algoritmos de correlação de Pearson e Spearman, juntamente com os modelos de classificação Random Forest, CatBoost, Regressão Logística, Naive Bayes e KNN. Devido ao desbalanceamento presente na base original — em que a classe de interesse `VIVO`, correspondente à categoria 0, era significativamente menor do que a categoria 1 — foram utilizados três conjuntos de dados distintos para as análises. O segundo conjunto foi reduzido para 2.732 amostras, enquanto no terceiro aplicou-se a técnica SMOTE para lidar com o desbalanceamento, resultando em um novo dataset expandido com 100.000 amostras.

Além disso, utilizamos a técnica de Grid Search para otimização dos hiperparâmetros em cada um dos algoritmos, buscando aprimorar o desempenho preditivo dos modelos. Os melhores resultados, em termos de acurácia e performance geral, foram observados nos conjuntos de dados balanceados, especialmente no dataset gerado com SMOTE, onde os modelos apresentaram um desempenho significativamente superior.

## Resultados

A média das correlações foi calculada, e os resultados obtidos estão representados no gráfico a seguir:

![graph](https://github.com/gabrielborralhogomes/Predicao-de-Morte-Neonatal-no-Maranhao-utilizando-Aprendizado-de-Maquina/blob/main/dados/graficos/graf.png)

Após aplicar diferentes algoritmos de classificação, foi evidente que o desempenho dos modelos foi significativamente afetado pelo desbalanceamento das classes na variável `VIVO`. No dataset original, os modelos enfrentaram dificuldades para classificar corretamente a classe minoritária, resultando em métricas de desempenho aquém do esperado.

No entanto, ao utilizar técnicas de balanceamento, como a redução da base para 2.732 linhas e, em especial, a aplicação da técnica SMOTE, os resultados melhoraram consideravelmente. O uso do SMOTE, que gerou uma base com 100.000 linhas balanceadas, proporcionou os melhores resultados entre todos os datasets.

Com o SMOTE, os algoritmos Random Forest, CatBoost, Regressão Logística, Naive Bayes e KNN apresentaram ganhos significativos em precisão, sensibilidade e F1-score. Os resultados obtidos com o dataset balanceado pelo SMOTE foram ainda mais expressivos do que aqueles obtidos com a redução de dados, reforçando a importância de abordar o desbalanceamento para a criação de modelos preditivos mais robustos e eficazes.

## Conclusão

O estudo apresentou uma abordagem para a predição de morte neonatal no estado do Maranhão, utilizando algoritmos de Aprendizado de Máquina em bases de dados públicas do DataSUS. A análise focou na criação de modelos preditivos a partir de variáveis como o índice de Apgar, escolaridade da mãe, tipo de parto, peso ao nascer, entre outras.

Devido ao significativo desbalanceamento da classe de interesse, a aplicação de técnicas de balanceamento de dados, como a redução de amostras e o SMOTE, foi fundamental para melhorar a performance dos modelos. O uso do SMOTE, em particular, demonstrou ser a técnica mais eficiente para equilibrar o dataset, resultando em uma melhoria substancial na acurácia e nas métricas preditivas, como precisão e F1-score, quando comparada ao dataset original.

Os resultados ressaltam a importância de estratégias de balanceamento de dados em problemas de classificação com classes desproporcionais, especialmente em estudos críticos de saúde pública, como a predição de óbitos neonatais. Com essas técnicas, os modelos de machine learning podem ser aplicados com maior confiabilidade na identificação de fatores de risco, contribuindo para o desenvolvimento de políticas de saúde mais eficazes.

Sugere-se que os sistemas de saúde pública do Maranhão adotem um monitoramento contínuo das variáveis mais relevantes, como o índice de Apgar, consultas pré-natais, peso ao nascer e semanas de gestação, integrando análises preditivas em programas de atenção neonatal para antecipar intervenções em gestações de alto risco. O estudo também pode fundamentar a criação de sistemas de alerta precoce, permitindo que equipes médicas identifiquem rapidamente casos com alta probabilidade de óbito neonatal e adotem medidas preventivas eficazes.


