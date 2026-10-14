# Instruções de execução — Pipeline de previsão de churn

## 1. Objetivo

Este notebook demonstra um fluxo de aprendizado supervisionado para prever quais clientes podem cancelar um serviço (*churn*), utilizando **Regressão Logística**, Python e Scikit-learn.

## 2. Arquivos e recursos necessários

- Notebook: `pipeline_churn.ipynb` (ou o nome que você der ao notebook).
- Dataset **Telco Customer Churn**, disponível no Kaggle: https://www.kaggle.com/datasets/blastchar/telco-customer-churn
- Uma conta Google para usar o Google Colab.

> O notebook não inclui a base de dados. Baixe o dataset no Kaggle. Se o download vier em um arquivo ZIP, extraia-o e localize o arquivo CSV. Na execução, envie somente o CSV correto do dataset Telco Customer Churn.

## 3. Ambiente recomendado

Execute no **Google Colab**, pois o notebook utiliza `google.colab.files.upload()` para receber o arquivo CSV. As bibliotecas utilizadas — pandas, Matplotlib, Seaborn e Scikit-learn — normalmente já estão disponíveis no ambiente padrão do Colab; não é necessário instalar nada para a execução habitual.

## 4. Passo a passo

1. Abra https://colab.research.google.com/ no navegador.
2. Selecione **Arquivo → Fazer upload de notebook** e escolha o arquivo `.ipynb`.
3. Com o notebook aberto, selecione **Ambiente de execução → Executar tudo**. Também é possível executar as células uma por vez, sempre de cima para baixo.
4. Quando a célula de carregamento solicitar o upload, selecione o CSV do dataset Telco Customer Churn no computador.
5. Aguarde as células terminarem. Verifique a prévia dos dados, as mensagens de treinamento, as métricas calculadas e o gráfico da matriz de confusão.

**Importante:** o notebook seleciona o primeiro arquivo `.csv` enviado. Para evitar carregar o arquivo errado, envie apenas o CSV correto. Ele precisa conter as colunas esperadas pela implementação, incluindo `Churn` e `TotalCharges`; a coluna `customerID` é removida pelo próprio notebook quando existe.

## 5. O que cada etapa faz

1. **Importação das bibliotecas:** carrega as ferramentas para manipular dados, treinar o modelo e avaliar as previsões.
2. **Carregamento dos dados:** recebe o CSV e exibe uma prévia das primeiras linhas.
3. **Preparação:** remove espaços extras dos nomes das colunas e dos textos; converte `TotalCharges` para formato numérico; preenche valores ausentes nessa coluna com a mediana; remove `customerID`, quando presente; transforma `Churn` em alvo numérico (`No = 0`, `Yes = 1`) e converte variáveis categóricas em colunas numéricas (*one-hot encoding*).
4. **Separação e escala:** divide os registros em conjuntos de treino (80%) e teste (20%), mantendo a proporção das classes com `stratify`. A padronização é ajustada no conjunto de treino e aplicada ao conjunto de teste.
5. **Treinamento:** treina uma Regressão Logística com `class_weight='balanced'` e `max_iter=1000`.
6. **Avaliação:** calcula acurácia, precisão, recall, F1-score e ROC-AUC com os dados de teste.
7. **Matriz de confusão:** apresenta visualmente as previsões corretas e os erros do modelo.

## 6. Como interpretar os resultados

- **Acurácia:** proporção de previsões corretas entre todos os clientes do conjunto de teste.
- **Precisão:** entre os clientes previstos como possíveis cancelamentos, a proporção que realmente cancelou.
- **Recall:** entre os clientes que realmente cancelaram, a proporção identificada pelo modelo.
- **F1-score:** medida que combina precisão e recall.
- **ROC-AUC:** avalia a capacidade de ordenar/separar clientes das duas classes usando as probabilidades previstas; não representa a mesma coisa que acurácia.

Na matriz de confusão, as **linhas representam a classe real** e as **colunas representam a previsão**. A diagonal principal corresponde aos acertos; as outras duas células são os erros. No problema de churn, um falso negativo é um cliente que cancelou, mas que o modelo previu como alguém que não cancelaria.

Os valores das métricas são calculados pelo notebook quando ele é executado. Registre no relatório ou na apresentação os números que aparecerem na sua execução, sem presumir que serão idênticos caso o arquivo de dados ou o processamento seja alterado.

## 7. Solução de problemas

- **A janela de upload não aparece:** confirme que está executando no Google Colab e que chegou à célula de carregamento dos dados.
- **Erro de coluna ausente (`KeyError`):** confira se selecionou o CSV correto do Telco Customer Churn e se ele contém as colunas esperadas, especialmente `Churn` e `TotalCharges`.
- **O arquivo enviado não é o dataset esperado:** execute novamente a célula de carregamento e envie apenas o CSV correto.
- **As métricas não aparecem:** execute as células anteriores na ordem, principalmente as etapas de preparação e treinamento.
- **Quer executar localmente:** a célula `from google.colab import files` depende do Colab; a execução local exige adaptar a forma de carregar o arquivo CSV.

## 8. Fonte do dataset

IBM Telco Customer Churn, disponibilizado no Kaggle: https://www.kaggle.com/datasets/blastchar/telco-customer-churn

## 9. Limitações

Este notebook é uma demonstração didática, e suas métricas não garantem o mesmo desempenho em clientes futuros. O resultado depende dos dados, do preparo realizado e do modelo escolhido. As previsões devem apoiar análises e decisões, não ser usadas como certeza de que uma pessoa cancelará o serviço.
