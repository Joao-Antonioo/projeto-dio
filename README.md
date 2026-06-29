Docker com Microsserviços — Load Balancing com Nginx e PHP

Projeto original criado por Denilson Bonatti para a Digital Innovation One (DIO).


Sobre o projeto

Demonstração prática de load balancing com Docker. O Nginx recebe as requisições e as distribui entre múltiplos containers PHP, que inserem dados num banco MySQL e registram qual container atendeu cada requisição.

Arquitetura:

Cliente
   ↓
[Nginx - porta 4500]
   ↙     ↓     ↘
[PHP]  [PHP]  [PHP]
         ↓
      [MySQL]


Melhorias aplicadas

1. Variáveis de ambiente no index.php

As credenciais do banco foram removidas do código e substituídas por variáveis de ambiente, evitando expor dados sensíveis no repositório.

php// antes
$servername = "54.234.153.24";
$username   = "root";
$password   = "Senha123";
$database   = "meubanco";

// depois
$servername = getenv("DB_HOST");
$username   = getenv("DB_USER");
$password   = getenv("DB_PASS");
$database   = getenv("DB_NAME");

Passe os valores na hora de subir o container:

bashdocker run -e DB_HOST=seu_host -e DB_USER=seu_usuario -e DB_PASS=sua_senha -e DB_NAME=seu_banco ...

2. Campo created_at no banco.sql

Adicionado um campo de timestamp na tabela para registrar automaticamente o horário de cada inserção, permitindo rastrear a sequência de requisições e qual container as atendeu.

sqlcreated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

Nenhuma alteração no PHP é necessária — o banco preenche o campo automaticamente.


Pré-requisitos


Docker
Conhecimentos básicos em Linux e Docker