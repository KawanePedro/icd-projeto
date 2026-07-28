# Projeto - Introdução a Ciência de Dados

## INTEGRANTES:
- Gabriel Costa Morais
- Icaro Eduardo de Souza Lucena
- Kawane Pedro Antas Siqueira
- Murilo José Passos Lima

## TEMA
Filmes

## ABORDAGEM DE COLETA DE DADOS
Abordagem híbrida: extração de dados públicos do IMDb (via arquivos estruturados compactados) combinada com consumo de dados complementares via APIs do TMDB e OMDb.

---

## 📊 1. Contextualização do Conjunto de Dados
Este projeto realiza uma análise exploratória de dados do cinema mundial para identificar padrões ocultos e responder a perguntas estratégicas sobre a indústria cinematográfica. A análise investiga o comportamento financeiro dos filmes (relação entre orçamento e faturamento), características técnicas (evolução da duração dos filmes ao longo das décadas e impacto das franquias) e a percepção do público e da crítica especializada em diferentes plataformas.

---

## 🛠️ 2. Processo de Coleta e Pré-processamento (Base)
A construção do conjunto de dados principal foi realizada de forma local através do script `notebooks/01_base_projeto.ipynb`. Como os arquivos brutos fornecidos pelo IMDb contêm milhões de registros englobando todo tipo de produção audiovisual da história, aplicamos regras estritas de filtragem para garantir a qualidade e a relevância das análises:

1. **Leitura Otimizada:** Carregamos o arquivo `title.basics.tsv.gz` selecionando apenas as colunas essenciais para o escopo do projeto.
2. **Filtro de Formato:** Restringimos a base exclusivamente para produções listadas como `movie` (filmes de longa-metragem lançados para cinema ou streaming), descartando curtas-metragens, episódios de TV, novelas e jogos.
3. **Filtro de Relevância Estatística:** Cruzamos as informações com a tabela de avaliações `title.ratings.tsv.gz` e eliminamos todos os filmes que possuíam menos de **10.000 votos**.
4. **Junção (Inner Join):** Unificamos as tabelas através do identificador único do filme (`tconst`), gerando o arquivo final compactado.

---
### 2.1 Enriquecimento via API (TMDB e OMDb)

Para aprofundar as análises e responder às perguntas de forma completa, a base inicial foi expandida utilizando duas APIs externas:

1. **API do TMDB (The Movie Database)**: Através do script `notebooks/02_enriquecimento_tmdb.ipynb`, utilizamos o ID do IMDb para resgatar dados financeiros (orçamento e bilheteria absolutos), o idioma original do áudio e um identificador para mapear se o filme pertence a uma franquia. Esse enriquecimento ficou salvo como `df_tmdb.csv`.

2. **API do OMDb (The Open Movie Database)**: Através do script `notebooks/03_enriquecimento_omdb.ipynb`, buscamos o "Metascore" oficial da crítica especializada para permitir a comparação direta entre a opinião do público geral e a dos críticos. Esse enriquecimento ficou salvo como `df_omdb.csv`. A API do OMDb aceita apenas 1000 requisições diárias na sua versão gratuita, então, buscando simplicidade e agilidade no código, pagamos pela versão patreon (que custou menos de 10 reais) que permite 100.000 requisições diarias. Isso explica a mudança do script `notebooks/03_enriquecimento_omdb.ipynb` no commit do primeiro dia para o commit final.

---

## 💾 3. Link para os Dados Brutos e Processados
Todos os arquivos brutos originais do IMDb `(.tsv.gz)` e a base unificada/enriquecida gerada pelos nossos scripts de tratamento estão armazenados no ambiente de nuvem do projeto.

* **Acesso aos dados:** [https://drive.google.com/drive/folders/18xKlGb5MpACcoXz7IyIjFDGVxaOFSIKY?usp=sharing]

---

## 📖 4. Dicionário de Dados (`base_projeto.csv`)

| Nome da Coluna | Tipo de Dado | Descrição | Exemplo |
| :--- | :--- | :--- | :--- |
| `tconst` | Alfanumérico (String) | Identificador único e permanente do filme no IMDb. | `tt3783958` |
| `titleType` | Texto (String) | Tipo de mídia da produção (filtrado estritamente para `movie`). | `movie` |
| `primaryTitle` | Texto (String) | Título principal utilizado na divulgação internacional. | `La La Land` |
| `startYear` | Inteiro (Int) | Ano oficial de lançamento do filme. | `2016` |
| `runtimeMinutes` | Inteiro (Int) | Duração total do filme em minutos. | `128` |
| `genres` | Texto (String) | Gêneros associados ao filme (separados por vírgula). | `Comedy, Drama, Music` |
| `averageRating` | Float (Decimal) | Nota média ponderada atribuída pelo público do IMDb (0.0 a 10.0). | `8.0` |
| `numVotes` | Inteiro (Int) | Número total de votos recebidos pelo filme no IMDb (mínimo de 10.000). | `767859` |
| `original_language` | Texto (String) | Idioma original do áudio do filme (código ISO). | `en` |
| `is_franchise` | Booleano (Bool) | Indica se o filme pertence a uma sequência/franquia cinematográfica. | `False` |
| `budget` | Inteiro (Int) | Orçamento de produção do filme em dólares. Valores zerados indicam ausência de dados públicos oficiais. | `30000000.0` |
| `revenue` | Inteiro (Int) | Arrecadação total de bilheteria mundial em dólares. | `509183536.0` |
| `metascore_critica` | Inteiro (Int) | Nota atribuída pela crítica especializada (Metacritic), variando de 0 a 100. Valores -1 indicam que o filme não possui nota oficial da crítica na plataforma. | `94` |