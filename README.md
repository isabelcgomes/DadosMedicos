# Dados Medicos

Neste projeto vamos analisar uma base de dados médicos. Para proteger os dados, estes não serão disponibilizados junto com o repositório.

# Passo a Passo

Para a análise dos dados, foi necessário primeiro realizar uma triagem destes. Com uma tabela de 1,2GB e 1048576 linhas, cada uma delas representando um procedimento cirúrgico realizado pelo SUS, a limpeza inicial dos dados foi essencial para realizar qualquer tratamento e análise nestes.

Em mãos de um dicionário de dados, o primeiro tratamento foi remover todas as colunas com valores "Zerados", para reduzir a dimensionalidade dos dados e, com isso, diminuir seu tamanho. As colunas removidas nessa etapa foram:

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

Depois dessa primeira etapa, uma análise primária dos dados foi realizada com o objetivo de reduzir mais a quantidade de colunas. A partir disso, removeu-se as colunas vazias ou preenchidas com todos os valores identicos, isso porque essa informação não acrescenta valor ao dado analisado, uma vez que todos os registros têm o mesmo preenchimento. Nessa etapa as colunas removidas foram:

|Nome da Coluna|Descrição|Preenchimento|Nota|
|--------------|---------|-------------|----|
|ANO_CMPT | Ano de processamento | todos 2015 | porque a base de dados disponibilizada é a de dados processados em 2015
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

A partir dos dados pré-trabalhados, realizou-se um processo de avaliação da qualidade dos dados em cada um dos atributos do conjunto de dados. Como resultado desse processo, foram removidas as colunas:

| Nome da Coluna | Descrição | Tratamento | Nota |
| -------------- | --------- | ---------- | ---- |
| NASC | Data de nascimento do paciente | Coluna removida | Redundância com coluna Idade. A coluna Idade traz um valor maior para a análise de dados porque não exige tratamento adicional para ser avaliada |
| CEP | CEP do paciente | Coluna removida | Nível de detalhamento irrelevante para a análise |
| MES_CMTP | Mês do processamento | Coluna removida | Identificador irrelevante para a análise ou para identificação |
| NATUREZA | Natureza jurídica do hospital | Coluna removida | Redundância com coluna NAT_JUR e dado desatualizado |
| SEQUENCIA | Sequencial da AIH na remessa | Coluna Removida | Identificação redundante com a coluna N_AIH e irrelevante para a análise |
| REMESSA | Remessa de processamento da AIH | Coluna Removida | Identificação redundante com a coluna N_AIH e irrelevante para a análise |
<!-- | CBOR | Ocupação do paciente de acordo com a Classificação Brasileira de Ocupações | Coluna removida | Identificador irrelevante para a análise ou para identificação | -->
<!-- | DT_SAIDA | Data de saída | Coluna removida | Redundância com a coluna Quantidade de diárias. A coluna Quantidade de diárias traz um valor maior para a análise de dados porque não exige tratamento adicional para ser avaliada | -->


# Definição do Projeto

O objetivo da análise é propor melhorias no sistema de marcação de cirurgias eletivas para redução de filas. O atendimento dos pacientes em cirurgias eletivas é condicionado à quantidade de leitos disponíveis a internação da pessoa na preparação para a cirurgia e cuidados pós-operatórios. Existem duas principais variáveis que impactam na disponibilidade de leitos:

- Dinheiro: para a compra de novos leitos, o que, entretanto, aumenta custos operacionais do sistema de saúde
- Tempo de permanência no hospital: o que impacta na disponibilidade imediata do leito para o paciente futuro

Visando a manutenção do equilíbrio financeiro do sistema único de saúde, a variável resposta da presente análise é a quantidade de diárias (QT_DIARIAS), de forma que, o problema da pesquisa se torna:

_Quais as variáveis de maior impacto na quantidade de diárias que um paciente permanece em um hospital?_

Para responder essa pergunta, algumas considerações iniciais foram realizadas:

- Existem variáveis de alta correlação com a quantidade de diárias (QT_DIARIAS) que um paciente permanece em um hospital, entretanto, essas variáveis são influenciadas pela quantidade de diárias e não o contrário. Portanto, em uma primeira avaliação, foram removidas as variáveis:

| Nome da Coluna | Descrição                      | Motivo da Remoção      | 
| -------------- | ------------------------------ | ---------------------- |
| DIAR_ACOM      | Quantidade de diárias do acompanhante do paciente | Quanto mais diárias um paciente tem em um hospital mais um acompanhante pode ficar no hospital com este, entretanto, sem a existência de um paciente não há acompanhantes e, portanto, não há diárias de acompanhantes |
|VAL_SH| Valor de serviços hospitalares | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|VAL_SP| Valor de serviços profissionais | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|VAL_TOT| Valor total | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|US_TOT| Valor total em dólares | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|VAL_SH_FED| Valor do complemento federal de serviços hospitalares | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|VAL_SP_FED| Valor do complemento federal de serviços profissionais | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|VAL_SH_GES| Valor do complemento do gestor de serviços hospitalares | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|VAL_SP_GES| Valor do complemento do gestor de serviços profissionais | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |
|VAL_UCI| Valor de UCI | Os custos da internação de uma pessoa não interferem na quantidade de diárias, mas a quantidade de diárias pode interferir nos custos de uma internação em uma relação diretamente proporcional |

A correlação entre cada uma das variáveis em relação à quantidade de dias (QT_DIARIAS) de internação do paciente pode ser observada na [Figura 1](#Figura_1)

[Figura_1]: imagens\correlation_matrix_removal.png "Matriz de correlação entre as colunas removidas e a quantidade de diárias do paciente" 
![Figura 1](imagens\correlation_matrix_removal.png)

A [Figura 1](#Figura_1) indica uma alta correlação entre a quantidade de diárias do paciente e a quantidade de diárias do acompanhante e uma baixa correlação entre a quantidade de diárias do paciente e os custos associados à estadia deste. Não descarta-se, entretanto, uma possível relação entre a quantidade de diárias e os custos de internação, só indica-se que outros fatores, como o procedimento realizado, podem ter mais relação com os custos.