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