# Projeto - Introdução a Ciência de Dados

## INTEGRANTES:
- Gabriel Costa Morais
- Icaro Eduardo de Souza Lucena
- Kawane Pedro Antas Siqueira
- Murilo José Passos Lima

## TEMA
Filmes

## ABORDAGEM DE COLETA DE DADOS
Abordagem híbrida: extração de dados públicos do IMDb (via arquivos estruturados compactados) combinada com consumo de dados complementares via API do TMDB.

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

## 💾 3. Link para os Dados Brutos e Processados
Todos os arquivos brutos originais do IMDb (`.tsv.gz`) e a base unificada gerada pelo nosso script de tratamento estão armazenados no ambiente de nuvem do projeto.

* **Acesso aos dados:** [INSIRA_AQUI_O_LINK_DO_GOOGLE_DRIVE]

---

## 📖 4. Dicionário de Dados (`base_projeto.csv`)

| Nome da Coluna | Tipo de Dado | Descrição | Exemplo |
| :--- | :--- | :--- | :--- |
| `tconst` | Alfanumérico (String) | Identificador único e permanente do filme no IMDb. | `tt0111161` |
| `titleType` | Texto (String) | Tipo de mídia da produção (filtrado estritamente para `movie`). | `movie` |
| `primaryTitle` | Texto (String) | Título principal utilizado na divulgação internacional. | `The Shawshank Redemption` |
| `startYear` | Inteiro (Int) | Ano oficial de lançamento do filme. | `1994` |
| `runtimeMinutes` | Inteiro (Int) | Duração total do filme em minutos. | `142` |
| `genres` | Texto (String) | Gêneros associados ao filme (separados por vírgula). | `Drama` |
| `averageRating` | Float (Decimal) | Nota média ponderada atribuída pelo público do IMDb (0.0 a 10.0). | `9.3` |
| `numVotes` | Inteiro (Int) | Número total de votos recebidos pelo filme no IMDb (mínimo de 10.000). | `2900000` |
