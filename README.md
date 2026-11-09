# Banco de dados da aula de machine learning

**Paraisópolis, São Paulo · Introdução à clusterização territorial**

Responsável pela aula: Adirson Maciel de Freitas Junior — CEFAVELA / UFABC.

Este repositório contém bancos de dados didáticos em CSV e este documento. O CSV é o recorte didático usado na V3 da aula: **338 células de uma grade de 50 × 50 metros**, cada uma com um identificador, coordenadas e oito medidas. Não há uma coluna com o “grupo correto”: os alunos calcularão e interpretarão os agrupamentos.

## Abrir os dados diretamente no Google Colab

Abra um notebook no [Google Colab](https://colab.research.google.com/), crie uma célula de código, copie o trecho e execute com **Shift + Enter**:

```python
import pandas as pd
url = "https://raw.githubusercontent.com/adirson52/aula-machine-learning-territorio/main/paraisopolis.csv"
dados = pd.read_csv(url, dtype={"id_celula": str})
dados.head()
```

O comando lê o CSV pela internet e cria a tabela `dados` na sessão Python do Colab. Não é necessário baixar o CSV manualmente, enviar um arquivo pelo painel Arquivos, montar o Google Drive ou autenticar no GitHub. A conexão com a internet continua necessária.

Confira a dimensão em outra célula:

```python
dados.shape
```

Resultado esperado: **(338, 11)**. Cada linha representa uma célula territorial e cada coluna descreve uma informação sobre ela. Os nomes das colunas são os mesmos do caderno da aula.

O endereço acima acompanha a versão da branch `main`. Para reproduzir uma versão específica no futuro, substitua `main` pelo identificador do commit correspondente.

## Dicionário das colunas

| Coluna | Significado | Unidade | Uso na aula |
|---|---|---|---|
| `id_celula` | Identificador da célula; manter como texto. | Sem unidade | Identificação |
| `longitude` | Longitude do centro da célula, WGS84 / EPSG:4326. | Graus decimais | Mapa |
| `latitude` | Latitude do centro da célula, WGS84 / EPSG:4326. | Graus decimais | Mapa |
| `cobertura_edificada_pct` | Percentual da célula coberto por edificações. | % | Análise com quatro e oito medidas |
| `altura_media_m` | Altura média dos edifícios na célula. | m | Análise com quatro e oito medidas |
| `declividade_pct` | Declividade média do terreno; percentual, não graus. | % de declividade | Análise com quatro e oito medidas |
| `distancia_via_m` | Distância à via da camada de vias para carros da base de 2022. | m | Análise com quatro e oito medidas |
| `vegetacao_ndvi` | Índice de vegetação médio da célula. | Índice de −1 a 1 | Opção na atividade em duplas |
| `idade_ocupacao_anos` | Tempo desde a primeira observação urbana na série até 2022; proxy de idade da ocupação. | Anos | Opção na atividade em duplas |
| `quantidade_edificacoes` | Contagem de edificações associada à célula. | Edificações | Opção na atividade em duplas |
| `quantidade_domicilios` | Contagem de domicílios do CNEFE 2022 associada à célula. | Domicílios | Opção na atividade em duplas |

## As quatro medidas da demonstração

```python
quatro = ["cobertura_edificada_pct", "altura_media_m",
          "declividade_pct", "distancia_via_m"]
dados[quatro].head()
```

Na atividade em duplas, as outras quatro medidas podem ser acrescentadas conforme a pergunta territorial. `id_celula`, `longitude` e `latitude` identificam e localizam os registros; ficam fora do conjunto de características do K-means desta aula.

Antes do agrupamento, confira as unidades e distribuições. As medidas têm escalas diferentes: o exercício aplica padronização antes de comparar distâncias. Para interpretar os perfis, retorne às unidades originais.

## Origem e preparação

Este é um recorte da base física de Paraisópolis organizada no projeto Revelando Favelas, já agregado por célula. Não representa uma nova coleta realizada pelo notebook da aula.

| Campo da aula | Campo de origem | Preparação/origem registrada |
|---|---|---|
| `cobertura_edificada_pct` | `gba_cobertura` | Cobertura do GlobalBuildingAtlas multiplicada por 100 para expressar porcentagem. |
| `altura_media_m` | `gba_altura_media` | Altura média derivada do GlobalBuildingAtlas. A fonte documenta imagens principalmente de 2019/2018; a data da publicação não é a data de todas as observações. |
| `declividade_pct` | `decli_media` | Relevo derivado de SRTM, expresso em porcentagem de declividade. |
| `distancia_via_m` | `vias_carro_dist_2022` | Medida da camada de vias para carros da base de 2022. |
| `vegetacao_ndvi` | `ndvi_2022` | Sentinel-2 SR Harmonized, referência 2022. |
| `idade_ocupacao_anos` | `idade_ocupacao_media_2022` | Série urbana MapBiomas, 1985–2022, conforme documentação da preparação. |
| `quantidade_edificacoes` | `gba_contagem` | Contagem derivada do GlobalBuildingAtlas. |
| `quantidade_domicilios` | `cnefe_qtd_dom_2022` | Contagem associada à célula a partir do CNEFE 2022. |

Os centros da grade foram convertidos para coordenadas geográficas WGS84 (EPSG:4326). O CSV disponibilizado conserva os valores e a ordem do recorte da aula; não houve alteração para melhorar a separação dos grupos.

## Como interpretar com cuidado

- As fontes têm períodos e métodos diferentes; a tabela não é um retrato de um único dia nem uma atualização em tempo real.
- A série de idade começa em 1985: um valor de 37 anos pode representar **37 anos ou mais** em 2022. Não é a idade de uma pessoa nem a data exata da construção de uma casa.
- Cobertura, altura e contagens são medidas agregadas. Uma linha não corresponde a uma pessoa, família ou edifício individual.
- Ausência de valores faltantes não garante ausência de erros nas fontes.
- Os grupos dependem das variáveis, da escala e do método escolhidos; suas etiquetas numéricas não são uma ordem de qualidade.

## Formato e conferência da edição publicada

- CSV em UTF-8; separador vírgula; ponto como separador decimal.
- 338 linhas de dados, 11 colunas e 338 identificadores distintos.
- Nenhum valor faltante no recorte disponibilizado.
- Não há rótulos de cluster nem respostas dos exercícios no arquivo.
- SHA-256 do CSV: `0480d389e45238404ecafaaa9362650271d7a2871898ef4a6ad401a5ecfdb179`.

## English

This teaching dataset covers **338 cells of a 50 × 50 m grid in Paraisópolis, São Paulo**, with **11 columns**: one ID, two geographic coordinates and eight measurements. Use the Colab code above to load the public CSV directly into a pandas table; no manual file upload or GitHub authentication is required. The expected shape is `(338, 11)`.

| Column | Meaning and unit |
|---|---|
| `id_celula` | Grid-cell ID; keep as text. |
| `longitude`, `latitude` | Cell-centre coordinates in WGS84 / EPSG:4326, decimal degrees. |
| `cobertura_edificada_pct` | Building coverage, %. |
| `altura_media_m` | Mean building height, m. |
| `declividade_pct` | Mean terrain slope, percent rather than degrees. |
| `distancia_via_m` | Distance to a road in the vehicle-road layer of the 2022 database, m. |
| `vegetacao_ndvi` | Mean NDVI, index from −1 to 1. |
| `idade_ocupacao_anos` | Years since the first urban observation in the available series, up to 2022. |
| `quantidade_edificacoes` | Building count associated with the cell. |
| `quantidade_domicilios` | Dwelling count associated with the cell, from CNEFE 2022. |

The demonstration uses building coverage, height, slope and road distance. Other measurements are options for the paired exercise. IDs and coordinates are excluded from the clustering features. Sources include GlobalBuildingAtlas, SRTM, Sentinel-2, MapBiomas and CNEFE; observations refer to different periods. The urban-age series starts in 1985, so 37 years may mean 37 years or more in 2022. These are aggregated measurements, not records of individuals. All 338 IDs are unique and the published file has no missing values or cluster labels. Standardize features for the exercise and interpret profiles in the original units. The CSV is identical to the local teaching extract.

## Español

Esta base didáctica contiene **338 celdas de una cuadrícula de 50 × 50 m en Paraisópolis, São Paulo**, con **11 columnas**: un identificador, dos coordenadas y ocho medidas. Use el código de Colab de arriba para leer el CSV público directamente en pandas, sin descargar y subir el archivo manualmente ni autenticarse en GitHub. La dimensión esperada es `(338, 11)`.

| Columna | Significado y unidad |
|---|---|
| `id_celula` | Identificador de la celda; conservar como texto. |
| `longitude`, `latitude` | Coordenadas del centro en WGS84 / EPSG:4326, grados decimales. |
| `cobertura_edificada_pct` | Cobertura de edificaciones, %. |
| `altura_media_m` | Altura media de las edificaciones, m. |
| `declividade_pct` | Pendiente media del terreno, porcentaje y no grados. |
| `distancia_via_m` | Distancia a una vía de la capa de vías para vehículos de la base de 2022, m. |
| `vegetacao_ndvi` | NDVI medio, índice de −1 a 1. |
| `idade_ocupacao_anos` | Años desde la primera observación urbana de la serie disponible hasta 2022. |
| `quantidade_edificacoes` | Cantidad de edificaciones asociada a la celda. |
| `quantidade_domicilios` | Cantidad de viviendas asociada a la celda, a partir del CNEFE 2022. |

La demostración usa cobertura, altura, pendiente y distancia a la vía. Las otras medidas son opciones para el ejercicio en parejas. Los identificadores y las coordenadas quedan fuera de las variables de agrupamiento. Las fuentes incluyen GlobalBuildingAtlas, SRTM, Sentinel-2, MapBiomas y CNEFE, con períodos de observación diferentes. La serie de edad urbana empieza en 1985: 37 años puede significar 37 años o más en 2022. Son medidas agregadas, no registros de personas. Los 338 identificadores son únicos y el archivo no contiene valores faltantes ni etiquetas de grupos. Estandarice las variables para el ejercicio e interprete los perfiles en sus unidades originales. El CSV conserva el recorte local de la clase.


## Exemplos fictícios para aprender as contas

Os dois arquivos abaixo foram **inventados para ensino**. Não são dados de alunos reais nem observações territoriais. Nenhum deles contém o grupo final como resposta.

| Arquivo | Conteúdo | Objetivo |
|---|---|---|
| `notas_alunos_ficticios.csv` | Quatro nomes fictícios, notas de Matemática e Português na escala 0–10. | Começar o K-means por uma nota, acrescentar a segunda e calcular distâncias e médias à mão. São os mesmos quatro pontos da V3. |
| `ocupacao_ficticia.csv` | Seis células hipotéticas de 50×50m; lado, área da célula, área edificada e cobertura em %. | Entender unidades e a relação cobertura = 100 × área edificada / área da célula, antes dos dados reais. |

Para ler as notas no Colab:

```python
import pandas as pd
url_notas = "https://raw.githubusercontent.com/adirson52/aula-machine-learning-territorio/main/notas_alunos_ficticios.csv"
notas = pd.read_csv(url_notas).set_index("aluno")
notas
```

Para ler o exemplo de ocupação:

```python
url_exemplo = "https://raw.githubusercontent.com/adirson52/aula-machine-learning-territorio/main/ocupacao_ficticia.csv"
exemplo = pd.read_csv(url_exemplo)
exemplo
```

**English:** `notas_alunos_ficticios.csv` contains four invented students and two grades (0–10), matching the V3 manual K-means example. `ocupacao_ficticia.csv` contains six hypothetical 50×50m cells: side length (m), cell area (m²), built-up area (m²) and coverage (%). They are synthetic teaching examples, not personal or observed territorial records. Neither includes cluster answers.

**Español:** `notas_alunos_ficticios.csv` contiene cuatro estudiantes ficticios y dos notas (0–10), iguales al ejemplo manual de K-means de la V3. `ocupacao_ficticia.csv` contiene seis celdas hipotéticas de 50×50m: lado (m), área de la celda (m²), superficie edificada (m²) y cobertura (%). Son ejemplos inventados, no datos personales ni observaciones territoriales. Ninguno incluye las respuestas de los grupos.
