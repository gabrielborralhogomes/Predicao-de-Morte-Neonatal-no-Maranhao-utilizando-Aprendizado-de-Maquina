# Predição de morte neonatal no Maranhão utilizando Aprendizado de Máquina

A morte neonatal, definida como o óbito que ocorre até os 28 dias de vida, é amplamente reconhecida em muitos países como um importante indicador da qualidade de vida, do desenvolvimento socioeconômico e do acesso da população aos serviços de saúde. Suas causas estão fortemente ligadas à qualidade dos cuidados de saúde prestados durante a gestação, o parto e o período pós-natal. A mortalidade neonatal desempenha um papel significativo nas taxas gerais de mortalidade infantil. De acordo com a Organização Mundial da Saúde (OMS), as mortes neonatais representam 47% dos óbitos em crianças menores de cinco anos. Embora continue a ser um fator central na mortalidade infantil, as taxas globais de morte neonatal diminuíram de 37 por mil nascidos vivos em 2000 para 18 por mil nascidos vivos em 2017. No Brasil, a taxa passou de 26 óbitos por mil nascidos vivos em 1990 para 16 por mil em 2015.

Apesar dessa redução, a morte neonatal permanece um problema crítico, não apenas no Brasil, mas em diversos outros países. Diante desse cenário, a predição de óbitos neonatais, utilizando bases de dados públicas do Brasil e aplicando técnicas de Aprendizado de Máquina, mostra-se altamente relevante para melhorar a saúde pública e reduzir ainda mais essas taxas.

O objetivo deste trabalho é desenvolver um modelo preditivo de óbitos neonatais utilizando algoritmos de Aprendizado de Máquina. Para isso, foram utilizadas duas bases de dados públicas disponibilizadas pelo DataSUS: o Sistema de Informação de Mortalidade ([SIM](https://opendatasus.saude.gov.br/dataset/sim)) e o Sistema de Informação de Nascidos Vivos ([SINASC](https://datasus.saude.gov.br/transferencia-de-arquivos/#)). As bases abrangem os casos de nascimento no Maranhão e os óbitos ocorridos tanto dentro quanto fora do estado, no período de 2017 a 2022. No total, foram analisados 12 arquivos no formato CSV.

Todas as etapas foram implementadas em Python, utilizando a biblioteca `pandas` para manipulação e análise de dados, e o ambiente Jupyter Notebook, que permite a execução do código de forma interativa através do navegador.

Inicialmente, foi realizada a análise, limpeza e normalização dos dados provenientes do SIM e SINASC. A base de dados do SINASC foi utilizada como principal fonte de informações sobre os nascimentos, enquanto o SIM serviu para adicionar uma nova variável chamada `VIVO`, que indica se o recém-nascido está vivo ou veio a óbito. Essa coluna foi criada com base em comparações de múltiplas variáveis entre as duas bases de dados.

A base final, usada para análise e modelagem preditiva, inclui as seguintes informações:

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
