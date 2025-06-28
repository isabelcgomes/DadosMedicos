# Dados Medicos

Neste projeto, uma base de dados do ministério da saúde será analisada para realizar uma avaliação de quais variáveis relatadas no prontuário de cirurgias eletivas mais contribuem para a permanência do paciente no hospital, a possibilidade de morte deste e o valor utilizado para custear a cirurgia.

# Tratamento

Nesta análise, primeiro foi realizado um tratamento dos dados para a escolha das variáveis mais relevantes para a análise. A primeira remoção de variáveis foram aquelas que, de acordo com o dicionário de dados disponibilizado pelo ministério estavam vazias ou desatualizadas, são estas:

- UTI_MES_IN
- UTI_MES_AN
- UTI_MES_AL
- UTI_INT_IN
- UTI_INT_AN
- UTI_INT_AL
- VAL_SADT
- VAL_RN
- VAL_ACOMP
- VAL_ORTP
- VAL_SANGUE
- VAL_SADTSR
- VAL_TRANSP
- VAL_OBSANG
- VAL_PED1AC
- DIAG_SECUN
- RUBRICA
- NUM_PROC
- TOT_PT_SP
- CPF_AUT

Com a remoção destas variáveis, foram analisadas as variáveis com valor igual em todos os registros, uma vez que essa informação se trataria de um filtro e não uma informação que acrescenta valor à análise. Dessa forma, as colunas removidas sob esse critério foram:

|Nome da Coluna|Descrição|Preenchimento|Nota|
|--------------|---------|-------------|----|
|ANO_CMPT | Ano de processamento | todos 2015 | porque a base de dados disponibilizada é a de dados processados em 2015 |
|IDENT | Identificação do tipo da AIH | todos 1 ||
|CAR_INT| Caráter da internação | todos vazios | |
|SEQ_AIH5| Sequencial de longa permanência | todos vazios | |
|GESTOR_DT| Data de autorização do gestor | todos vazios | |
|INFEHOSP| Status de infecção hospitalar | todos 0 | |
|CID_ASSO| CID Causa | todos vazios | dados pessoais |
|CID_MORTE| CID Morte | todos vazios | dados pessoais |
|AUD_JUST| Justificativa do gestor para aceitação | todos vazios | |
|SIS_JUST| Justificativa do estabelecimento para aceitação | todos vazios | |
|DIAGSEC5| Diagnóstico secundário 5 | todos vazios | |
|DIAGSEC6| Diagnóstico secundário 6 | todos vazios | |
|DIAGSEC7| Diagnóstico secundário 7 | todos vazios | |
|DIAGSEC8| Diagnóstico secundário 8 | todos vazios | |
|DIAGSEC9| Diagnóstico secundário 9 | todos vazios | |
|TPDISEC5| Tipo de diagnóstico secundário 5 | todos 0 | |
|TPDISEC6| Tipo de diagnóstico secundário 6 | todos 0 | |
|TPDISEC7| Tipo de diagnóstico secundário 7 | todos 0 | |
|TPDISEC8| Tipo de diagnóstico secundário 8 | todos 0 | |
|TPDISEC9| Tipo de diagnóstico secundário 9 | todos 0 | |

Por fim, foram removidas as colunas de identificação do prontuário porque estas trazem informações que não contribuem para a análise, mas para uma identificação e validação a posteriori dos resultados encontrados, foram estas:

|Nome da Coluna|Descrição|
|--------------|---------|
|N_AIH | Número de identificação do prontuário |
|REMESSA | Número da remessa do prontuário |

# Avaliação dos problemas

## Quantidade de dias de internação

O problema da quantidade de dias do paciente em internação trata-se de um problema de classificação em múltiplas classes. Isso se dá porque, o valor, em dias, adotado pelo ministério da saúde para registro dessa variável não é contínuo, e sim uma variável categórica que representa a quantidade de dias de permanência de um paciente no hospital (arredondada para cima), de forma que, se um paciente ficou 3 dias e 5 horas no hospital, no prontuário o registro será de 4 dias.

Dessa forma, foram analisados modelos de classificação para a apreciação do problema e, com base nos resultados de acurácia e precisão encontrados, o modelo com a melhor performance foi o Naive Bayes, modelo de classificação baseado no método bayesiano de probabilidade e estatística, com uma acurácia próxima a 60% no acerto da quantidade de dias de internação de um paciente no hospital com base nas variáveis avaliadas [Figura 1](#Figura_1).


[Figura_1]: imagens/GNB.png "Acurácia do método Gaussian Naive Bayes para o problema de quantidade de dias de internação" 
![Figura 1](imagens/GNB.png)

## Óbito

O problema da previsão de óbito trata-se de uma classificação binária, uma vez que um paciente pode ou não ir a óbito em decorrência de um procedimento cirurgico.

Para a avaliação deste problema também foi utilizado o método Naive Bayes de classificação com uma acurácia de 99,8% [Figura 2](#Figura_2).


[Figura_2]: imagens/GNB_obito.png "Acurácia do método Gaussian Naive Bayes para o problema de óbito" 
![Figura 2](imagens/GNB_obito.png)

## Valor total da operação e internação

Já o problema do valor, em reais, utilizado para custear a internação e operação do paciente é um problema de regressão, uma vez que a variável resposta é um valor numérico contínuo. Para este caso, o modelo escolhido para a apreciação foi a Árvore de Decisão em virtude da natureza das variáveis preditoras disponíveis na base de dados analisada, com um coeficiente de determinação de 70% [Figura 3](#Figura_3).


[Figura_3]: imagens/DT_val.png "Coeficiente de determinação para o problema de previsão do valor utilizado para custeio do procedimento e internação do paciente" 
![Figura 3](imagens/DT_val.png).
