# Mini-projeto — Análise Exploratória de Dados do Varejo

**Aluna:** Isabela Gomes da Costa  
**Curso identificado no notebook:** Manipulação de Dados com Python e SQL [T7]  
**Atividade:** M1S10 — Mini Projeto Módulo 1  
**Início:** 21/10/2026

## Objetivo

Desenvolver uma Análise Exploratória de Dados (AED) aplicada ao varejo, identificando problemas de qualidade, realizando a limpeza e extraindo informações sobre os clientes e os produtos presentes nas compras.

O arquivo principal é `mini_projeto_analise_de_dados_com_python.ipynb`. 

## Base de dados e dicionário

A base foi obtida no [Kaggle — Base Varejo](https://www.kaggle.com/datasets/namespaiva/base-varejo/data) e salva como `base_varejo.csv`, com ponto e vírgula como separador. O arquivo utilizado possui 830.000 registros e 14 colunas na importação.

Conforme o dicionário de dados disponibilizado com a base:

| Coluna | Significado |
| --- | --- |
| `DATA` | Data da compra |
| `CO_ID` | Identificação da compra pelo número da nota fiscal |
| `CL_ID` | Identificação do cliente |
| `CL_GENERO` | Sexo biológico informado pelo cliente |
| `CL_EC` | Estado civil: 1 — casado ou união estável; 2 — divorciado; 3 — separado; 4 — solteiro; 5 — viúvo |
| `CL_FHL` | Número de filhos do cliente |
| `CL_SEG` | Segmentação econômica do cliente: classe A, B ou C |
| `PR_ID` | Código do produto adquirido (SKU) |
| `PR_CAT` | Categoria do produto adquirido |
| `PR_NOME` | Nome do produto adquirido |

As outras quatro colunas, com nomes como `Unnamed: 10`, estão totalmente vazias e são removidas. Identificadores e códigos de categorias não são tratados como medidas quantitativas.

## Ferramentas e execução no VS Code

O trabalho usa Python, pandas para manipulação e análise dos dados, Matplotlib para o boxplot e ipykernel para executar o notebook. `pathlib` faz parte da biblioteca padrão do Python e não precisa ser instalado separadamente.

1. Abra no VS Code a pasta que contém o notebook, o `base_varejo.csv` e o `requirements.txt`.
2. Instale as extensões **Python** e **Jupyter**, da Microsoft.
3. No terminal, instale as dependências no ambiente que será selecionado como kernel:

   ```powershell
   python -m pip install -r requirements.txt
   ```

4. Abra `mini_projeto_analise_de_dados_com_python.ipynb` e selecione esse ambiente em **Selecionar Kernel**.
5. Reinicie o kernel e use **Executar Tudo**, executando as células na ordem. Se alguma célula apresentar erro, resolva antes de continuar.

As células `%pip install pandas` e `%pip install matplotlib` já presentes no notebook servem para instalação dentro do kernel. Depois de instalar o `requirements.txt` no ambiente correto, essas instalações são redundantes e podem ser comentadas. 

## Etapas desenvolvidas

### Sprint 1 — Importação dos dados

Importação do pandas, leitura do CSV e apresentação das primeiras linhas.

### Sprint 2 — Reconhecimento, diagnóstico e transformação de textos

Apresentação do número de registros, nomes das colunas e tipos de dados. Verificação de nulos, frequências dos preenchimentos textuais, datas inválidas, categorias vazias e registros duplicados.

O `isnull()` não identificou o texto `#N/D` como ausência. A exploração com `value_counts(dropna=False)` mostrou esse marcador em `PR_CAT` e `PR_NOME`, justificando a investigação complementar. As variações `N/D`, `N/A` e `NULL` também entram na busca, sem pressupor que todas existam na base.

A transformação dos textos utiliza expressão regular para padronizar espaços, remove espaços das extremidades, converte letras para maiúsculas e transforma marcadores de ausência em `pd.NA`. A base original é preservada em `df_varejo`, enquanto o tratamento é realizado em `df_limpo`.

### Sprint 3 — Limpeza de nulos e duplicatas

Remoção das quatro colunas totalmente vazias e de 96.553 linhas completamente repetidas, mantendo a primeira ocorrência. As ausências textuais são preenchidas com `NAO INFORMADO`. Registros sem informações essenciais seriam removidos; nesta base, nenhum registro foi excluído por esse motivo.

Os registros sem categoria pertencem ao produto 107, cujo nome e categoria não estão disponíveis em nenhuma ocorrência. Por isso, não foi realizada imputação por SKU nem pela categoria mais frequente.

### Sprint 4 — Estatística descritiva, boxplot e agrupamentos

Estatísticas do número de filhos por registro e por cliente distinto: contagem, média, mediana, desvio padrão amostral, moda, mínimo, máximo e quartis. Antes de manter uma linha por cliente, é verificada a existência de números de filhos divergentes para o mesmo `CL_ID`.

O boxplot considera cada cliente uma única vez e utiliza `whis=1.5`, correspondente ao critério de 1,5 × IQR. Não foram observados outliers do número de filhos por esse critério. Nenhum valor é removido por ser um possível outlier. 

Após conferir a consistência das compras, são realizados dois agrupamentos:

- Por `CL_GENERO`: registros, compras distintas, clientes distintos, percentual de compras e compras por cliente.
- Por `PR_CAT`: registros, compras que contêm a categoria, SKUs distintos, percentual de registros e percentual de compras com a categoria.

O percentual de compras com uma categoria indica sua presença nas compras distintas. Uma compra pode conter várias categorias; por isso, esses percentuais não precisam somar 100%.

### Sprint 5 — Relatório e documentação

Construção de `relatorio_final`, com contadores da base original, limpeza, compras, clientes e qualidade remanescente. O notebook exibe o relatório na saída da célula com `display()`. 

O notebook também apresenta as conclusões e as instruções de exportação da base limpa e dos resumos. A ausência de outliers do número de filhos é discutida a partir do boxplot; o relatório não depende de uma variável de contagem de outliers.

## Principais resultados

- Base limpa com **733.447 registros e 10 colunas**, sem nulos ou duplicatas exatas restantes.
- **18.471 compras distintas** e **1.000 clientes distintos**.
- Média de filhos de aproximadamente **1,146 por registro** e **1,136 por cliente**; mediana e moda iguais a zero.
- Sexo informado `F`: **9.615 compras e 519 clientes**; `M`: **8.856 compras e 481 clientes**. As médias de compras por cliente são próximas: aproximadamente 18,53 e 18,41.
- Categoria `ALIMENTOS`: **384.197 registros**, aproximadamente **52,38%** da base limpa, presente em **18.267 compras**.
- **3.228 registros do SKU 107** com nome e categoria não informados. Esse total não representa 3.228 produtos distintos.

A média por registro não é um cálculo errado: ela responde a uma pergunta diferente da média por cliente e dá mais peso aos clientes que aparecem em mais registros. Para descrever o perfil dos clientes, a análise por cliente distinto é a mais adequada.

## Limitações e decisões

A base não possui preços ou quantidades, portanto não permite calcular faturamento ou unidades vendidas. Uma linha representa uma associação de compra e produto, não necessariamente uma unidade física adquirida.

A remoção de linhas completamente iguais é uma hipótese para esta AED. Sem quantidade ou identificador do item, não é possível confirmar se todas as repetições são erros ou se algumas correspondem a múltiplas unidades do mesmo produto. A base original é mantida para permitir revisão dessa decisão.

O preenchimento `NAO INFORMADO` sinaliza a informação ausente, mas não recupera o nome ou a categoria real do SKU 107. O boxplot identifica possíveis valores extremos segundo um critério estatístico; ele não determina se um valor é um erro de cadastro.

## Arquivos exportados pelo notebook

Depois de executar as células na ordem, o bloco de exportação salva:

```text
df_limpo.csv
resultados/
├── nulos_originais.csv
├── estatisticas_filhos.csv
├── modas_filhos.csv
├── resumo_genero.csv
├── resumo_categoria.csv
└── relatorio_final.csv
```

Os CSVs usam `;` como separador. A base limpa é exportada sem o índice e os resumos usam índices quando eles identificam as linhas. As saídas são gravadas na pasta de trabalho do kernel; abra a pasta do projeto no VS Code para manter os caminhos relativos corretos. Uma nova execução substitui os arquivos de mesmo nome.

## Tipos de dados e revisão atual

A coluna `DATA` é convertida diretamente em `df_limpo` com `pd.to_datetime`, no formato de entrada dia/mês/ano, e o notebook confere o tipo resultante e a quantidade de datas ausentes ou inválidas. Assim, a exportação pode gravar as datas como `AAAA-MM-DD` com `date_format="%Y-%m-%d"`.
