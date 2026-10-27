# Arquivos de entrada

Extraia `DADOS_PMC.zip`. Copie os arquivos abaixo para esta pasta e renomeie somente os arquivos, mantendo seus conteúdos originais.

| Arquivo no ZIP fornecido | Nome usado no notebook |
|---|---|
| `execucao_detalhada_de_despesas_2023.csv` | `execucao_2023.csv` |
| `execucao_detalhada_de_despesas_2024.csv` | `execucao_2024.csv` |
| `execucao_detalhada_de_despesas_2025.csv` | `execucao_2025.csv` |
| `34878_despesasOrcamentarias_csv_fde1588ce523b7794fd663a7d3d8f37b.csv` | `orcamento_2023.csv` |
| `34878_despesasOrcamentarias_csv_f6aaf7bfe092f7ff351f661ceee8ffa9.csv` | `orcamento_2024.csv` |
| `34878_despesasOrcamentarias_csv_571c373ac278c9e21e2bfd4076c45970.csv` | `orcamento_2025.csv` |

Não é necessário utilizar os arquivos de cargos, contratos ou o orçamento de 2022 para esta unificação.

Não abra e salve os CSVs no Excel antes da leitura: isso pode modificar datas e códigos. O notebook lê diretamente os relatórios originais, incluindo suas tabelas internas.

A pasta é ignorada pelo Git para não versionar os CSVs brutos. O arquivo final já está disponível na raiz do repositório.
