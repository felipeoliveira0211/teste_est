# Análise estatística dos pagamentos da Saúde de Criciúma

Projeto da aula de Estatística. Período dos pagamentos: 2023 a 2025.

**Pergunta proposta:** Como o perfil das despesas e a execução orçamentária se associam ao intervalo entre o empenho e o pagamento na Saúde de Criciúma, e como esses padrões podem apoiar a programação dos desembolsos?

## Arquivos

- [Unificacao_Saude.ipynb](Unificacao_Saude.ipynb): notebook executado, com as etapas explicadas.
- [Analise_Estatistica_Saude.ipynb](Analise_Estatistica_Saude.ipynb): continuação executada, com 25 candidatas, correlações, seleção de 15 explicativas, medidas descritivas, gráficos, Shapiro-Wilk, tentativas de transformação e regressões exploratórias.
- [Base_unificada_Saude.csv](Base_unificada_Saude.csv): resultado da unificação, com 26.125 registros e 40 colunas, incluindo o intervalo calculado.
- [Base_estatistica_Saude.csv](Base_estatistica_Saude.csv): base numérica com 26.125 registros e 16 colunas, contendo o alvo original e as 15 explicativas selecionadas.
- [dados/](dados/): os seis CSVs originais utilizados, com nomes simplificados e conteúdos preservados. A [documentação das entradas](dados/README.md) mostra a correspondência com os nomes do ZIP fornecido.
- [resultados/RELATORIO_ESTATISTICO.md](resultados/RELATORIO_ESTATISTICO.md): interpretação dos resultados, conferência das exigências e limitações.
- [resultados/](resultados/): tabelas CSV, cinco gráficos PNG e resumos das regressões.

O notebook de unificação usa somente `pandas`. A organização e a união usam listas, `for`, `if`, comparações e acesso às colunas do DataFrame, seguindo o nível dos exercícios enviados. Não utiliza `merge`, `concat` ou funções próprias.

Para o cálculo de datas autorizado, foram acrescentados `pd.to_datetime` e `.dt.days`, explicados na célula correspondente. Essas operações não constavam nos exercícios enviados.

O notebook de análise usa as bibliotecas presentes nos materiais das aulas: `pandas`, `numpy`, `scipy`, `matplotlib` e `statsmodels`. As etapas indicam os exercícios ou slides de referência. As categorias são representadas com listas de 0/1 e condições simples.

## Executar

1. Baixe ou clone este repositório completo. Os seis CSVs originais já estão na pasta `dados`.
2. No ambiente Python usado para os notebooks, disponibilize as dependências de `requirements.txt`:

   ```text
   python -m pip install -r requirements.txt
   ```

3. Abra `Unificacao_Saude.ipynb` e execute as células na ordem para reproduzir a unificação. O DataFrame `base_unificada` conterá os pagamentos e o intervalo.
4. Abra `Analise_Estatistica_Saude.ipynb` e execute as células na ordem. Ele lê o CSV unificado já incluído e cria os resultados em memória.
5. Para iniciar diretamente a análise, não é necessário repetir a unificação: use o CSV já entregue. O diretório de trabalho deve ser a raiz deste projeto.

Os notebooks exibem os resultados e mantêm os DataFrames em memória. Os CSVs e PNGs entregues foram exportados como arquivos de apoio a partir da execução conferida; métodos de exportação não foram acrescentados ao código do aluno, pois não apareceram nos exercícios. Para ler diretamente a base estatística selecionada:

```python
import pandas as pd

base_estatistica = pd.read_csv("Base_estatistica_Saude.csv")
base_estatistica.head()
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

## Resultados e requisitos

| Exigência dos materiais enviados | Resultado |
|---|---|
| Pelo menos 20 mil registros | 26.125 pagamentos |
| Definir o alvo | Intervalo entre empenho e pagamento, em dias |
| Testar 25 candidatas | 25 testadas |
| Pelo menos 15 correlações com r < -0,3 ou r > 0,3 | 15, usando o alvo original |
| Shapiro-Wilk do alvo | Executado, com ressalvas para n > 5.000 e dependência entre registros |
| Tentar os tratamentos vistos na aula | Limites de outliers, raiz quadrada e Box-Cox, preservando o alvo original |
| Obter normalidade | Não obtida nas tentativas; resultado documentado para discutir com o professor |
| Regressão da Aula 10 | Exemplos simples e múltiplo, somente como ajuste exploratório histórico |

O intervalo médio foi de aproximadamente 75,84 dias e a mediana foi de 34 dias. Os empenhos estimativos apresentaram intervalo maior no conjunto analisado, compatível com pagamentos previstos ao longo do ano. Isso não demonstra atraso ou falha de gestão.

As medidas orçamentárias podem ser relacionadas entre si e são compartilhadas por vários pagamentos. Os valores correspondem à exportação fornecida, sem reconstrução do que era conhecido na data de cada empenho. As regressões não foram validadas em dados novos e seus p-valores convencionais não corrigem a dependência dos registros.

O aviso do SciPy sobre o Shapiro-Wilk para mais de 5.000 observações é mantido no notebook: não é falha na execução. A documentação oficial indica que o p-valor pode ser impreciso nesse tamanho de conjunto. Nenhuma tentativa foi apresentada como normalização bem-sucedida.

A pergunta, a interpretação dos resultados não normais e eventuais orientações posteriores precisam ser alinhadas com o professor. Os materiais fornecidos não incluíam outros critérios específicos da entrega final nem os feedbacks do grupo.

## Fonte

Arquivos municipais fornecidos em `DADOS_PMC.zip`, obtidos no [portal de dados abertos de Criciúma](https://transparencia.betha.cloud/#/n4W91vnHptoBkiHKAxioOA==/dados-abertos?esconderCabecalho=S&esconderMenu=S&esconderRodape=S).

Os seis arquivos brutos utilizados estão incluídos no Git, na pasta `dados`, e a base unificada está na raiz. Os relatórios originais preservam os campos publicados pela prefeitura. A base unificada contém as informações necessárias à próxima etapa, sem os nomes e documentos dos credores ou as contas bancárias dos pagamentos.
