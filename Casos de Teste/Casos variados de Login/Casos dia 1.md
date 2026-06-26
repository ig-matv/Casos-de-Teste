# Feature: Login

## Background

> Dado que o usuário está na tela de Login.

---

# Login com Sucesso

### Cenário: Login com credenciais válidas

```gherkin
Dado que existe um usuário ativo cadastrado
Quando ele informa um e-mail válido
E informa uma senha válida
E clica em "Entrar"
Então ele deve ser autenticado
E deve ser redirecionado para a Dashboard
```

### Cenário: Realizar login pressionando Enter

```gherkin
Dado que existe um usuário ativo cadastrado
Quando informa um e-mail válido
E informa uma senha válida
E pressiona a tecla Enter
Então o usuário deve ser autenticado
```

---

# Campos Obrigatórios

### Cenário: Login sem preencher e-mail

```gherkin
Quando informa apenas a senha
E clica em "Entrar"
Então o sistema deve informar que o e-mail é obrigatório
```

### Cenário: Login sem preencher senha

```gherkin
Quando informa apenas o e-mail
E clica em "Entrar"
Então o sistema deve informar que a senha é obrigatória
```

### Cenário: Login sem preencher nenhum campo

```gherkin
Quando clica em "Entrar"
Então o sistema deve informar que os campos obrigatórios devem ser preenchidos
```

---

# Validação de E-mail

### Cenário: Validar formato inválido do e-mail

```gherkin
Quando informa "<email>"
Então o sistema deve informar que o e-mail é inválido

Exemplos:

| email |
|-------|
| teste |
| teste@ |
| @gmail.com |
| teste@gmail |
| teste.com |
| teste@@gmail.com |
```

### Cenário: Login utilizando e-mail em letras maiúsculas

```gherkin
Dado que existe um usuário ativo
Quando informa o e-mail em letras maiúsculas
E informa uma senha válida
Então o login deve ser realizado com sucesso
```

### Cenário: Login utilizando e-mail com espaços antes e depois

```gherkin
Dado que existe um usuário ativo
Quando informa o e-mail com espaços extras
E informa uma senha válida
Então o sistema deve ignorar os espaços e realizar o login
```

---

# Senha

### Cenário: Validar máscara da senha

```gherkin
Quando digita a senha
Então todos os caracteres devem permanecer ocultos
```

### Cenário: Validar senha abaixo do tamanho mínimo

```gherkin
Quando informa uma senha menor que o limite permitido
Então o sistema deve impedir o login
```

### Cenário: Validar senha acima do tamanho máximo

```gherkin
Quando informa uma senha maior que o limite permitido
Então o sistema deve impedir o login
```

### Cenário: Login utilizando senha com caracteres especiais

```gherkin
Dado que existe um usuário cuja senha possui caracteres especiais
Quando informa a senha corretamente
Então o login deve ser realizado com sucesso
```

---

# Credenciais Inválidas

### Cenário: Login com e-mail inexistente

```gherkin
Quando informa um e-mail inexistente
E informa uma senha válida
Então o acesso deve ser negado
E deve ser exibida uma mensagem genérica
```

### Cenário: Login com senha incorreta

```gherkin
Quando informa um e-mail válido
E informa uma senha incorreta
Então o acesso deve ser negado
E deve ser exibida uma mensagem genérica
```

### Cenário: Login com e-mail e senha incorretos

```gherkin
Quando informa um e-mail inexistente
E informa uma senha incorreta
Então o acesso deve ser negado
E deve ser exibida uma mensagem genérica
```

---

# Segurança

### Cenário: Não permitir acesso via HTTP

```gherkin
Quando o usuário envia as credenciais utilizando HTTP
Então a comunicação deve ser redirecionada para HTTPS
```

### Cenário: Não exibir senha em texto puro

```gherkin
Quando o usuário digita sua senha
Então a senha nunca deve aparecer em texto puro
```

### Cenário: Validar proteção contra SQL Injection no campo e-mail

```gherkin
Quando informa "' OR 1=1 --" no campo e-mail
Então o login não deve ser realizado
```

### Cenário: Validar proteção contra SQL Injection na senha

```gherkin
Quando informa "' OR 1=1 --" na senha
Então o login não deve ser realizado
```

### Cenário: Validar proteção contra XSS

```gherkin
Quando informa "<script>alert('x')</script>"
Então o sistema não deve executar scripts
```

---

# Sessão

### Cenário: Criar sessão após login

```gherkin
Dado que o login foi realizado
Então uma sessão autenticada deve ser criada
```

### Cenário: Atualizar a página após login

```gherkin
Dado que o usuário está autenticado
Quando atualiza a página
Então o usuário deve permanecer autenticado
```

### Cenário: Expiração da sessão

```gherkin
Dado que a sessão expirou
Quando o usuário tenta acessar uma funcionalidade protegida
Então deve ser redirecionado para Login
```

---

# Acesso às Páginas

### Cenário: Acessar página protegida sem login

```gherkin
Dado que o usuário não está autenticado
Quando acessa diretamente uma URL protegida
Então deve ser redirecionado para Login
```

### Cenário: Acessar tela de Login estando autenticado

```gherkin
Dado que o usuário está autenticado
Quando tenta acessar a tela de Login
Então deve ser redirecionado para Dashboard
```

---

# Logout

### Cenário: Logout com sucesso

```gherkin
Dado que o usuário está autenticado
Quando clica em Logout
Então a sessão deve ser encerrada
E deve ser redirecionado para Login
```

### Cenário: Tentar voltar pelo navegador após logout

```gherkin
Dado que o usuário realizou Logout
Quando utiliza o botão Voltar do navegador
Então o sistema não deve permitir acesso às páginas protegidas
```

---

# Recuperação de Senha

### Cenário: Acessar recuperação de senha

```gherkin
Quando clica em "Esqueci minha senha"
Então deve ser direcionado para a tela de recuperação
```

---

# Lembrar-me

### Cenário: Login utilizando Lembrar-me

```gherkin
Dado que marca a opção "Lembrar-me"
Quando realiza login
Então o usuário deve permanecer autenticado após fechar e abrir o navegador
```

---

# Bloqueio

### Cenário: Bloquear usuário após várias tentativas inválidas

```gherkin
Dado que existe limite de tentativas configurado
Quando excede o número permitido de tentativas
Então a conta deve ser bloqueada temporariamente
```

---

# Disponibilidade

### Cenário: Serviço de autenticação indisponível

```gherkin
Dado que o serviço de autenticação está indisponível
Quando tenta realizar login
Então o sistema deve informar indisponibilidade
```

---

# Acessibilidade

### Cenário: Navegação utilizando teclado

```gherkin
Quando navega pela tela utilizando Tab
Então todos os elementos devem receber foco corretamente
```

### Cenário: Leitor de tela

```gherkin
Dado que utiliza leitor de tela
Então todos os campos devem possuir descrição acessível
```

---

# Responsividade

### Cenário: Login em dispositivo móvel

```gherkin
Dado que o usuário acessa pelo celular
Então todos os elementos da tela devem permanecer visíveis
```

### Cenário: Login em tablet

```gherkin
Dado que o usuário acessa pelo tablet
Então a tela de login deve ser exibida corretamente
```

---

# Performance

### Cenário: Tempo de autenticação

```gherkin
Quando realiza login com credenciais válidas
Então a autenticação deve ocorrer dentro do tempo esperado
```