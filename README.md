# Modelagem de Banco de Dados - Plataforma de Troca de Livros

Este projeto apresenta a modelagem de banco de dados para uma **plataforma de economia colaborativa**, voltada ao compartilhamento e empréstimo de livros físicos entre usuários.  
O objetivo é fomentar a troca justa, sustentável e acessível, reduzindo custos e incentivando a circulação de conhecimento.

---

## 1. Objetivo da Plataforma
Criar uma base de dados que suporte as principais funcionalidades de uma plataforma de trocas de livros físicos, permitindo:
- Cadastro de usuários e seus endereços
- Registro de livros disponíveis para empréstimo
- Controle de empréstimos, histórico e avaliações
- Gestão de pontos (gamificação)
- Comunicação entre usuários
- Lista de desejos personalizada

---

## 2. Requisitos da Plataforma
### Requisitos Funcionais
- Usuários devem se cadastrar com dados básicos e pelo menos um endereço.
- Usuários podem cadastrar livros que possuem, vinculados a autores e gêneros.
- O sistema deve permitir gerenciar empréstimos (solicitação, aprovação, entrega, devolução).
- Deve existir um sistema de **avaliação** e **pontuação** para promover confiança.
- Usuários podem adicionar livros a uma lista de desejos.
- Comunicação via mensagens entre usuários deve ser registrada.
- Sistema de pontuação para incentivar participação ativa

### Requisitos Não Funcionais
- Segurança de dados sensíveis (senhas com hash).
- Escalabilidade para suportar crescimento do número de usuários/livros.
- Consistência dos dados (restrições de integridade no banco).

---

## 3. Identificação das Entidades e Atributos
### Usuários
- id_usuario (PK)
- nome  
- email (UNIQUE)
- senha (hash)  
- data_nascimento  
- telefone  
- reputacao  
- status_usuario  
- data_cadastro

### Livros
- id_livro (PK)  
- titulo  
- id_autor (FK)  
- id_genero (FK)  
- id_usuario_dono (FK)  
- id_condicao (FK)  
- ano_publicacao
- numero_paginas
- idioma
- sinopse
- editora  
- isbn
- disponivel (boolean)

### Autores
- id_autor (PK)  
- nome_autor
- nacionalidade  
- data_nascimento  

### Gênero
- id_genero (PK)  
- nome_genero  

### Condição do Livro
- id_condicao (PK)
- nome_condicao
- descricao  

### Pontuação
- id_transacao_pontos (PK)
- id_usuario (FK)
- id_emprestimo (FK) - nullable
- tipo_transacao (ganho, gasto, bonus, penalidade)
- quantidade_pontos
- descricao
- data_transacao 

### Empréstimo
- id_emprestimo (PK)
- id_livro (FK)
- id_usuario_solicitante (FK)
- id_usuario_proprietario (FK)
- id_status (FK)
- data_solicitacao
- data_aprovacao
- data_entrega_prevista
- data_entrega_real
- data_devolucao_prevista
- data_devolucao_real
- observacoes_proprietario
- observacoes_solicitante
- valor_pontos_transacao

### Avaliação
- id_avaliacao (PK)
- id_emprestimo (FK)
- id_usuario_avaliador (FK)
- id_usuario_avaliado (FK)
- tipo_avaliacao (como_proprietario, como_solicitante)
- nota (1-5)
- comentario
- data_avaliacao
- visivel (boolean)


### Endereço
- id_endereco (PK)  
- id_usuario (FK)
- tipo_endereço (residencial, trabalho, outro)
- cep  
- rua  
- numero  
- complemento  
- bairro  
- cidade  
- estado
- pais
- principal

### Mensagem
- id_mensagem (PK)
- id_conversa (FK)
- id_remetente (FK)
- conteudo
- data_envio
- lida (boolean)
- data_leitura
- tipo_mensagem (texto, sistema, emprestimo) 

### Lista de Desejos
- id_lista_desejo (PK)
- id_usuario (FK)
- id_livro (FK) - nullable
- titulo_desejado - caso não exista livro cadastrado
- id_autor (FK) - nullable
- prioridade (1-5)
- data_adicao
- notificar_disponibilidade (boolean)

### Status do Empréstimo
- id_status (PK)
- nome_status (Solicitado, Aprovado, Rejeitado, Em Trânsito, Emprestado, Devolvido, Atrasado, Cancelado)
- descricao
- permite_acao_usuario (boolean)

---

## 4. Relacionamentos
Relacionamentos 1:N

  Usuário → Endereços (1:N)
  Usuário → Livros Cadastrados (1:N)
  Usuário → Empréstimos como Solicitante (1:N)
  Usuário → Empréstimos como Proprietário (1:N)
  Livro → Empréstimos (1:N)
  Autor → Livros (1:N)
  Gênero → Livros (1:N)

Relacionamentos N:N

  Usuário ↔ Lista de Desejos ↔ Livros (N:N)
  Usuário ↔ Conversas ↔ Usuário (N:N)

Relacionamentos 1:1

  Usuário → Saldo de Pontos (1:1)

---

## 5. Diagrama Entidade-Relacionamento (DER)
![modelo conceitual](images/modelo_conceitual.jpg)

---

## 7. Autoria
Projeto desenvolvido para a disciplina **Laboratório de Banco de Dados**, por Liz Martins.
