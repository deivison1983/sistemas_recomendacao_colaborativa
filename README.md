# Recomendação Colaborativa

## Visão geral

Este projeto apresenta experimentos práticos com **sistemas de recomendação colaborativa**, explorando como avaliações históricas de usuários podem ser utilizadas para identificar preferências, estimar notas e gerar rankings personalizados de filmes.

O notebook progride de exemplos didáticos com pequenas matrizes de avaliações para experimentos com dados de filmes e avaliações, implementando abordagens baseadas em vizinhança, fatoração de matrizes e ranking. Também compara modelos com métricas de erro de predição e métricas de qualidade do ranking.

O trabalho foi desenvolvido no contexto acadêmico da Pós-Graduação em Inteligência Artificial e Aprendizado de Máquina da PUC Minas.

## Objetivos de aprendizagem

- Compreender os fundamentos da recomendação colaborativa e a diferença entre prever notas (*rating prediction*) e ordenar itens (*ranking*).
- Explorar as abordagens **User-User** e **Item-Item**.
- Preparar dados de avaliações no formato usuário–item–nota e construir matrizes de interação.
- Treinar modelos colaborativos baseados em vizinhança e em fatores latentes.
- Gerar recomendações personalizadas, evitando recomendar itens já avaliados pelo usuário.
- Comparar modelos utilizando métricas adequadas à tarefa.

## Dados e preparação

O notebook utiliza dois conjuntos de dados:

- **Avaliações coletadas por formulário da turma:** avaliações de um conjunto de filmes são convertidas para o formato `user`, `item` e `rating`. Respostas ausentes ou notas iguais a zero são descartadas nessa preparação.
- **Dados de filmes e avaliações:** A prática utiliza exemplos de recomendação de filmes com o conjunto de dados **MovieLens**.

As principais etapas de preparação incluem:

- Conversão das avaliações para formato longo e padronização dos identificadores de usuário e filme.
- Construção de matrizes usuário × item, nas quais valores ausentes representam interações não observadas.
- Conversão dos registros para os formatos exigidos pelas bibliotecas Cornac e Surprise.
- Separação de conjuntos de treino e teste para avaliação dos modelos.


## Técnicas implementadas

### 1. Recomendação baseada em vizinhança

- **User-User:** calcula a similaridade entre usuários com base em seus padrões de avaliação. O notebook utiliza correlação de Pearson em exemplos didáticos e o modelo `UserKNN` da Cornac.
- **Item-Item:** identifica filmes com padrões de avaliação semelhantes. O exemplo com Surprise utiliza `KNNBaseline` e similaridade `pearson_baseline` na configuração baseada em itens.
- **Similaridade por cosseno ajustado:** em um exemplo didático, centraliza as avaliações pela média de cada usuário e calcula similaridade do cosseno entre itens.
- **Predição de nota:** demonstra estimativas por média ponderada das avaliações de usuários ou itens vizinhos.

### 2. Modelos colaborativos para previsão de notas (*rating*)

O notebook treina e compara modelos das bibliotecas Cornac e Surprise, incluindo:

- **KNN:** `UserKNN`, `ItemKNN`, `KNNBasic`, `KNNWithMeans` e `KNNBaseline`.
- **Fatoração de matrizes e fatores latentes:** `MF`, `SVD`, `SVD++`, `NMF` e `PMF`.
- **Modelo de linha de base:** `BaselineOnly`.

A avaliação de previsão de notas utiliza **RMSE** (*Root Mean Squared Error*) e **MAE** (*Mean Absolute Error*). O notebook reúne os resultados das duas bibliotecas para ordenar os modelos segundo o RMSE e selecionar o modelo com menor erro observado nesse experimento.

### 3. Modelos colaborativos para ranking

A abordagem baseada em ranking busca ordenar os itens conforme a preferência estimada do usuário, em vez de se concentrar apenas na previsão de uma nota exata.

- **BPR (Bayesian Personalized Ranking):** utilizado nos exemplos com dados da turma e na avaliação dos modelos de ranking.
- **WBPR (Weighted Bayesian Personalized Ranking):** incluído na comparação de modelos de ranking.

Na avaliação com Cornac, avaliações iguais ou superiores ao limiar configurado são tratadas como interações positivas. Os modelos são comparados por **Precision@10**, **Recall@10**, **NDCG@10** e **MAP**. O notebook seleciona o modelo com maior NDCG@10 entre os modelos avaliados.

### 4. Geração de recomendações

As funções implementadas:

- Produzem listas *Top-N* para usuários específicos e, no exemplo da turma, para vários alunos.
- Excluem dos candidatos os filmes que o usuário já avaliou.
- Associam os IDs recomendados a títulos, gêneros e outras informações presentes no catálogo de filmes.
- Permitem comparar listas geradas por modelos de previsão de notas e por modelos de ranking.

### 5. Perfil de usuário por gêneros

Além da recomendação colaborativa, o notebook constrói um perfil descritivo de preferências a partir dos gêneros dos filmes avaliados positivamente:

- Cria uma matriz binária filme × gênero.
- Normaliza a contribuição dos gêneros de cada filme.
- Pondera os atributos pelos ratings do usuário, considerando por padrão notas a partir de `4.0`.
- Agrega e normaliza os pesos para obter um perfil de preferências, que pode ser visualizado por gênero.

Essa parte é uma representação baseada em atributos dos filmes e complementa os experimentos de recomendação colaborativa; não deve ser confundida com os modelos colaborativos avaliados.

## Tecnologias e bibliotecas

- **Python**
- **Pandas** e **NumPy** — manipulação e transformação dos dados.
- **Matplotlib** — visualizações e gráficos exploratórios.
- **scikit-learn** — cálculo de similaridade do cosseno.
- **Cornac** — modelos colaborativos, divisão de dados e métricas de avaliação.
- **Surprise** — algoritmos de recomendação e avaliação de previsões.

## Principais aprendizados

- A recomendação colaborativa pode explorar similaridades entre usuários ou entre itens.
- Prever uma nota e produzir um ranking são objetivos diferentes e exigem métricas de avaliação distintas.
- Modelos baseados em vizinhança são interpretáveis por suas relações entre usuários ou itens, enquanto modelos de fatores latentes aprendem representações a partir das interações.
- A comparação de modelos precisa considerar a tarefa, a configuração experimental e as métricas adotadas; não existe um modelo universalmente melhor a partir de um único experimento.
- O tratamento de avaliações ausentes, a definição de interações positivas e a exclusão de itens já conhecidos afetam os resultados e a utilidade das recomendações.

## Limitações e próximos passos

- Os resultados dependem dos dados disponíveis, do nível de esparsidade da matriz usuário × item e dos parâmetros utilizados.
- As métricas e os resultados obtidos representam os experimentos definidos no notebook; não comprovam desempenho em produção.
- A avaliação implementada usa divisões de treino e teste e métricas agregadas. Experimentos futuros podem investigar validação cruzada, estabilidade entre divisões e análise por usuário.
- Para aprofundar a análise, seria útil comparar cobertura do catálogo, diversidade, novidade e desempenho em diferentes tamanhos de *Top-N*.
- O perfil por gêneros é uma representação simples: pode ser expandido e avaliado separadamente, sem confundir essa abordagem com a filtragem colaborativa.

## Contexto

Projeto acadêmico desenvolvido como prática da unidade de **Análise de Sentimentos e Sistemas de Recomendação**, na Pós-Graduação em Inteligência Artificial e Aprendizado de Máquina da PUC Minas. O notebook tem finalidade didática e experimental.

## Autor

**Deivison Morais.** Visite o meu portfólio de projetos [aqui](https://deivison1983.github.io/portfolio_projetos/).

Pós-Graduação em Inteligência Artificial e Aprendizado de Máquina — PUC Minas

Professor Orientador: Humberto Mossri

### Contatos

<div>
  <a href = "https://www.linkedin.com/in/deivisonmorais/"><img src = "https://img.shields.io/badge/-deivisonmorais-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"></a>
  <a href = "mailto:deivison1983@gmail.com"><img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"></a>
</div>
