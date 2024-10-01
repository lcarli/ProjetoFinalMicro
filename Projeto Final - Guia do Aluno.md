# Guia do Aluno: Modernização do Spring PetClinic com Micro Serviços e Azure

## Introdução ao Spring PetClinic

O **Spring PetClinic** é uma aplicação monolítica desenvolvida em **Java Spring Boot**, que simula o gerenciamento de uma clínica veterinária. A aplicação oferece funcionalidades como cadastro de clientes, veterinários e pets, além de agendamento e histórico de consultas.

### Funcionalidades Principais:
- **Cadastro de Clientes**: Gerencia os dados dos donos dos animais.
- **Cadastro de Veterinários**: Gerencia os dados dos veterinários da clínica.
- **Cadastro de Animais**: Permite o cadastro de pets e associa cada um a um cliente.
- **Consultas**: Permite o agendamento e o acompanhamento de consultas veterinárias.
- **Autenticação**: Veterinários e administradores podem se autenticar para gerenciar as consultas e o sistema.

[(Link para o projeto)](https://github.com/spring-projects/spring-petclinic)

---

## Casos de Uso do Sistema

A seguir estão os principais **casos de uso** da aplicação Spring PetClinic, representados em diagramas. Esses fluxos mostram como as interações entre usuários, API e banco de dados acontecem na arquitetura atual da aplicação.

### Caso de Uso 1: Cadastro de Clientes

```mermaid
sequenceDiagram
    participant User as Usuário
    participant Frontend as Frontend (HTML/JS)
    participant API as API Spring Boot
    participant DB as Banco de Dados (H2)

    User->>Frontend: Preenche formulário de cadastro
    Frontend->>API: Envia dados do cliente (POST /owners)
    API->>DB: Insere dados do cliente no banco de dados
    DB-->>API: Confirmação de inserção
    API-->>Frontend: Resposta de sucesso (Cliente cadastrado)
    Frontend-->>User: Confirmação de cadastro
```

---

### Caso de Uso 2: Agendamento de Consulta

```mermaid
sequenceDiagram
    participant User as Usuário
    participant Frontend as Frontend (HTML/JS)
    participant API as API Spring Boot
    participant DB as Banco de Dados (H2)
    participant Vet as Veterinário

    User->>Frontend: Seleciona data e horário da consulta
    Frontend->>API: Envia solicitação de agendamento (POST /visits)
    API->>DB: Grava agendamento no banco de dados
    DB-->>API: Confirmação de agendamento
    API-->>Frontend: Resposta de sucesso (Consulta agendada)
    Frontend-->>User: Consulta confirmada
    API-->>Vet: Notificação de nova consulta
```

---

### Caso de Uso 3: Exibição do Histórico de Consultas

```mermaid
sequenceDiagram
    participant User as Usuário
    participant Frontend as Frontend (HTML/JS)
    participant API as API Spring Boot
    participant DB as Banco de Dados (H2)

    User->>Frontend: Solicita histórico de consultas do pet
    Frontend->>API: Faz requisição (GET /pets/{id}/visits)
    API->>DB: Busca histórico de consultas no banco de dados
    DB-->>API: Retorna histórico de consultas
    API-->>Frontend: Envia histórico para exibição
    Frontend-->>User: Exibe histórico de consultas
```

---

### Caso de Uso 4: Upload de Imagem do Pet

```mermaid
sequenceDiagram
    participant User as Usuário
    participant Frontend as Frontend (HTML/JS)
    participant API as API Spring Boot
    participant Local as Servidor Local
    participant DB as Banco de Dados (H2)

    User->>Frontend: Envia imagem do pet
    Frontend->>API: Envia requisição de upload de imagem
    API->>Local: Armazena imagem no servidor
    API->>DB: Atualiza URL da imagem no banco de dados
    DB-->>API: Confirmação de atualização
    API-->>Frontend: Resposta de sucesso (Imagem enviada)
    Frontend-->>User: Imagem carregada com sucesso
```

---

### Caso de Uso 5: Login de Veterinários

```mermaid
sequenceDiagram
    participant Vet as Veterinário
    participant Frontend as Frontend (HTML/JS)
    participant API as API Spring Boot
    participant AuthDB as Banco de Dados de Usuários

    Vet->>Frontend: Envia credenciais de login
    Frontend->>API: Requisição de autenticação (POST /login)
    API->>AuthDB: Verifica credenciais no banco de dados
    AuthDB-->>API: Resposta com sucesso ou erro
    API-->>Frontend: Resposta com token de autenticação
    Frontend-->>Vet: Login efetuado com sucesso
```

---

### Caso de Uso 6: Gerenciamento de Veterinários

```mermaid
sequenceDiagram
    participant Admin as Administrador
    participant Frontend as Frontend (HTML/JS)
    participant API as API Spring Boot
    participant DB as Banco de Dados (H2)

    Admin->>Frontend: Solicita lista de veterinários
    Frontend->>API: Requisição para listar veterinários (GET /vets)
    API->>DB: Busca lista no banco de dados
    DB-->>API: Retorna lista de veterinários
    API-->>Frontend: Envia lista de veterinários
    Frontend-->>Admin: Exibe lista de veterinários
```

---

## Perguntas para Reflexão

Com base nos casos de uso apresentados e no que você aprendeu sobre **arquitetura em nuvem** e **micro serviços**, responda às seguintes perguntas:

1. **Divisão de Responsabilidades**:
   - Quais partes da aplicação podem ser separadas em micro serviços? Por quê?

2. **Escalabilidade e Resiliência**:
   - Como você pode garantir que a aplicação escale adequadamente com o aumento da demanda?
   - Quais serviços da nuvem poderiam ajudar a garantir alta disponibilidade e resiliência?

3. **Armazenamento de Dados**:
   - Onde e como os dados da aplicação devem ser armazenados para garantir segurança, escalabilidade e acesso rápido?

4. **Automação**:
   - Como você poderia implementar automação de deploy e integração contínua (CI/CD) para melhorar a eficiência no desenvolvimento e nas atualizações da aplicação?

5. **Comunicação entre Serviços**:
   - Se você transformar o sistema em micro serviços, como os diferentes serviços devem se comunicar entre si?
   - Como garantir que a comunicação seja eficiente e segura?

6. **Melhorias no Upload de Arquivos**:
   - Como o armazenamento de imagens e arquivos poderia ser feito de forma mais eficiente utilizando os recursos de nuvem?

7. **Segurança**:
   - Como você garantiria que a comunicação entre os diferentes componentes da aplicação é segura e que o acesso às informações sensíveis está protegido?

8. **Governança e Organização de Recursos**:
   - Como você organizaria os recursos no Azure para manter a governança, separação de responsabilidades e controle de custos?
   - Quais **Resource Groups** criaria e quais serviços incluiria em cada grupo?

9. **Gestão de Identidade e Acesso**:
   - Como você gerenciaria **Identidade e Acesso** utilizando **Azure Active Directory (AD)** e **RBAC (Role-Based Access Control)** para controlar quem pode acessar ou modificar os recursos?
   - Quais grupos AD criaria e quais permissões RBAC atribuiria a eles para gerenciar o acesso de forma segura?

---

Ao responder essas perguntas e refletir sobre os diagramas apresentados, você deverá identificar oportunidades de modernização para transformar o Spring PetClinic em uma aplicação mais eficiente e escalável, utilizando micro serviços e recursos do **Azure**.

---
