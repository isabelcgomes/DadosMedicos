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

A partir dos dados pré-trabalhados, realizou-se um processo de engenharia de atributos para avaliar a qualidade das informações trazidas por cada um dos atributos do conjunto de dados. Como resultado desse processo, foram removidas as colunas:

| Nome da Coluna | Descrição | Tratamento | Nota |
| -------------- | --------- | ---------- | ---- |
| NASC | Data de nascimento do paciente | Coluna removida | Redundância com coluna Idade. A coluna Idade traz um valor maior para a análise de dados porque não exige tratamento adicional para ser avaliada |
| CEP | CEP do paciente | Coluna removida | Nível de detalhamento irrelevante para a análise |
| MES_CMTP | Mês do processamento | Coluna removida | Identificador irrelevante para a análise ou para identificação |
| NATUREZA | Natureza jurídica do hospital | Coluna removida | Redundância com coluna NAT_JUR e dado desatualizado |
<!-- | CBOR | Ocupação do paciente de acordo com a Classificação Brasileira de Ocupações | Coluna removida | Identificador irrelevante para a análise ou para identificação | -->
<!-- | DT_SAIDA | Data de saída | Coluna removida | Redundância com a coluna Quantidade de diárias. A coluna Quantidade de diárias traz um valor maior para a análise de dados porque não exige tratamento adicional para ser avaliada | -->