# Arquivos de entrada

Esta pasta contém os seis CSVs originais usados na unificação, extraídos de `DADOS_PMC.zip`. Somente os nomes dos arquivos foram simplificados; os conteúdos foram preservados integralmente, incluindo as tabelas internas.

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

Os arquivos estão versionados no Git e podem ser usados diretamente pelo notebook após baixar ou clonar o projeto. O arquivo final `Base_unificada_Saude.csv` permanece na raiz do repositório.

O arquivo `.gitattributes` desativa a conversão de quebras de linha nestes CSVs para preservar os bytes dos originais.
