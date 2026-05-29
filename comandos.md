# Exercício 1
SET usuario "Carlos"
GET usuario
SET usuario "Carlos Silva"
GET usuario

# Exercício 2
SET produto "Notebook" preco "3500" estoque "15"
MGET produto preco estoque

# Exercício 3
SETNX admin "root"
SETNX admin "outro_admin"
GET admin

# Exercício 4
SET nome "Maria"
APPEND nome " Oliveira"
STRLEN nome
GETRANGE nome 0 4

# Exercício 5
SET cidade "Campinas"
SETRANGE cidade 0 "São "
GET cidade

# Exercício 6
SET pontos 10
INCR pontos
INCRBY pontos 5
DECRBY pontos 3
GET pontos

# Exercício 7
SET saldo 100.50
INCRBYFLOAT saldo 25.75
GET saldo

# Exercício 8
SET token "abc123" EX 60
TTL token
PERSIST token
TTL token

# Exercício 9
SET sessao "ativa"
PEXPIRE sessao 5000
PTTL sessao

# Exercício 10
SET curso "Redis"
RENAME curso disciplina
COPY disciplina backup_disciplina
TYPE disciplina
EXISTS disciplina
DEL disciplina

# Exercício 11
SET minha_chave "valor"
MOVE minha_chave 1
SELECT 1
GET minha_chave
SELECT 0

# Exercício 12
SET chave1 "A"
SET chave2 "B"
SET chave3 "C"
SET chave4 "D"
SET chave5 "E"
KEYS *
SCAN 0

# Exercício 13
DEL tarefas
RPUSH tarefas "Estudar Redis" "Fazer Exercícios" "Dormir"
LRANGE tarefas 0 -1

# Exercício 14
LLEN tarefas
LINDEX tarefas 0
LINDEX tarefas -1

# Exercício 15
LSET tarefas 1 "Fazer exercícios de Redis"
LRANGE tarefas 0 -1

# Exercício 16
LINSERT tarefas BEFORE "Dormir" "Tomar Café"
LRANGE tarefas 0 -1

# Exercício 17
LPOP tarefas
RPOP tarefas
LRANGE tarefas 0 -1

# Exercício 18
DEL fila_atividade_pendente fila_atividade_processando
RPUSH fila_atividade_pendente "atividade 1" "atividade 2" "atividade 3"
LMOVE fila_atividade_pendente fila_atividade_processando LEFT RIGHT
LRANGE fila_atividade_pendente 0 -1
LRANGE fila_atividade_processando 0 -1

# Exercício 19
DEL logs_tarefas
RPUSH logs_tarefas "RPUSH tarefas" "LRANGE tarefas" "LLEN tarefas" "LINDEX tarefas 0" "LINDEX tarefas -1"
RPUSH logs_tarefas "LSET tarefas" "LINSERT tarefas" "LPOP tarefas" "RPOP tarefas" "LRANGE tarefas"
LTRIM logs_tarefas -5 -1
LRANGE logs_tarefas 0 -1

# Exercício 20
RPUSH tarefas "Dormir" "Dormir" "Dormir"
LREM tarefas 1 "Dormir"
LRANGE tarefas 0 -1
LREM tarefas 0 "Dormir"
LRANGE tarefas 0 -1

# Exercício 21
RPUSH tarefas "Estudar Redis" "Dormir" "Estudar Redis" "Dormir"
LPOS tarefas "Estudar Redis"
LPOS tarefas "Dormir" COUNT 10

# Exercício 22
LREM tarefas 0 "Dormir"
LREM tarefas 0 "Estudar Redis"
LRANGE tarefas 0 -1

# Exercício 23
DEL lista_temporaria
RPUSH lista_temporaria "item1" "item2"
EXPIRE lista_temporaria 10
TTL lista_temporaria
EXISTS lista_temporaria



EXERCÍCIO 24 - DESAFIO

Uma empresa desenvolveu um sistema web para gerenciamento de chamados técnicos internos. Todos os atendimentos realizados pelos usuários precisavam ser organizados em tempo real, já que muitos chamados chegavam simultaneamente e precisavam ser priorizados rapidamente.
O principal problema enfrentado pela equipe era identificar quais usuários estavam realizando mais solicitações, quais chamados eram mais frequentes e quais tarefas ainda estavam pendentes de atendimento. Além disso, o sistema precisava manter um histórico recente das ações executadas sem sobrecarregar o banco principal da aplicação.
Para resolver esse cenário, a equipe decidiu utilizar o Redis devido à sua velocidade e capacidade de manipulação de dados em memória.
No fluxo criado:
usuários são cadastrados no sistema;
chamados são adicionados em filas de atendimento;
atendimentos prioritários são inseridos no início da fila;
tarefas concluídas são removidas automaticamente;
logs recentes são armazenados em listas limitadas;
o sistema contabiliza quantos chamados cada usuário realizou;
sessões inválidas podem ser removidas rapidamente;
verificações de existência e tipo de chave são utilizadas constantemente.

# Cadastro de usuários usando HASH
HSET usuario:1001 nome "João Silva" setor "TI"
HSET usuario:1002 nome "Maria Souza" setor "Financeiro"
# Consultar dados de um usuário
HGETALL usuario:1001
# Adicionando chamados no final da fila
RPUSH fila_chamados "Chamado #001 - Computador não liga"
RPUSH fila_chamados "Chamado #002 - Sistema lento"
RPUSH fila_chamados "Chamado #003 - Erro no login"

# Ver todos os chamados da fila
LRANGE fila_chamados 0 -1
# Adicionando chamado prioritário no início da fila
LPUSH fila_chamados "Chamado #003 - Sem conexão com o banco-Urgente"
# Verificar nova ordem da fila
LRANGE fila_chamados 0 -1
# Remover o primeiro chamado da fila, simulando atendimento concluído
LPOP fila_chamados
# Verificar fila após atendimento
LRANGE fila_chamados 0 -1
# Criando logs recentes do sistema
LPUSH logs_sistema "Usuário 1001 abriu chamado"
LPUSH logs_sistema "Usuário 1002 abriu chamado"
LPUSH logs_sistema "Chamado prioritário adicionado"
LPUSH logs_sistema "Chamado atendido"
# Manter apenas os 5 logs mais recentes
LTRIM logs_sistema 0 4
# Visualizar logs
LRANGE logs_sistema 0 -1
# Contabilizar quantidade de chamados por usuário
INCR chamados_usuario:1001
INCR chamados_usuario:1001
INCR chamados_usuario:1002

# Consultar quantidade de chamados de um usuário
GET chamados_usuario:1001
GET chamados_usuario:1002
# Criar sessão de login para usuário
SET sessao:abc123 "usuario:1001"
# Definir tempo de expiração da sessão em 30 minutos
EXPIRE sessao:abc123 1800
# Verificar tempo restante da sessão
TTL sessao:abc123
# Remover sessão inválida manualmente
DEL sessao:abc123
# Verificar se a sessão ainda existe
EXISTS sessao:abc123
# Verificar se uma chave existe
EXISTS usuario:1001
EXISTS fila_chamados
# Verificar o tipo de dado de cada chave
TYPE usuario:1001
TYPE fila_chamados
TYPE logs_sistema
TYPE chamados_usuario:1001
TYPE ranking_chamados
# Contar quantos chamados ainda estão pendentes
LLEN fila_chamados

# Ver apenas os 3 primeiros chamados da fila
LRANGE fila_chamados 0 2
# Remover dados de teste, se necessário
DEL usuario:1001
DEL usuario:1002
DEL fila_chamados
DEL logs_sistema
DEL chamados_usuario:1001
DEL chamados_usuario:1002
DEL ranking_chamados
