# Análise dos pagamentos da Saúde de Criciúma

Execução conferida em 31/10/2026. Período: pagamentos de 2023 a 2025.

## Pergunta e base

**Como o perfil das despesas e a execução orçamentária se associam ao intervalo entre o empenho e o pagamento na Saúde de Criciúma, e como esses padrões podem apoiar a programação dos desembolsos?**

O alvo é `intervalo_empenho_pagamento_dias`, obtido pela diferença entre a data de cada pagamento e a data do seu empenho. A base unificada tem 26.125 registros e 40 colunas. A seleção estatística tem os mesmos 26.125 registros, com 16 colunas: o alvo original e 15 explicativas selecionadas entre 25 candidatas.

Fontes municipais: Execução Detalhada de Despesas e Despesas por Programas e Ações. Os seis CSVs de origem estão em `dados/`. A união e sua regra conservadora de exclusão de 114 pagamentos integralmente iguais estão documentadas no notebook de unificação e no README principal. Nenhuma linha adicional foi excluída na análise estatística.

## Relação com as aulas

| Etapa | Material de referência |
|---|---|
| Média, mediana, moda, quartis, variância e desvio padrão | Aula 2 e exercício de 18/08 |
| Correlação e gráfico de dispersão | Exercícios de 25/08 |
| Shapiro-Wilk | Aula 7, exercício de 15/09 e checkpoint da Aula 8 |
| Limites dos outliers e Box-Cox | Aula 7, slides 35 a 45, e exercício de 22/09 |
| Indicadores binários e regressões | Aula 10, slides 15 a 26 |

Os notebooks usam listas, `for`, `if` e as bibliotecas dos materiais. As duas operações para calcular datas já haviam sido autorizadas e explicadas na etapa de unificação. Os arquivos CSV e PNG de resultados são exportações de apoio dos objetos calculados, sem acrescentar métodos de exportação ao código didático.

## Correlações

Foram testadas 25 explicativas com o alvo original: 16 medidas orçamentárias, 7 indicadores binários de características da despesa e 2 valores monetários. Foram encontradas **15 correlações estritamente menores que -0,3 ou maiores que 0,3**.

A [tabela completa das 25 candidatas](correlacoes_25_variaveis.csv) contém os coeficientes sem arredondamento usado na seleção. Alguns exemplos:

| Característica | r |
|---|---:|
| Serviços médico-hospitalares, odontológicos e laboratoriais, máscara 33903950000000 | 0,476 |
| Subfunção Assistência Hospitalar e Ambulatorial | 0,464 |
| Categoria Processo | 0,446 |
| Saldo a liquidar da dotação | 0,422 |
| Empenho estimativo | 0,380 |
| Subfunção Atenção Básica | -0,375 |
| Categoria Diária | -0,419 |

Nos indicadores 0/1, o coeficiente compara a presença da característica com os demais registros. O sinal não demonstra efeito causal. Algumas explicativas orçamentárias têm relação contábil entre si; 15 correlações não significam 15 efeitos independentes. A configuração foi escolhida de forma exploratória depois de examinar outras perguntas, sem confirmação em dados novos.

## Descrição do intervalo

| Medida | Resultado |
|---|---:|
| Registros | 26.125 |
| Média | 75,84 dias |
| Mediana | 34 dias |
| Moda | 1 dia |
| Primeiro quartil | 7 dias |
| Terceiro quartil | 118 dias |
| Mínimo | 0 dias |
| Máximo | 504 dias |
| Desvio padrão populacional | 94,52 dias |

A diferença entre média e mediana e o [boxplot do intervalo](graficos/01_grafico.png) mostram uma distribuição com cauda longa para os valores altos. Os intervalos são observações reais do arquivo, e os extremos não foram declarados erros.

Na comparação por tipo de empenho:

| Tipo | Pagamentos | Média | Mediana |
|---|---:|---:|---:|
| Ordinário | 23.294 | 63,33 dias | 25 dias |
| Estimativo | 2.831 | 178,73 dias | 168 dias |

Cada pagamento recebe o mesmo peso nessa comparação; não há ponderação pelo valor pago. O intervalo maior dos estimativos deve ser interpretado considerando sua possibilidade de prever pagamentos ao longo de vários meses.

## Normalidade e tentativas de transformação

O Shapiro-Wilk foi executado porque consta no checkpoint. A documentação do SciPy informa que, acima de 5.000 observações, W é adequado, mas o p-valor pode ser impreciso. Além disso, os pagamentos compartilham empenhos e dotações, de modo que a independência não pode ser assumida. Os números abaixo são diagnósticos didáticos, não certificação de normalidade. [Documentação oficial](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.shapiro.html).

| Versão | W | p-valor aproximado |
|---|---:|---:|
| Original | 0,7788 | 8,591 × 10⁻¹⁰² |
| Limites de outliers | 0,7828 | 2,494 × 10⁻¹⁰¹ |
| Raiz quadrada | 0,9206 | 2,277 × 10⁻⁷⁷ |
| Box-Cox do original + 1 | 0,9596 | 3,957 × 10⁻⁶³ |
| Box-Cox após limites + 1 | 0,9513 | 6,305 × 10⁻⁶⁷ |

Os limites de 1,5 amplitude interquartil foram -159,5 e 284,5 dias. O experimento limitou 1.396 valores altos somente em uma lista separada. O alvo original foi preservado, pois esses pagamentos podem refletir compromissos previstos para vários meses depois do empenho.

Box-Cox exige valores positivos; por isso, foi somado 1 aos intervalos, inclusive aos zeros. O lambda estimado para a versão original foi aproximadamente 0,09909. A inversa, seguida da subtração de 1, recuperou os valores originais dentro da precisão numérica. [Documentação oficial do Box-Cox](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.boxcox.html).

**Nenhuma tentativa atingiu p > 0,05 no diagnóstico realizado.** Box-Cox aumentou W, mas não foi apresentado como normalização concluída. Os intervalos são discretos e reúnem diferentes perfis de despesa. A base final mantém o alvo original, e as tentativas ficam documentadas para discussão com o professor. As correlações do alvo transformado não foram confundidas com as 15 selecionadas no alvo original.

## Regressões exploratórias da Aula 10

Foram ajustados dois exemplos OLS com o intervalo original em dias. No simples, a explicativa é o indicador de empenho estimativo. No múltiplo, acrescentam-se categoria Processo e subfunção hospitalar e ambulatorial. Não foram colocadas todas as medidas orçamentárias relacionadas no mesmo modelo.

| Modelo | R² no próprio conjunto de ajuste | RMSE no ajuste |
|---|---:|---:|
| Simples | 0,1440 | 87,45 dias |
| Múltiplo | 0,3593 | 75,66 dias |

O múltiplo apresentou melhor ajuste histórico entre os dois exemplos. R² de 0,3593 não significa 35,93% de acerto em previsões. Ambos foram ajustados e avaliados nos mesmos registros, sem validação futura ou em dados separados.

A equação múltipla aproximada é:

`intervalo = 26,99 + 88,11 × estimativo + 66,02 × processo + 47,61 × hospitalar_ambulatorial`

Os três indicadores valem 0 ou 1. Os coeficientes expressam o ajuste conjunto, não uma mudança causada por cada característica.

A normalidade do alvo isolado não é requisito da OLS. Os resíduos e a estrutura de dependência precisam ser considerados para inferência. Os erros padrão e p-valores convencionais dos resumos não corrigem pagamentos compartilhando empenhos/dotações. Por isso, não foram usados para declarar confiabilidade preditiva ou formular previsões operacionais. Os resumos completos estão em [regressão simples](regressao_simples.txt) e [regressão múltipla](regressao_multipla.txt).

## Resposta e subsídio ao planejamento

No conjunto fornecido, existem associações entre o perfil das despesas e o intervalo entre empenho e pagamento. A comparação sugere utilidade em organizar e acompanhar calendários financeiros por tipo de empenho e perfil de despesa, em vez de tratar todos os pagamentos como se tivessem o mesmo comportamento.

Esses resultados podem orientar perguntas sobre a distribuição dos desembolsos ao longo do ano e apoiar o acompanhamento dos compromissos recorrentes. Não demonstram atraso, ineficiência, qualidade do atendimento ou efeito de determinado gasto. Para medir atraso seriam necessárias datas de vencimento.

Os valores orçamentários são os disponíveis na exportação; não foi reconstituída a informação conhecida em cada data. A base de pagamentos realizados não representa os ainda pendentes e não foi comprovada como a totalidade da despesa da Saúde. As conclusões se restringem ao conjunto analisado.

## Conferência do trabalho

Os mínimos de 20 mil registros, 25 candidatas e 15 correlações foram atingidos. O Shapiro-Wilk e as tentativas de transformação solicitados foram executados e registrados. O resultado não normal deve ser apresentado ao professor, pois não existe garantia de normalização pelos métodos tentados.

A pergunta e a interpretação final continuam sujeitas à aprovação do professor. Não foram fornecidos outros critérios específicos da entrega final ou os feedbacks do grupo. Este relatório não declara uma aprovação que ainda não foi informada.
