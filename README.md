# Unificação das bases da Saúde de Criciúma

Projeto de preparação de dados para a aula de Estatística. Período dos pagamentos: 2023 a 2025.

**Pergunta proposta:** Como o perfil das despesas e a execução orçamentária se associam ao intervalo entre o empenho e o pagamento na Saúde de Criciúma, e como esses padrões podem apoiar a programação dos desembolsos?

## Arquivos

- [Unificacao_Saude.ipynb](Unificacao_Saude.ipynb): notebook executado, com as etapas explicadas.
- [Base_unificada_Saude.csv](Base_unificada_Saude.csv): resultado da unificação, com 26.125 registros e 40 colunas, incluindo o intervalo calculado.
- [dados/](dados/): os seis CSVs originais utilizados, com nomes simplificados e conteúdos preservados. A [documentação das entradas](dados/README.md) mostra a correspondência com os nomes do ZIP fornecido.

O notebook usa somente `pandas`. A organização e a união usam listas, `for`, `if`, comparações e acesso às colunas do DataFrame, seguindo o nível dos exercícios enviados. Não utiliza `merge`, `concat`, funções próprias ou outras bibliotecas.

Para o cálculo de datas autorizado, foram acrescentados `pd.to_datetime` e `.dt.days`, explicados na célula correspondente. Essas operações não constavam nos exercícios enviados.

## Executar

1. Baixe ou clone este repositório completo. Os seis CSVs originais já estão na pasta `dados`.
2. Abra o notebook no ambiente usado nas aulas, com `pandas` disponível.
3. Execute as células na ordem, usando a pasta deste projeto como diretório de trabalho.
4. Ao final, o DataFrame `base_unificada` conterá os pagamentos e o intervalo em dias.

O notebook termina no DataFrame. O CSV entregue foi exportado como arquivo de apoio a partir do resultado executado; a função de exportação não foi acrescentada ao código do aluno porque não apareceu nos exercícios. Para iniciar as próximas etapas diretamente pelo resultado:

```python
import pandas as pd

base_unificada = pd.read_csv("Base_unificada_Saude.csv")
base_unificada.head()
```

## Organização e conferências

- Fontes: **Execução Detalhada de Despesas** e **Despesas por Programas e Ações**.
- Recorte: função **Saúde**, pagamentos datados entre 01/01/2023 e 31/12/2025.
- Cada linha final corresponde a um registro de pagamento do relatório.
- A chave de união é **entidade + exercício do empenho + número da despesa**. A seção `dotacaoOrcamentaria` informa o número da despesa de cada empenho.
- Os relatórios contêm tabelas internas; o notebook separa os pagamentos e seus respectivos empenhos antes da união.
- Foram excluídos conservadoramente **114 registros de pagamento integralmente iguais nos nove campos originais, dentro do mesmo empenho**. Sem um identificador único de transação preenchido, não é possível distinguir eventuais lançamentos reais que sejam integralmente iguais. Essa regra deve ser informada na descrição da base.
- Todos os 26.125 pagamentos aproveitados tiveram uma única correspondência na tabela orçamentária: nenhum pagamento ficou sem orçamento e a união não multiplicou as linhas.
- Os campos de orçamento repetidos nas linhas representam o contexto da dotação; não devem ser somados novamente a cada pagamento.

## Variável alvo

`intervalo_empenho_pagamento_dias` = `Data do pagamento` − `Data do empenho`.

O resultado está em dias inteiros. Não representa necessariamente atraso: empenhos podem prever pagamentos ao longo de vários meses, e não foram fornecidas datas de vencimento. A base descreve pagamentos realizados, não o prazo dos empenhos ainda não pagos.

Este trabalho está limitado à unificação e ao cálculo do intervalo. A transformação de categorias em indicadores, as correlações, os testes de normalidade e os modelos ficam para as próximas etapas do grupo. Os valores orçamentários correspondem à exportação fornecida, sem reconstrução do que era conhecido na data de cada empenho.

## Fonte

Arquivos municipais fornecidos em `DADOS_PMC.zip`, obtidos no [portal de dados abertos de Criciúma](https://transparencia.betha.cloud/#/n4W91vnHptoBkiHKAxioOA==/dados-abertos?esconderCabecalho=S&esconderMenu=S&esconderRodape=S).

Os seis arquivos brutos utilizados estão incluídos no Git, na pasta `dados`, e a base unificada está na raiz. Os relatórios originais preservam os campos publicados pela prefeitura. A base unificada contém as informações necessárias à próxima etapa, sem os nomes e documentos dos credores ou as contas bancárias dos pagamentos.
