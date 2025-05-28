# 📖 API de Referência - Sistema de Gerenciamento de Biblioteca

## Modelos (Models)

### 👤 StudentExtra
Extensão do modelo User do Django para estudantes.

**Campos:**
- `user` (OneToOneField): Referência ao usuário Django
- `enrollment` (CharField): Número de matrícula (max 40 chars)
- `branch` (CharField): Curso/Departamento (max 40 chars)

**Métodos:**
- `__str__()`: Retorna "Nome[Matrícula]"
- `get_name` (property): Retorna o primeiro nome
- `getuserid` (property): Retorna o ID do usuário

### 📚 Book
Modelo para representar livros na biblioteca.

**Campos:**
- `name` (CharField): Nome do livro (max 30 chars)
- `isbn` (PositiveIntegerField): Código ISBN único
- `author` (CharField): Nome do autor (max 40 chars)
- `category` (CharField): Categoria do livro

**Categorias Disponíveis:**
- `education`: Educação
- `entertainment`: Entretenimento
- `comics`: Quadrinhos
- `biography`: Biografia
- `history`: História

**Métodos:**
- `__str__()`: Retorna "Nome[ISBN]"

### 📋 IssuedBook
Modelo para controle de empréstimos.

**Campos:**
- `enrollment` (CharField): Matrícula do estudante (max 30 chars)
- `isbn` (CharField): ISBN do livro emprestado (max 30 chars)
- `issuedate` (DateField): Data de empréstimo (auto preenchido)
- `expirydate` (DateField): Data de devolução (padrão: +15 dias)

**Funções Relacionadas:**
- `get_expiry()`: Calcula data de expiração (hoje + 15 dias)

## Formulários (Forms)

### 📝 ContactusForm
Formulário de contato para visitantes.

**Campos:**
- `Name` (CharField): Nome do remetente (max 30 chars)
- `Email` (EmailField): E-mail para resposta
- `Message` (CharField): Mensagem (max 500 chars, textarea)

### 👨‍💼 AdminSigupForm
Formulário de cadastro para administradores.

**Campos (baseado em User):**
- `first_name`: Primeiro nome
- `last_name`: Sobrenome
- `username`: Nome de usuário único
- `password`: Senha

### 👨‍🎓 StudentUserForm & StudentExtraForm
Formulários para cadastro de estudantes (separados em dois forms).

**StudentUserForm (baseado em User):**
- `first_name`: Primeiro nome
- `last_name`: Sobrenome
- `username`: Nome de usuário único
- `password`: Senha

**StudentExtraForm (baseado em StudentExtra):**
- `enrollment`: Número de matrícula
- `branch`: Curso/Departamento

### 📚 BookForm
Formulário para adicionar/editar livros.

**Campos (baseado em Book):**
- `name`: Nome do livro
- `isbn`: Código ISBN
- `author`: Nome do autor
- `category`: Categoria (choices)

### 📋 IssuedBookForm
Formulário para empréstimo de livros.

**Campos:**
- `isbn2` (ModelChoiceField): Seleção de livro disponível
- `enrollment2` (ModelChoiceField): Seleção de estudante

## Views (Controladores)

### 🏠 Views Públicas

#### `home_view(request)`
- **URL:** `/`
- **Template:** `library/index.html`
- **Acesso:** Público
- **Descrição:** Página inicial com lista de livros

#### `aboutus_view(request)`
- **URL:** `/aboutus`
- **Template:** `library/aboutus.html`
- **Acesso:** Público
- **Descrição:** Página sobre a biblioteca

#### `contactus_view(request)`
- **URL:** `/contactus`
- **Template:** `library/contactus.html`
- **Acesso:** Público
- **Descrição:** Formulário de contato

### 🔐 Views de Autenticação

#### `adminclick_view(request)` / `studentclick_view(request)`
- **URLs:** `/adminclick`, `/studentclick`
- **Acesso:** Público
- **Descrição:** Páginas de seleção de tipo de usuário

#### `adminsignup_view(request)` / `studentsignup_view(request)`
- **URLs:** `/adminsignup`, `/studentsignup`
- **Acesso:** Público
- **Descrição:** Cadastro de novos usuários

#### `afterlogin_view(request)`
- **URL:** `/afterlogin`
- **Acesso:** Autenticado
- **Descrição:** Redirecionamento baseado no tipo de usuário

### 👨‍💼 Views de Administrador

#### `addbook_view(request)`
- **URL:** `/addbook`
- **Decoradores:** `@login_required`, `@user_passes_test(is_admin)`
- **Template:** `library/addbook.html`
- **Descrição:** Adicionar novos livros

#### `viewbook_view(request)`
- **URL:** `/viewbook`
- **Decoradores:** `@login_required`, `@user_passes_test(is_admin)`
- **Template:** `library/viewbook.html`
- **Descrição:** Visualizar todos os livros

#### `issuebook_view(request)`
- **URL:** `/issuebook`
- **Decoradores:** `@login_required`, `@user_passes_test(is_admin)`
- **Template:** `library/issuebook.html`
- **Descrição:** Emprestar livros para estudantes

#### `viewissuedbook_view(request)`
- **URL:** `/viewissuedbook`
- **Decoradores:** `@login_required`, `@user_passes_test(is_admin)`
- **Template:** `library/viewissuedbook.html`
- **Descrição:** Ver todos os empréstimos

#### `viewstudent_view(request)`
- **URL:** `/viewstudent`
- **Decoradores:** `@login_required`, `@user_passes_test(is_admin)`
- **Template:** `library/viewstudent.html`
- **Descrição:** Gerenciar estudantes

### 👨‍🎓 Views de Estudante

#### `viewissuedbookbystudent(request)`
- **URL:** `/viewissuedbookbystudent`
- **Decorador:** `@login_required(login_url='studentlogin')`
- **Template:** `library/viewissuedbookbystudent.html`
- **Descrição:** Ver próprios empréstimos e multas

## Funções Auxiliares

### `is_admin(user)`
- **Parâmetro:** User object
- **Retorno:** Boolean
- **Descrição:** Verifica se o usuário pertence ao grupo 'ADMIN'

### `get_expiry()`
- **Retorno:** Date object (hoje + 15 dias)
- **Descrição:** Calcula data de expiração padrão para empréstimos

## Sistema de Permissões

### Grupos de Usuário
- **ADMIN:** Acesso completo ao sistema
- **STUDENT:** Acesso limitado às próprias informações

### Decoradores de Segurança
- `@login_required`: Requer autenticação
- `@user_passes_test(is_admin)`: Requer permissão de admin

## Templates Principais

### Layout Base
- `library/navbar.html`: Navegação geral
- `library/navbaradmin.html`: Navegação para admins
- `library/navbarstudent.html`: Navegação para estudantes
- `library/footer.html`: Rodapé do site

### Páginas de Autenticação
- `library/adminlogin.html`: Login de administrador
- `library/studentlogin.html`: Login de estudante
- `library/adminsignup.html`: Cadastro de administrador
- `library/studentsignup.html`: Cadastro de estudante

### Páginas Funcionais
- `library/addbook.html`: Formulário de adição de livros
- `library/viewbook.html`: Lista de livros
- `library/issuebook.html`: Formulário de empréstimo
- `library/viewissuedbook.html`: Lista de empréstimos
- `library/viewissuedbookbystudent.html`: Empréstimos do estudante
