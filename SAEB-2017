 *BANCO DE DADOS SAEB 2017*

***Alterações do dia 08/05/2020
**************************///*** Professor ******************************


*importando banco de dados professor 2017*

import delimited "C:\Users\l_rod\Desktop\Trabalho de Conclusão de Curso\SAEB2017\TS_PROFESSOR.csv"

*Filtrando para utilizar apenas o 5 - Ensino Fundamental*

keep if id_serie == 5

**Professores da mesma turma com salario, experiencia e escolaridade iguas são considerados iguais*
*Experiência do professor: Q13: Há quantos anos você trabalha como professor(a)?
*Q10 Como professor, qual é, aproximadamente, o seu salário bruto? (com adicionais, se houver)
*Q4: Qual é o mais alto nível de escolaridade que você concluiu (até a graduação)?

duplicates drop id_turma tx_resp_q010 tx_resp_q013 tx_resp_q004  , force

*Filtrando professores que preencheram o questinario*

keep if in_preenchimento_questionario == 1

*Identificando as turmas com 2 professores*

gen x=1

egen z = sum(x) , by (id_turma)	

*Criando variavel para professor de matematica*

gen prof_mat = .

*Criando variavel para professor de portugues*

gen prof_port = .

*Considerando professor de matematica professores que responderam dar aula apenas de matematica em uma determinada turma (1º Criterio)
**Existe a possibilidade do professor dar aula de matemática, portugues, história, etc.
*Q105: Nesta turma, qual disciplina voce leciona?
*R: *A) Lingua portuguesa B) Matemática C) Mais de uma disciplina

replace prof_mat = 1 if tx_resp_q105 == "B"

*Considerando professor de portugues professores que responderam dar aula apenas de portugues em uma determinada turma* (1º Criterio)
**Existe a possibilidade do professor dar aula de matemática, portugues, história, etc.
*Q105: Nesta turma, qual disciplina voce leciona?
*R: *A) Lingua portuguesa B) Matemática C) Mais de uma disciplina

replace prof_port = 1 if tx_resp_q105 == "A"

*Considerando professor de matematica aqueles que não responderam nenhuma das questoes dos professores de portugues e reponderam pelo menos alguma das questoes dos professores de matematica (2º Criterio)	  
*Se você é professor de LÍNGUA PORTUGUESA, responda às questões de 114 a 119.
*Se você é professor de MATEMÁTICA, passe às questões de 120 a 125. 
*Se você é professor das DUAS DISCIPLINAS, responda às questões de 114 a 125.

gen np = 1 if tx_resp_q114 == "" & tx_resp_q115 == "" & tx_resp_q116 == "" & tx_resp_q117 == "" & tx_resp_q118 == "" & tx_resp_q119 == ""  

gen tm = 1 if tx_resp_q120 != "" | tx_resp_q121 != "" | tx_resp_q122 != "" | tx_resp_q123 != "" | tx_resp_q124 != "" | tx_resp_q125 != "" 

replace prof_mat = 1 if np==1 & tm == 1 & z==2 & prof_port==.

*Considerando professor de portugues aqueles que não responderam nenhuma das questoes dos professores de matematica e responderam pelo menos alguma das questoes dos professores de portugues*	(2º Criterio)
*Se você é professor de LÍNGUA PORTUGUESA, responda às questões de 114 a 119.
*Se você é professor de MATEMÁTICA, passe às questões de 120 a 125. 
*Se você é professor das DUAS DISCIPLINAS, responda às questões de 114 a 125.


gen nm = 1 if tx_resp_q120 == "" & tx_resp_q121 == "" & tx_resp_q122 == "" & tx_resp_q123 == "" & tx_resp_q124 == "" & tx_resp_q125 == "" 

gen tp = 1 if tx_resp_q114 != "" | tx_resp_q115 != "" | tx_resp_q116 != "" | tx_resp_q117 != "" | tx_resp_q118 != "" | tx_resp_q119 != ""  	

replace prof_port = 1 if nm == 1 & tp == 1 & z==2 & prof_mat==.


*Refinamento para caso o professor tenha respondido tanto as questoes de portugues quanto as de matematica, ou nao tenha respondido nenhuma das questoes dessa parte do questioanrio* (3º Criterio)

*Considerando professor de matematica, o professor que é formado em licenciatura em matematica*
*Q4: Qual é o mais alto nível de escolaridade que você concluiu (até a graduação)?
*R: F) Ensino superior - Licenciatura em matemática

replace prof_mat = 1 if prof_port == . &  tx_resp_q004 == "F" & z==2 


*Considerando professor de matematica, o professor que tem pos-graduacao com tematica em educaçao, enfatizando educação matematica*
*Q9: Indique a area tematica do curso de pos-gradução de mais alta titulação que possui.
*R: D) Educação, enfatizando educação matemática
 
replace prof_mat = 1 if prof_port == . &  tx_resp_q009 == "D" & z==2


*Considerando professor de portugues, o professor que é formado em licenciatura em letras*
*Q4: Qual é o mais alto nível de escolaridade que você concluiu (até a graduação)?
*R: G) Ensino superior - Licenciatura em letras

replace prof_port = 1 if prof_mat == . &  tx_resp_q004 == "G" & z==2


*Considerando professor de portugues, o professor que tem pós graduação com tematica em educação, entatizando alfabetização.*
*Q9: Indique a area tematica do curso de pos-gradução de mais alta titulação que possui.
*R: B) Educação, enfatizando alfabetização

replace prof_port = 1 if prof_mat == . &  tx_resp_q009 == "B" & z==2


*Considerando professor de portugeus, o professor que tem pós graduação com tematica em educação, entatizando linguistica ou letramento.*
*Q9: Indique a area tematica do curso de pos-gradução de mais alta titulação que possui.
*R: C) Educação, enfatizando linguistica e/ ou letramento

replace prof_port = 1 if prof_mat == . &  tx_resp_q009 == "C" & z==2


*Duplicando respostas para turmas com um unico professor*

replace prof_port =1 if z == 1

replace prof_mat =1 if z == 1

*Refinamente de dados , no caso de um professor da turma ser identificado e o outro nao*

*Dado que o professor de portugues foi encontrado, estamos supondo que o outro professor da turma é de matematica e vice-versa, visto que respondem o questionario apenas professores de portugues e matematica*

egen pt=sum( prof_port), by(id_turma)
egen mt=sum( prof_mat), by(id_turma)

replace prof_mat=1 if prof_port==. & pt>0

replace prof_port=1 if prof_mat==. & mt>0

*Como em muitas turmas os 2 professores não foram identificados faremos uma aproximacao pelo professor medio nessas turmas*

gen prof_ni = 1 if prof_mat==. & prof_port ==.

*Criando variaveis de analise*


*Salario professor não identificado, feito pela media dos dois professores não identificados da turma*
*Q10 Como professor, qual é, aproximadamente, o seu salário bruto? (com adicionais, se houver)

 gen salario_prof_ni =.
 
 replace salario_prof_ni = 937  if tx_resp_q010 == "A" & prof_ni == 1
 replace salario_prof_ni = 1171 if tx_resp_q010 == "B" & prof_ni == 1
 replace salario_prof_ni = 1640 if tx_resp_q010 == "C" & prof_ni == 1
 replace salario_prof_ni = 2108 if tx_resp_q010 == "D" & prof_ni == 1
 replace salario_prof_ni = 2577 if tx_resp_q010 == "E" & prof_ni == 1
 replace salario_prof_ni = 3045 if tx_resp_q010 == "F" & prof_ni == 1
 replace salario_prof_ni = 3514 if tx_resp_q010 == "G" & prof_ni == 1
 replace salario_prof_ni = 4217 if tx_resp_q010 == "H" & prof_ni == 1
 replace salario_prof_ni = 5622 if tx_resp_q010 == "I" & prof_ni == 1
 replace salario_prof_ni = 7965 if tx_resp_q010 == "J" & prof_ni == 1
 replace salario_prof_ni = 9370 if tx_resp_q010 == "K" & prof_ni == 1
 
 
 egen salario_prof_mat = mean(salario_prof_ni), by(id_turma)
 
 gen salario_prof_port = salario_prof_mat
 
 
*Experiencia professor não identificado, feito pela media dos dois professores não identificados da turma*
*Experiência do professor: Q13: Há quantos anos você trabalha como professor(a)?

 gen exp_prof_ni =. 

 replace exp_prof_ni = 0   if tx_resp_q013=="A" & prof_ni == 1
 replace exp_prof_ni = 1.5 if tx_resp_q013=="B" & prof_ni == 1
 replace exp_prof_ni = 4   if tx_resp_q013=="C" & prof_ni == 1
 replace exp_prof_ni = 8   if tx_resp_q013=="D" & prof_ni == 1
 replace exp_prof_ni = 13  if tx_resp_q013=="E" & prof_ni == 1
 replace exp_prof_ni = 18  if tx_resp_q013=="F" & prof_ni == 1
 replace exp_prof_ni = 20  if tx_resp_q013=="G" & prof_ni == 1
 
 egen exp_prof_mat = mean(exp_prof_ni), by(id_turma)
 
 gen exp_prof_port = exp_prof_mat
 
 *Escolaridade professor não identificado, feito pela media dos dois professores não identificados da turma*
 *Q4: Qual é o mais alto nível de escolaridade que você concluiu (até a graduação)?
 
 gen escolaridade_prof_ni =.
 
 replace escolaridade_prof_ni = 4  if tx_resp_q004 == "A" & prof_ni == 1
 replace escolaridade_prof_ni = 11 if tx_resp_q004 == "B" & prof_ni == 1
 replace escolaridade_prof_ni = 11 if tx_resp_q004 == "C" & prof_ni == 1
 replace escolaridade_prof_ni = 15 if tx_resp_q004 == "D" & prof_ni == 1
 replace escolaridade_prof_ni = 15 if tx_resp_q004 == "E" & prof_ni == 1
 replace escolaridade_prof_ni = 15 if tx_resp_q004 == "F" & prof_ni == 1
 replace escolaridade_prof_ni = 15 if tx_resp_q004 == "G" & prof_ni == 1
 replace escolaridade_prof_ni = 15 if tx_resp_q004 == "H" & prof_ni == 1
 replace escolaridade_prof_ni = 15 if tx_resp_q004 == "I" & prof_ni == 1
 
 egen escolaridade_prof_mat = mean(escolaridade_prof_ni), by(id_turma)
 
 gen escolaridade_prof_port = escolaridade_prof_mat 
 
 
 
*Salario professores identificados como de matematica*
*Salario bruto total do professor média da faixa
*Q10 Como professor, qual é, aproximadamente, o seu salário bruto? (com adicionais, se houver)

 
 replace salario_prof_mat = 937  if tx_resp_q010 == "A" & prof_mat == 1
 replace salario_prof_mat = 1171 if tx_resp_q010 == "B" & prof_mat == 1
 replace salario_prof_mat = 1640 if tx_resp_q010 == "C" & prof_mat == 1
 replace salario_prof_mat = 2108 if tx_resp_q010 == "D" & prof_mat == 1
 replace salario_prof_mat = 2577 if tx_resp_q010 == "E" & prof_mat == 1
 replace salario_prof_mat = 3045 if tx_resp_q010 == "F" & prof_mat == 1
 replace salario_prof_mat = 3514 if tx_resp_q010 == "G" & prof_mat == 1
 replace salario_prof_mat = 4217 if tx_resp_q010 == "H" & prof_mat == 1
 replace salario_prof_mat = 5622 if tx_resp_q010 == "I" & prof_mat == 1
 replace salario_prof_mat = 7965 if tx_resp_q010 == "J" & prof_mat == 1
 replace salario_prof_mat = 9370 if tx_resp_q010 == "K" & prof_mat == 1

 

 *Salario professores identificados como de portugues*
 *Salario bruto total do professor média da faixa
 *Q10 Como professor, qual é, aproximadamente, o seu salário bruto? (com adicionais, se houver)
 
 replace salario_prof_port = 937  if tx_resp_q010 == "A" & prof_port == 1
 replace salario_prof_port = 1171 if tx_resp_q010 == "B" & prof_port == 1
 replace salario_prof_port = 1640 if tx_resp_q010 == "C" & prof_port == 1
 replace salario_prof_port = 2108 if tx_resp_q010 == "D" & prof_port == 1
 replace salario_prof_port = 2577 if tx_resp_q010 == "E" & prof_port == 1
 replace salario_prof_port = 3045 if tx_resp_q010 == "F" & prof_port == 1
 replace salario_prof_port = 3514 if tx_resp_q010 == "G" & prof_port == 1
 replace salario_prof_port = 4217 if tx_resp_q010 == "H" & prof_port == 1
 replace salario_prof_port = 5622 if tx_resp_q010 == "I" & prof_port == 1
 replace salario_prof_port = 7965 if tx_resp_q010 == "J" & prof_port == 1
 replace salario_prof_port = 9370 if tx_resp_q010 == "K" & prof_port == 1
 
 
 
 *Experiencia professores identificados como de matematica*
 *Experiência do professor: Q13: Há quantos anos você trabalha como professor(a)?
 
 replace exp_prof_mat = 0   if tx_resp_q013=="A" & prof_mat == 1
 replace exp_prof_mat = 1.5 if tx_resp_q013=="B" & prof_mat == 1
 replace exp_prof_mat = 4   if tx_resp_q013=="C" & prof_mat == 1
 replace exp_prof_mat = 8   if tx_resp_q013=="D" & prof_mat == 1
 replace exp_prof_mat = 13  if tx_resp_q013=="E" & prof_mat == 1
 replace exp_prof_mat = 18  if tx_resp_q013=="F" & prof_mat == 1
 replace exp_prof_mat = 20  if tx_resp_q013=="G" & prof_mat == 1
 
 
 
 *Experiencia professores identificados como de portugues*
 *Experiência do professor: Q13: Há quantos anos você trabalha como professor(a)?
 
 replace exp_prof_port = 0   if tx_resp_q013=="A" & prof_port == 1
 replace exp_prof_port = 1.5 if tx_resp_q013=="B" & prof_port == 1
 replace exp_prof_port = 4   if tx_resp_q013=="C" & prof_port == 1
 replace exp_prof_port = 8   if tx_resp_q013=="D" & prof_port == 1
 replace exp_prof_port = 13  if tx_resp_q013=="E" & prof_port == 1
 replace exp_prof_port = 18  if tx_resp_q013=="F" & prof_port == 1
 replace exp_prof_port = 20  if tx_resp_q013=="G" & prof_port == 1
 
 
 *Escolaridade professores identificados como matematica*
 *Q4: Qual é o mais alto nível de escolaridade que você concluiu (até a graduação)?
 replace escolaridade_prof_mat = 4  if tx_resp_q004 == "A" & prof_mat == 1
 replace escolaridade_prof_mat = 11 if tx_resp_q004 == "B" & prof_mat == 1
 replace escolaridade_prof_mat = 11 if tx_resp_q004 == "C" & prof_mat == 1
 replace escolaridade_prof_mat = 15 if tx_resp_q004 == "D" & prof_mat == 1
 replace escolaridade_prof_mat = 15 if tx_resp_q004 == "E" & prof_mat == 1
 replace escolaridade_prof_mat = 15 if tx_resp_q004 == "F" & prof_mat == 1
 replace escolaridade_prof_mat = 15 if tx_resp_q004 == "G" & prof_mat == 1
 replace escolaridade_prof_mat = 15 if tx_resp_q004 == "H" & prof_mat == 1
 replace escolaridade_prof_mat = 15 if tx_resp_q004 == "I" & prof_mat == 1
 
 
 
 *Escolaridade professores identificados como portugues*
 *Q4: Qual é o mais alto nível de escolaridade que você concluiu (até a graduação)? 
 replace escolaridade_prof_port = 4  if tx_resp_q004 == "A" & prof_port == 1
 replace escolaridade_prof_port = 11 if tx_resp_q004 == "B" & prof_port == 1
 replace escolaridade_prof_port = 11 if tx_resp_q004 == "C" & prof_port == 1
 replace escolaridade_prof_port = 15 if tx_resp_q004 == "D" & prof_port == 1
 replace escolaridade_prof_port = 15 if tx_resp_q004 == "E" & prof_port == 1
 replace escolaridade_prof_port = 15 if tx_resp_q004 == "F" & prof_port == 1
 replace escolaridade_prof_port = 15 if tx_resp_q004 == "G" & prof_port == 1
 replace escolaridade_prof_port = 15 if tx_resp_q004 == "H" & prof_port == 1
 replace escolaridade_prof_port = 15 if tx_resp_q004 == "I" & prof_port == 1
 
 *///////*Hábitos de leitura/culturais
 *Q38: jornais e revistas
 
 gen professor_jornaisQ38 =.
 
 replace professor_jornaisQ38 = 1  if tx_resp_q038 == "A"
 replace professor_jornaisQ38 = 2  if tx_resp_q038 == "B"
 replace professor_jornaisQ38 = 3  if tx_resp_q038 == "C"
 
 *Q39: Livros
 
 gen professor_livrosQ39 =.
 
 replace professor_livrosQ39 = 1  if tx_resp_q039 == "A"
 replace professor_livrosQ39 = 2  if tx_resp_q039 == "B"
 replace professor_livrosQ39 = 3  if tx_resp_q039 == "C"
 
 *Q40: Sites na internet
 
 gen professor_internetQ40 =.
 
 replace professor_internetQ40 = 1  if tx_resp_q040 == "A"
 replace professor_internetQ40 = 2  if tx_resp_q040 == "B"
 replace professor_internetQ40 = 3  if tx_resp_q040 == "C"
 
 *Q41: Frequentar bibliotecas
 
 gen professor_bibliotecaQ41 =.
 
 replace professor_bibliotecaQ41 = 1  if tx_resp_q041 == "A"
 replace professor_bibliotecaQ41 = 2  if tx_resp_q041 == "B"
 replace professor_bibliotecaQ41 = 3  if tx_resp_q041 == "C"
 
 *Q42: Ir ao cinema
 gen professor_cinemaQ42 =.
 
 replace professor_cinemaQ42 = 1  if tx_resp_q042 == "A"
 replace professor_cinemaQ42 = 2  if tx_resp_q042 == "B"
 replace professor_cinemaQ42 = 3  if tx_resp_q042 == "C"
 
 *Q43: Ir ao teatro, museu, danca, musica
 
 gen professor_espetaculoQ43 =.
 
 replace professor_espetaculoQ43 = 1  if tx_resp_q043 == "A"
 replace professor_espetaculoQ43 = 2  if tx_resp_q043 == "B"
 replace professor_espetaculoQ43 = 3  if tx_resp_q043 == "C"
 
 
 **/////** Recursos pedagogicos
 
 *Q45 Livros de literatura em geral
 
 gen professor_livrosQ45 =.
 
 replace professor_livrosQ45 = 1  if tx_resp_q045 == "A"
 replace professor_livrosQ45 = 2  if tx_resp_q045 == "B"
 replace professor_livrosQ45 = 3  if tx_resp_q045 == "C"
 replace professor_livrosQ45 = 4  if tx_resp_q045 == "D"
 
*Q47 Filmes ou documentarios
 
 gen professor_filmesQ47 =.
 
 replace professor_filmesQ47 = 1  if tx_resp_q047 == "A"
 replace professor_filmesQ47 = 2  if tx_resp_q047 == "B"
 replace professor_filmesQ47 = 3  if tx_resp_q047 == "C"
 replace professor_filmesQ47 = 4  if tx_resp_q047 == "D"

 
 
 *Dividindo o banco para conseguir utilizar o "merge" pelo id_turma*
 
 save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_professores_todos_2017_inicial.dta"
 
 keep if prof_port == 1 | prof_ni == 1
 
 keep salario_prof_port exp_prof_port escolaridade_prof_port id_turma professor_jornaisQ38 professor_livrosQ39 professor_internetQ40 professor_bibliotecaQ41 professor_cinemaQ42 professor_espetaculoQ43 professor_livrosQ45 professor_filmesQ47
 
 save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_professores_port_2017.dta"
 
 
 clear
 
 
 use "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_professores_todos_2017_inicial.dta"
 
 keep if prof_mat == 1 | prof_ni == 1
 
 keep id_turma salario_prof_mat exp_prof_mat escolaridade_prof_mat professor_jornaisQ38 professor_livrosQ39 professor_internetQ40 professor_bibliotecaQ41 professor_cinemaQ42 professor_espetaculoQ43 professor_livrosQ45 professor_filmesQ47
 
 save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_professores_mat_2017.dta"
 
 clear


***********//////**********************************************************************************************************************************

**Do-file para o diretor da escola

*importando banco de dados diretor 2017*

import delimited "C:\Users\l_rod\Desktop\Trabalho de Conclusão de Curso\SAEB2017\TS_DIRETOR.csv"

*Filtrado variveis* - Não irei executar este comando para poder decidir depois qual serão as variáveis excluídas

keep id_escola tx_resp_q010 tx_resp_q008 tx_resp_q014 tx_resp_q016 tx_resp_q004

*Criando variaveis de analise*

*Experiência do diretor: Q16: Há quantos anos você exerce funções de direção?

 gen exp_dir =. 

 replace exp_dir = 0   if tx_resp_q016 == "A"
 replace exp_dir = 1.5 if tx_resp_q016 == "B"
 replace exp_dir = 4   if tx_resp_q016 == "C"
 replace exp_dir = 8   if tx_resp_q016 == "D"
 replace exp_dir = 13  if tx_resp_q016 == "E"
 replace exp_dir = 18  if tx_resp_q016 == "F"
 replace exp_dir = 20  if tx_resp_q016 == "G"

 
*Escolaridade diretor - Conversar com o professor Marcos - Esses valores não fazem muito sentido
*Q4: Qual é o mais alto nível de escolaridade que você concluiu (até a graduação)?
 
 
 gen escolaridade_dir =.
 
 replace escolaridade_dir = 4  if tx_resp_q004 == "A"
 replace escolaridade_dir = 11 if tx_resp_q004 == "B"
 replace escolaridade_dir = 11 if tx_resp_q004 == "C"
 replace escolaridade_dir = 15 if tx_resp_q004 == "D"
 replace escolaridade_dir = 15 if tx_resp_q004 == "E"
 replace escolaridade_dir = 15 if tx_resp_q004 == "F"
 replace escolaridade_dir = 15 if tx_resp_q004 == "G"
 replace escolaridade_dir = 15 if tx_resp_q004 == "H"
 replace escolaridade_dir = 15 if tx_resp_q004 == "I"
 
 *Salario bruto diretor méida das faixas*
 
 *Salario bruto total do diretor média da faixa
*Q10 Como diretor, qual é, aproximadamente, o seu salário bruto? (com adicionais, se houver)
 
gen salario_dir =.
 
replace salario_dir = 937  if tx_resp_q010 == "A"
replace salario_dir = 1171  if tx_resp_q010 == "B"
replace salario_dir = 1640 if tx_resp_q010 == "C"
replace salario_dir = 2108 if tx_resp_q010 == "D"
replace salario_dir = 2577 if tx_resp_q010 == "E"
replace salario_dir = 3045 if tx_resp_q010 == "F"
replace salario_dir = 3514 if tx_resp_q010 == "G"
replace salario_dir = 4217 if tx_resp_q010 == "H"
replace salario_dir = 5622 if tx_resp_q010 == "I"
replace salario_dir = 7965 if tx_resp_q010 == "J"
replace salario_dir = 9370 if tx_resp_q010 == "K"

 

*Dummy informa se o diretor assumiu a direção por meio de votação*
*Q14 Você assumiu a direção desta  Como diretor, qual é, aproximadamente, o seu salário bruto? (com adicionais, se houver)

gen 	dir_eleito = 1 if tx_resp_q014 == "B" | tx_resp_q014 == "E"
replace dir_eleito = 0 if dir_eleito != 1

 
*Dummy informa se o diretor tem pos-graduacao (mestrado ou doutorado)*
*Q8 - Indique o curso de pós-graduação de mais alta titulação que você possui:
*Alternativas de respostas - D - Mestrado E - Doutorado
 
gen 	dir_pos = 1 if tx_resp_q008 == "D" | tx_resp_q008 == "E"
replace dir_pos = 0 if dir_pos != 1


*Dummy  para informar se o diretor tem pos-graduacao (mestrado ou doutorado) ou especialazacao ou atualização
*Q8 - Indique o curso de pós-graduação de mais alta titulação que você possui:
*Alternativas de respostas: 
*A - Não fiz 
*B - Atualização
*C - Especialização
*D - Mestrado 
*E - Doutorado

 gen 	 dir_especialista = 1 if tx_resp_q008 == "D" | tx_resp_q008 == "E" | tx_resp_q008 == "B" | tx_resp_q008 == "C" 
 replace dir_especialista = 0 if dir_especialista!= 1
 

*Filtro final* - Pode ser alterado

keep id_escola dir_eleito salario_dir escolaridade_dir exp_dir dir_especialista dir_pos

save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_diretor_2017.dta"

clear


*************************************************************************************************************************************************************************

*Do-file para a ESCOLA SAEB 2017
*importando o banco de dados ESCOLA

import delimited "C:\Users\l_rod\Desktop\Trabalho de Conclusão de Curso\SAEB2017\TS_ESCOLA.csv"

*Filtrado variveis* - Não irei executar este comando para poder decidir depois qual serão as variáveis excluídas

keep id_escola tx_resp_q057 tx_resp_q059 tx_resp_q060 tx_resp_q063 tx_resp_q064 nivel_socio_economico nu_matriculados_censo_5ef nu_presentes_5ef taxa_participacao_5ef

*Criando variaveis para realizar a análise

*Dummy informa se a escola contem bliblioteca
*Questionamento: Indique a existência e as condições de uso dos seguintes espaços da escola:
*Q57: Biblioteca - a) Bom; b)Regular; c) Ruim; d) Inexistente

gen 	biblioteca = 0 if tx_resp_q057 == "D" 
replace biblioteca = 1 if tx_resp_q057 == "A" | tx_resp_q057 == "B" | tx_resp_q057 == "C" 


*Dummy informa se a escola contem quadra de esportes
*Questionamento: Indique a existência e as condições de uso dos seguintes espaços da escola:
*Q59: Quadra de esportes - a) Bom; b)Regular; c) Ruim; d) Inexistente

gen 	quadra = 0 if tx_resp_q059 == "D" 
replace quadra = 1 if tx_resp_q059 == "A" | tx_resp_q059 == "B" | tx_resp_q059 == "C"


*Dummy informa se a escola contem laboratorio de informatica*
*Questionamento: Indique a existência e as condições de uso dos seguintes espaços da escola:
*Q60: Laboratório de informática - a) Bom; b)Regular; c) Ruim; d) Inexistente

gen		informatica = 0 if tx_resp_q060 == "D" 
replace informatica = 1 if tx_resp_q060 == "A" | tx_resp_q060 == "B" | tx_resp_q060 == "C"

*Q63 - Sala para atividades de musica:

gen		musica_Q63 = 0 if tx_resp_q063 == "D" 
replace musica_Q63 = 1 if tx_resp_q063 == "A" | tx_resp_q063 == "B" | tx_resp_q063 == "C"

*Q64 - Sala para atividades de musica:

gen		artes_Q64 = 0 if tx_resp_q064 == "D" 
replace artes_Q64 = 1 if tx_resp_q064 == "A" | tx_resp_q064 == "B" | tx_resp_q064 == "C"


*Filtro final - Pode ser alterado
*Falar com o professor Marcos sobre quais anos utilizaremos

keep informatica quadra biblioteca id_escola nivel_socio_economico nu_matriculados_censo_5ef nu_presentes_5ef taxa_participacao_5ef musica_Q63 artes_Q64

save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_escola_2017.dta"

clear


*******************************************************************************************************************************************************************

*Do-file para o ALUNO

*Importando o banco de dados dos alunos 5EF
import delimited "C:\Users\l_rod\Desktop\Trabalho de Conclusão de Curso\SAEB2017\TS_ALUNO_5EF.csv"

*Realizando o filtro nas variáveis - Não irei executar este comando para poder decidir depois qual serão as variáveis excluídas
keep peso_aluno_mt in_prova_brasil id_turma id_uf id_regiao id_municipio id_area id_escola id_dependencia_adm id_localizacao id_turno id_serie id_aluno in_preenchimento_prova proficiencia_lp_saeb proficiencia_mt_saeb tx_resp_q001 tx_resp_q002 tx_resp_q003 tx_resp_q004 tx_resp_q005 tx_resp_q006 tx_resp_q007 tx_resp_q008 tx_resp_q009 tx_resp_q010 tx_resp_q011 tx_resp_q012 tx_resp_q013 tx_resp_q014 tx_resp_q015 tx_resp_q016 tx_resp_q017 tx_resp_q018 tx_resp_q019 tx_resp_q020 tx_resp_q021 tx_resp_q022 tx_resp_q023 tx_resp_q024 tx_resp_q025 tx_resp_q026 tx_resp_q027 tx_resp_q028 tx_resp_q029 tx_resp_q030 tx_resp_q031 tx_resp_q032 tx_resp_q033 tx_resp_q034 tx_resp_q035 tx_resp_q036 tx_resp_q037 tx_resp_q038 tx_resp_q039 tx_resp_q040 tx_resp_q041 tx_resp_q042 tx_resp_q043 tx_resp_q044 tx_resp_q045 tx_resp_q046 tx_resp_q047 tx_resp_q048 tx_resp_q049 tx_resp_q050 tx_resp_q051 

*Criando variáveis de análise

**Dummy informa se o aluno é do sexo feminino*
gen	feminino = 1 if tx_resp_q001 == "B"
replace feminino = 0 if tx_resp_q001 == "A"
drop tx_resp_q001

**Dummy informa se o aluno branco (Não brancos são considerados pretos pardos e indigenas e brancos são considerados brancos e amarelos)
gen 	branco = 0 if tx_resp_q002 == "B" | tx_resp_q002 == "C" | tx_resp_q002 == "E"
replace branco = 1 if tx_resp_q002 == "A" | tx_resp_q002 == "D"
drop tx_resp_q002

*Idade do aluno
gen     idade=.
replace idade=8  if tx_resp_q004=="A"
replace idade=9  if tx_resp_q004=="B"
replace idade=10 if tx_resp_q004=="C"
replace idade=11 if tx_resp_q004=="D"
replace idade=12 if tx_resp_q004=="E"
replace idade=13 if tx_resp_q004=="F"
replace idade=14 if tx_resp_q004=="G"
replace idade=15 if tx_resp_q004=="H"
drop tx_resp_q004

*Dummy para o caso de o aluno mora com a mae ou mulher responsavel*

gen     mora_mae=1 if tx_resp_q018=="A" | tx_resp_q018=="C"
replace mora_mae=0 if tx_resp_q018=="B" 

drop tx_resp_q018

*Escolaridade da mae (Como o IBGE utiliza na Pnad) 

gen escolaridade_mae=.

replace escolaridade_mae=0  if tx_resp_q019=="A"
replace escolaridade_mae=2  if tx_resp_q019=="B" 
replace escolaridade_mae=4  if tx_resp_q019=="C"
replace escolaridade_mae=8  if tx_resp_q019=="D"
replace escolaridade_mae=11 if tx_resp_q019=="E"
replace escolaridade_mae=15 if tx_resp_q019=="F"

drop tx_resp_q019

*Dummy informa se a mae (ou mulher resposavel) sabe ler e escrever

gen     mae_alfabetizada = 0 if tx_resp_q020  == "B"
replace mae_alfabetizada = 1 if  tx_resp_q020 == "A"

drop tx_resp_q020

*Dummy informa se o filho constuma ver a mae lendo*

gen 	mae_costuma_ler	= 0 if tx_resp_q021 == "B"
replace mae_costuma_ler	= 1 if tx_resp_q021 == "A"

drop tx_resp_q021

*Dummy se o aluno mora com a pai ou homem responsavel*

gen     mora_pai=1 if tx_resp_q022=="A" | tx_resp_q022=="C"
replace mora_pai=0 if tx_resp_q022=="B" 

drop tx_resp_q022

*Escolaridade do pai*

gen escolaridade_pai=.

replace escolaridade_pai=0  if tx_resp_q023=="A" 
replace escolaridade_pai=2  if tx_resp_q023=="B" 
replace escolaridade_pai=4  if tx_resp_q023=="C"
replace escolaridade_pai=8  if tx_resp_q023=="D"
replace escolaridade_pai=11 if tx_resp_q023=="E"
replace escolaridade_pai=15 if tx_resp_q023=="F"

drop tx_resp_q023

*Dummy informa se a pai (ou homem resposavel) sabe ler e escrever

gen     pai_alfabetizado = 0 if tx_resp_q024  == "B"
replace pai_alfabetizado = 1 if  tx_resp_q024 == "A"

drop tx_resp_q024

*Dummy informa se o filho constuma ver a pai lendo*

gen 	pai_costuma_ler	= 0 if tx_resp_q025 == "B"
replace pai_costuma_ler	= 1 if tx_resp_q025 == "A"

drop tx_resp_q025

*Dummy informa se os pais comparecem na reuniao de pais e mestres (Falar com o Marcos sobre a possibilidade de fazer numerica)*

gen 	reuniao_pais = 1 if tx_resp_q026 == "A" | tx_resp_q026 == "B"
replace reuniao_pais = 0 if tx_resp_q026 == "C"

*Dummy informa se os pais comparecem sempre ou quase sempre na reuniao de pais e mestres*

gen 	reuniao_pais_sempre = 1 if tx_resp_q026 == "A"
replace reuniao_pais_sempre = 0 if tx_resp_q026 == "B" | tx_resp_q026 == "C"

drop tx_resp_q026

*Dummy informa se os pais incentivam o estudo do aluno*

gen 	pais_incentivam_estudar = 1 if tx_resp_q027 == "A"
replace pais_incentivam_estudar = 0 if tx_resp_q027 == "B"

drop tx_resp_q027

*Dummy informa se os pais incentivam o aluno a fazer dever de casa*

gen 	pais_incentivam_tarefa 	= 1 if tx_resp_q028 == "A"
replace pais_incentivam_tarefa	= 0 if tx_resp_q028 == "B"

drop tx_resp_q028

*Dummy informa se os pais incentivam o aluno a ler*

gen 	pais_incentivam_ler = 1 if tx_resp_q029 == "A"
replace pais_incentivam_ler	= 0 if tx_resp_q029 == "B"

drop tx_resp_q029


*Dummy informa se os pais incentivam o aluno ir a escola*

gen 	pais_incentivam_ir_escola 	= 1 if tx_resp_q030 == "A"
replace pais_incentivam_ir_escola	= 0 if tx_resp_q030 == "B"

drop tx_resp_q030


*Dummy informa se os pais conversam sobre a escola*

gen 	pais_conversam_escola	= 1 if tx_resp_q031 == "A"
replace pais_conversam_escola	= 0 if tx_resp_q031 == "B"

drop tx_resp_q031

*Dummy informa se o aluno le jornais (Ver com o Marcos)*

gen 	le_jornais = 1  if tx_resp_q032 == "A" | tx_resp_q032 == "B"
replace le_jornais = 0  if tx_resp_q032 == "C" 

drop tx_resp_q032

*Dummy informa se o aluno le livros (Ver com o Marcos)*

gen 	le_livros = 1  if tx_resp_q033 == "A" | tx_resp_q033 == "B"
replace le_livros = 0  if tx_resp_q033 == "C" 

*Dummy informa se o aluno sempre le livro (Ver com o Marcos)

gen 	le_livros_sempre = 1  if tx_resp_q033 == "A" 
replace le_livros_sempre = 0  if tx_resp_q033 == "B" | tx_resp_q033 == "C"

drop tx_resp_q033

*Dummy informa se o aluno le revista (Ver com o Marcos)*

gen 	le_revista = 1  if tx_resp_q034 == "A" | tx_resp_q034 == "B"
replace le_revista = 0  if tx_resp_q034 == "C" 


*Dummy informa se o aluno sempre le revista (Ver com o Marcos)*

gen 	le_revista_sempre = 1  if tx_resp_q034 == "A" 
replace le_revista_sempre = 0  if tx_resp_q034 == "B" | tx_resp_q034 == "C"

drop tx_resp_q034


*Dummy informa se o aluno le gibi (Ver com o Marcos)*

gen 	le_gibi = 1  if tx_resp_q035 == "A" | tx_resp_q035 == "B"
replace le_gibi = 0  if tx_resp_q035 == "C" 


*Dummy informa se o aluno sempre le gibi*

gen 	le_gibi_sempre = 1  if tx_resp_q035 == "A" 
replace le_gibi_sempre = 0  if tx_resp_q035 == "B" | tx_resp_q035 == "C"

drop tx_resp_q035

*Dummy informa se o aluno le noticias ou blogs na internet (Ver com o Marcos)*

gen 	le_internet = 1  if tx_resp_q036 == "A" | tx_resp_q036 == "B"
replace le_internet = 0  if tx_resp_q036 == "C" 

*Dummy informa se o aluno sempre le internet*

gen 	le_internet_sempre = 1  if tx_resp_q036 == "A" 
replace le_internet_sempre = 0  if tx_resp_q036 == "B" | tx_resp_q036 == "C"

drop tx_resp_q036

*Dummy informa se o aluno costuma frenquentar  biblioteca (Ver com o Marcos) *

gen 	frequenta_biblioteca = 1  if tx_resp_q037 == "A" | tx_resp_q037 == "B"
replace frequenta_biblioteca = 0  if tx_resp_q037 == "C" 

*Dummy informa se o aluno sempre frenquenta biblioteca*

gen 	frequenta_biblioteca_sempre = 1  if tx_resp_q037 == "A" 
replace frequenta_biblioteca_sempre = 0  if tx_resp_q037 == "B" | tx_resp_q037 == "C"

drop tx_resp_q037

*Dummy informa se o aluno costuma frenquentar  cinema (Ver com o Marcos) (Ideia esta pergunta poderia se incuida o indice socio economico)*

gen 	frequenta_cinema = 1  if tx_resp_q038 == "A" | tx_resp_q038 == "B"
replace frequenta_cinema = 0  if tx_resp_q038 == "C" 


*Dummy informa se o aluno sempre frenquenta cinema*

gen 	frequenta_cinema_sempre = 1  if tx_resp_q038 == "A" 
replace frequenta_cinema_sempre = 0  if tx_resp_q038 == "B" | tx_resp_q038 == "C"

drop tx_resp_q038

*Dummy informa se o aluno costuma frenquentar espetaculo(teatro, museu, dança ou música) (OLHAR) (Ideia esta pergunta poderia se incuida o indice socio economico)*

gen 	frequenta_espetaculo = 1  if tx_resp_q039 == "A" | tx_resp_q039 == "B"
replace frequenta_espetaculo = 0  if tx_resp_q039 == "C" 

*Dummy informa se o aluno costuma frequntar sempre espetaculo*

gen 	frequenta_espetaculo_sempre = 1  if tx_resp_q039 == "A" 
replace frequenta_espetaculo_sempre = 0  if tx_resp_q039 == "B" | | tx_resp_q039 == "C"

drop tx_resp_q039


*Variavel informa quanto tempo o aluno gasta fazendo trabalhos domesticos
gen trab_domestico =.

replace trab_domestico = 0 if tx_resp_q041 == "E"
replace trab_domestico = 1 if tx_resp_q041 == "A"
replace trab_domestico = 2 if tx_resp_q041 == "B"
replace trab_domestico = 3 if tx_resp_q041 == "C"
replace trab_domestico = 4 if tx_resp_q041 == "D"

drop tx_resp_q041


*Dummy informa se o aluno trabalha fora de casa*

gen 	trabalha = 1 if tx_resp_q042 == "A"
replace trabalha = 0 if tx_resp_q042 == "B"

drop tx_resp_q042

*Dummy informa se o aluno faz tarefa de portugues*

gen 	tarefa_port = 1 if tx_resp_q047 == "A"
replace tarefa_port = 0 if tx_resp_q047 == "B"

drop tx_resp_q047

*Dummy informa se o aluno faz tarefa de matematica*

gen 	tarefa_mat = 1 if tx_resp_q049 == "A"
replace tarefa_mat = 0 if tx_resp_q049 == "B"

drop tx_resp_q049

*Dummy informa se o aluno ja foi reprovado*

gen 	reprovado = 1 if tx_resp_q045 == "B" | tx_resp_q045 == "C"
replace reprovado = 0 if tx_resp_q045 == "A"

drop tx_resp_q045

*Filtro final


*Salvando 
save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_alunos_2017.dta"

****
********************************//// incomporando ////	*****************************
****	
		
*Incomporando variáveis de professor de matematica*

merge m:m id_turma using "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_professores_mat_2017.dta" 

drop _merge

*Incomporando varaiveis professor de portugues

merge m:m id_turma using "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_professores_port_2017.dta"  

drop _merge

*Incorporando variaveis do diretor*

merge m:m id_escola using "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_diretor_2017.dta"  

drop _merge

*Incoporando variaveis da escola*

merge m:m id_escola using "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_escola_2017.dta" 

drop _merge

*Retirando os alunos que não preencheram o questionario*

drop if in_preenchimento_prova == 0

drop in_preenchimento_prova

*Mantendo apenas Prova Brasil

drop if in_prova_brasil == 0

drop in_prova_brasil

drop _merge

*salvando o resultado dos merges

save save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_merges_final_2017.dta" 


**************************************************************************************************************************************************************************************************************

*Indice socio economico. Foram utilizadas as variavies da nota tecnica inep 2015 inse, retirando apenas as variaveis de nivel educacional dos pais as quais serão utilizadas como controles nas regresoes*
*Ler os artigos:
*http://download.inep.gov.br/informacoes_estatisticas/indicadores_educacionais/2015/nota_tecnica/nota_tecnica_inep_inse_2015.pdf
*http://download.inep.gov.br/informacoes_estatisticas/indicadores_educacionais/2011_2013/nivel_socioeconomico/nota_tecnica_indicador_nivel_socioeconomico.pdf
**Não encontrei o de 2017
	
*Televisao*

 gen     tv=.
 
 replace tv=0 if tx_resp_q005=="A"
 replace tv=1 if tx_resp_q005=="B"
 replace tv=2 if tx_resp_q005=="C"
 replace tv=3 if tx_resp_q005=="D"
 replace tv=4 if tx_resp_q005=="E"	
 
 drop tx_resp_q005
 
 
*Geladeira (valores alterados devido a 2011)*

 gen     geladeira=.
 
 replace geladeira=0 if tx_resp_q008=="A"
 replace geladeira=1 if tx_resp_q008=="B"
 replace geladeira=2 if tx_resp_q008=="C" | tx_resp_q008=="D" | tx_resp_q008=="E"
 
 drop tx_resp_q008
 
 *Frezeer 
 
 gen 	 frezeer = 1 if tx_resp_q009 == "B" |  tx_resp_q009 == "C" | tx_resp_q009 == "D" | tx_resp_q009 == "E" | tx_resp_q010 == "B" |  tx_resp_q010 == "C" | tx_resp_q010 == "D" | tx_resp_q010 == "E"
 replace frezeer = 0 if frezeer != 1
 
 drop tx_resp_q009 tx_resp_q010
 
 *Maquina de lavar roupa
 
 gen     maquina_lavar_roupa=.
 
 replace maquina_lavar_roupa=0 if tx_resp_q011=="A"
 replace maquina_lavar_roupa=1 if tx_resp_q011=="B" | tx_resp_q011=="C" | tx_resp_q011=="D" |  tx_resp_q011=="E"	
 
 drop tx_resp_q011
 
 
 *Automovel*
 
 gen     automovel=.
 
 replace automovel=0 if tx_resp_q012=="A"
 replace automovel=1 if tx_resp_q012=="B"
 replace automovel=2 if tx_resp_q012=="C"
 replace automovel=3 if tx_resp_q012=="D" | tx_resp_q012=="E"	
 
 drop tx_resp_q012
 
 *Computador*
 
 gen     pc=.
 
 replace pc=0 if tx_resp_q013=="A"
 replace pc=1 if tx_resp_q013=="B" | tx_resp_q013=="C" | tx_resp_q013=="D" | tx_resp_q013=="E"	
 
 drop tx_resp_q013

 *Banheiro*
 
 gen     banheiro=.
 
 replace banheiro=0 if tx_resp_q014=="A"
 replace banheiro=1 if tx_resp_q014=="B"
 replace banheiro=2 if tx_resp_q014=="C"
 replace banheiro=3 if tx_resp_q014=="D"
 replace banheiro=4 if tx_resp_q014=="E"
 
 drop tx_resp_q014
 
 *Dividindo o número de quartos por pessoa*
 
 gen     quarto=0
 
 replace quarto=1 if tx_resp_q015=="B"
 replace quarto=2 if tx_resp_q015=="C"
 replace quarto=3 if tx_resp_q015=="D"
 replace quarto=4 if tx_resp_q015=="E"	
 
 gen     n_pessoas=1 if tx_resp_q016=="A"
 replace n_pessoas=2 if tx_resp_q016=="B"
 replace n_pessoas=3 if tx_resp_q016=="C"
 replace n_pessoas=4 if tx_resp_q016=="D"
 replace n_pessoas=5 if tx_resp_q016=="E"	
 replace n_pessoas=6 if tx_resp_q016=="F"
 
 gen quarto_por_pessoa = quarto / n_pessoas
 
 
 drop tx_resp_q015 tx_resp_q016
 
 *Empregada*
 
 gen     empregada=1 if tx_resp_q017 == "B" | tx_resp_q017 == "C" | tx_resp_q017 == "D" | tx_resp_q017 == "E"	
 replace empregada=0 if tx_resp_q017 == "A"
 
 drop tx_resp_q017
 
 
****************************///*****************

 *Analise de componentes principais para a criação do indice de nivel socioeconomico do aluno
 
 pca tv pc geladeira frezeer automovel banheiro quarto_por_pessoa empregada maquina_lavar_roupa
 
 *Teste KMO, mede se as variáveis são suficientemente comuns para garantir que os componentes tenham poder de explicação*
 
 estat kmo
 
 *Teste, verifica o numero de componentes que deve ser utilizado*///
 
 fapara, pca reps(10)
 
 *Criando variavel de nivel socioeconomico*
 
 predict nivel_socioeco_aluno1,score
 
*Padronizando nível socioeconômico*

egen ns_pad_aluno=std(nivel_socioeco_aluno1)

	
**************************************************///****************************************************************************************************************

*08/06/2020*

sum nivel_socioeco_aluno1 [aw=peso], detail
gen ns_baixo=0
replace ns_baixo=1 if nivel_socioeco_aluno1<-1.152724
gen ns_mediobaixo=0
replace ns_mediobaixo=1 if nivel_socioeco_aluno1>-1.152723 & nivel_socioeco_aluno1<-0.0577654
gen ns_medioalto=0
replace ns_medioalto=1 if nivel_socioeco_aluno1>-0.0577653 & nivel_socioeco_aluno1<0.9861624
gen ns_alto=0
replace ns_alto=1 if nivel_socioeco_aluno1>0.9861625

tab ns_baixo
tab ns_mediobaixo
tab ns_medioalto
tab ns_alto


gen ns_baixo=0 if nivel_socioeco_aluno1 ~=.

replace ns_baixo=1 if nivel_socioeco_aluno1<-1.152724

gen ns_mediobaixo=0 if nivel_socioeco_aluno1~=.

replace ns_mediobaixo=1 if nivel_socioeco_aluno1>-1.152723 & nivel_socioeco_aluno1<-0.0577654

gen ns_medioalto=0 if nivel_socioeco_aluno1~=.

replace ns_medioalto=1 if nivel_socioeco_aluno1>-0.0577653 & nivel_socioeco_aluno1<0.9861624

gen ns_alto=0 if nivel_socioeco_aluno1~=.

replace ns_alto=1 if nivel_socioeco_aluno1>0.9861625

*não é bonito, mas funcionou:
replace ns_alto=1 if ns_baixo==0 & ns_mediobaixo==0 & ns_medioalto==0	
*


*Ajustando o banco para o empilhamento de todos os anos*

*Renomeando para o empilhamento. OBS: varivel peso_aluno_lp e peso_aluno_mt são iguais*

rename peso_aluno_mt peso

*Filtrando alunos que não respoderam questões suficientes sobre o nivel_socio_economico*

drop if nivel_socioeco_aluno1 == .

*Criando variavel para reconhecimento de ano do censo*

gen ano = 2017

save "C:\Users\l_rod\Desktop\Banco de Dados novo 31-08-2020\filtro_merges_final_niveis_socioeconomicos_2017.dta"








