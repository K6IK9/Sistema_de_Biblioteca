# 📚 Sistema de Gerenciamento de Biblioteca - Django

[![Maintenance](https://img.shields.io/badge/maintained-yes-green.svg)](https://github.com/rajaprerak/MusicPlayer/commits/master)
[![License](http://img.shields.io/:license-mit-blue.svg?style=flat-square)](http://badges.mit-license.org)
[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Django](https://img.shields.io/badge/Django-3.0.5-green.svg)](https://www.djangoproject.com/)

## 📋 Sobre o Projeto

O **Django Library Management** é um sistema web completo para gerenciamento de bibliotecas, desenvolvido com Django. O sistema permite o controle eficiente de livros, usuários (estudantes e administradores), empréstimos e multas.

### 🛠️ Tecnologias Utilizadas

- **Backend:** Python 3.x, Django 3.0.5
- **Frontend:** HTML5, CSS3, JavaScript
- **Banco de Dados:** SQLite (padrão)
- **Bibliotecas:** django-widget-tweaks
- **Autenticação:** Sistema de autenticação integrado do Django



## 🚀 Instalação e Configuração

### Instalação Rápida
```bash
git clone https://github.com/BurhanMohammad/Django-librarymanagement.git
cd Django-librarymanagement
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

📖 **Para instruções detalhadas, consulte o [Guia de Instalação](INSTALACAO.md)**

## ✨ Funcionalidades do Sistema

### 🌐 Funcionalidades Públicas (Visitantes)
- ✅ Visualizar todos os livros na página inicial
- ✅ Pesquisar livros por autor, nome ou categoria
- ✅ Ordenar livros e autores alfabeticamente
- ✅ Visualizar informações sobre a biblioteca
- ✅ Formulário de contato

### 👨‍🎓 Funcionalidades para Estudantes
- ✅ **Autenticação:**
  - Login e cadastro seguros
  - Gerenciamento de perfil

- ✅ **Gestão de Empréstimos:**
  - Solicitar empréstimo de livros
  - Visualizar histórico próprio de empréstimos
  - Filtrar empréstimos por status:
    - Solicitações pendentes
    - Livros emprestados
    - Histórico completo

- ✅ **Sistema de Multas:**
  - Consultar multas pendentes
  - Visualizar dias restantes para devolução
  - Ver número de dias em atraso
  - Pagamento online de multas (integração RazorPay)

### 👨‍💼 Funcionalidades para Administradores
- ✅ **Dashboard Administrativo:**
  - Painel de controle centralizado
  - Visão geral do sistema

- ✅ **Gestão de Empréstimos:**
  - Visualizar todos os empréstimos
  - Aprovar/rejeitar solicitações
  - Definir data de devolução manualmente ou automaticamente
  - Pesquisar empréstimos por ID do estudante
  - Filtrar por status (emprestado/devolvido)

- ✅ **Gestão de Livros:**
  - Adicionar novos livros
  - Editar informações de livros
  - Remover livros do acervo
  - Pesquisar e filtrar por autor/categoria

- ✅ **Gestão de Usuários:**
  - Adicionar/remover estudantes
  - Modificar informações de usuários
  - Filtrar por departamento
  - Visualizar histórico completo de cada usuário
  - Alterar senhas de usuários

- ✅ **Sistema de Multas:**
  - Calcular multas automaticamente
  - Criar e gerenciar multas manualmente
  - Pesquisar multas por ID do estudante
  - Alternar status de pagamento (dinheiro/online)

### 🔧 Funcionalidades Técnicas Avançadas
- ✅ **Validação Inteligente:**
  - Verificação em tempo real de ID de estudante duplicado
  - Validação sem recarregamento de página

- ✅ **Status Dinâmico:**
  - Indicadores visuais de status dos livros
  - Atualização automática baseada no usuário logado

- ✅ **Segurança:**
  - Sistema de grupos (ADMIN/STUDENT)
  - Controle de acesso baseado em permissões
  - Proteção CSRF integrada

---

## Usage:
### To install Django Music Player, follow these steps:
## 1. Run the server:

```bash
  python manage.py runserver
```

## 2. Open your web browser and go to:
>'http://localhost:8000/'
## 3 . Sign up for a new account or log in with an existing one.

## 4. Add some books to the library.

## 5. Borrow and return books as needed.

## 6 . Make online payment using Razorpay payment gateway.


## Contributing 💡

#### If you'd like to contribute to Django Library Management, feel free to fork this repository and submit a pull request. For more information on contributing to the project, please check out my repository.


#### Step 1

- **Option 1**
    - 🍴 Fork this repo!

- **Option 2**
    - 👯 Clone this repo to your local machine.


#### Step 2

- **Build your code** 🔨🔨🔨

#### Step 3

- 🔃 Create a new pull request.
## Creadits :

#### Django Library Management was created by Mohammad Burhan


## License
[![License](http://img.shields.io/:license-mit-blue.svg?style=flat-square)](http://badges.mit-license.org)

- **[MIT license](http://opensource.org/licenses/mit-license.php)**

## 🏗️ Arquitetura do Sistema

### Padrão MVC (Model-View-Controller)
O projeto segue o padrão arquitetural MVC do Django:
- **Models:** Definição das entidades e regras de negócio
- **Views:** Lógica de controle e processamento das requisições
- **Templates:** Interface do usuário e apresentação

### Estrutura de Autenticação
- Sistema de grupos Django (ADMIN/STUDENT)
- Decoradores de autenticação (`@login_required`, `@user_passes_test`)
- Controle de acesso baseado em permissões

## 📁 Estrutura do Projeto

```
Django-librarymanagement-main/
├── 📄 manage.py                     # Script principal do Django
├── 📄 db.sqlite3                    # Banco de dados SQLite
├── 📄 requirements.txt              # Dependências do projeto
├── 📄 README.md                     # Documentação
├── 📁 librarymanagement/            # Configurações principais
│   ├── 📄 __init__.py
│   ├── 📄 settings.py              # Configurações do Django
│   ├── 📄 urls.py                  # URLs principais
│   ├── 📄 wsgi.py                  # Configuração WSGI
│   └── 📄 asgi.py                  # Configuração ASGI
├── 📁 library/                      # App principal da biblioteca
│   ├── 📄 __init__.py
│   ├── 📄 admin.py                 # Configuração do admin
│   ├── 📄 apps.py                  # Configuração da aplicação
│   ├── 📄 models.py                # Modelos do banco de dados
│   ├── 📄 views.py                 # Lógica das views
│   ├── 📄 forms.py                 # Formulários Django
│   ├── 📄 tests.py                 # Testes unitários
│   └── 📁 migrations/              # Migrações do banco
├── 📁 templates/                    # Templates HTML
│   └── 📁 library/                 # Templates da app library
│       ├── 📄 index.html           # Página inicial
│       ├── 📄 adminlogin.html      # Login admin
│       ├── 📄 studentlogin.html    # Login estudante
│       ├── 📄 addbook.html         # Adicionar livro
│       ├── 📄 viewbook.html        # Visualizar livros
│       ├── 📄 issuebook.html       # Emprestar livro
│       └── 📄 ...                  # Outros templates
└── 📁 static/                       # Arquivos estáticos
    ├── 📁 images/                  # Imagens do sistema
    └── 📁 screenshots/             # Capturas de tela
```

## 🗄️ Modelos do Banco de Dados

### 1. **StudentExtra**
Extensão do modelo User para estudantes:
```python
class StudentExtra(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    enrollment = models.CharField(max_length=40)  # Matrícula
    branch = models.CharField(max_length=40)      # Curso/Departamento
```

### 2. **Book**
Modelo para representar livros:
```python
class Book(models.Model):
    name = models.CharField(max_length=30)        # Nome do livro
    isbn = models.PositiveIntegerField()          # ISBN
    author = models.CharField(max_length=40)      # Autor
    category = models.CharField(max_length=30)    # Categoria
    # Categorias: education, entertainment, comics, biography, history
```

### 3. **IssuedBook**
Modelo para controle de empréstimos:
```python
class IssuedBook(models.Model):
    enrollment = models.CharField(max_length=30)  # Matrícula do estudante
    isbn = models.CharField(max_length=30)        # ISBN do livro
    issuedate = models.DateField(auto_now=True)   # Data de empréstimo
    expirydate = models.DateField()               # Data de devolução (padrão: +15 dias)
```

### Relacionamentos
- **User ↔ StudentExtra:** Relacionamento um-para-um
- **Book ↔ IssuedBook:** Relacionamento através do campo ISBN
- **StudentExtra ↔ IssuedBook:** Relacionamento através do campo enrollment

## 📋 Formulários Django

### 1. **AdminSigupForm**
Cadastro de administradores (baseado no modelo User)

### 2. **StudentUserForm & StudentExtraForm**
Cadastro de estudantes (dados do usuário + informações extras)

### 3. **BookForm**
Formulário para adicionar/editar livros

### 4. **IssuedBookForm**
Formulário para empréstimo de livros com ModelChoiceField

### 5. **ContactusForm**
Formulário de contato com validação de e-mail

## 🔗 Sistema de URLs

### URLs Principais (`librarymanagement/urls.py`)
```python
urlpatterns = [
    path('admin/', admin.site.urls),                    # Django Admin
    path('accounts/', include('django.contrib.auth.urls')),
    
    # Páginas principais
    path('', views.home_view),                          # Página inicial
    path('aboutus', views.aboutus_view),                # Sobre nós
    path('contactus', views.contactus_view),            # Contato
    
    # Seleção de tipo de usuário
    path('adminclick', views.adminclick_view),          # Acesso admin
    path('studentclick', views.studentclick_view),      # Acesso estudante
    
    # Autenticação
    path('adminsignup', views.adminsignup_view),        # Cadastro admin
    path('studentsignup', views.studentsignup_view),    # Cadastro estudante
    path('adminlogin', LoginView.as_view()),            # Login admin
    path('studentlogin', LoginView.as_view()),          # Login estudante
    path('logout', LogoutView.as_view()),               # Logout
    path('afterlogin', views.afterlogin_view),          # Redirecionamento pós-login
    
    # Gestão de livros (apenas admin)
    path('addbook', views.addbook_view),                # Adicionar livro
    path('viewbook', views.viewbook_view),              # Ver livros
    path('issuebook', views.issuebook_view),            # Emprestar livro
    path('viewissuedbook', views.viewissuedbook_view),  # Ver empréstimos
    
    # Gestão de usuários
    path('viewstudent', views.viewstudent_view),        # Ver estudantes
    path('viewissuedbookbystudent', views.viewissuedbookbystudent), # Livros por estudante
]
```

## 🛠️ Comandos Úteis do Django

### Desenvolvimento
```bash
# Criar migrações após mudanças nos models
python manage.py makemigrations

# Aplicar migrações no banco de dados
python manage.py migrate

# Criar superusuário
python manage.py createsuperuser

# Iniciar servidor de desenvolvimento
python manage.py runserver

# Acessar shell interativo do Django
python manage.py shell

# Coletar arquivos estáticos (produção)
python manage.py collectstatic
```

### Banco de Dados
```bash
# Verificar migrações pendentes
python manage.py showmigrations

# Reverter migrações específicas
python manage.py migrate library 0001

# Dump do banco de dados
python manage.py dumpdata > backup.json

# Carregar dados do backup
python manage.py loaddata backup.json
```

## 🔧 Configuração de Produção

### Configurações Importantes (`settings.py`)
```python
# Para produção, altere estas configurações:
DEBUG = False
ALLOWED_HOSTS = ['seu-dominio.com']

# Configure banco de dados de produção
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        # ... outras configurações
    }
}

# Configure arquivos estáticos
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

## 📊 Funcionalidades em Detalhes

### Sistema de Multas
- **Cálculo automático:** Após 15 dias do empréstimo
- **Taxa:** R$ 10,00 por dia de atraso
- **Pagamento:** Online (RazorPay) ou presencial

### Categorias de Livros
- 📚 **Education** (Educação)
- 🎭 **Entertainment** (Entretenimento)
- 📖 **Comics** (Quadrinhos)
- 👤 **Biography** (Biografia)
- 🏛️ **History** (História)

### Tipos de Usuário
- **👨‍💼 ADMIN:** Acesso completo ao sistema
- **👨‍🎓 STUDENT:** Acesso limitado às próprias informações

## 🤝 Como Contribuir

1. **Fork** o projeto
2. Crie uma **branch** para sua feature (`git checkout -b feature/AmazingFeature`)
3. **Commit** suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. **Push** para a branch (`git push origin feature/AmazingFeature`)
5. Abra um **Pull Request**

### Diretrizes para Contribuição
- Siga as convenções de código Python (PEP 8)
- Adicione testes para novas funcionalidades
- Atualize a documentação quando necessário
- Use mensagens de commit descritivas

## 🐛 Relatório de Bugs

Se encontrar um bug, por favor:
1. Verifique se o bug já foi reportado
2. Crie uma issue detalhada com:
   - Descrição do problema
   - Passos para reproduzir
   - Comportamento esperado vs atual
   - Screenshots (se aplicável)

## 📝 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 📞 Contato

- **Desenvolvedor:** [BurhanMohammad](https://github.com/BurhanMohammad)
- **Repositório:** [Django-librarymanagement](https://github.com/BurhanMohammad/Django-librarymanagement)

## 🙏 Agradecimentos

- Comunidade Django pela documentação excelente
- Contribuidores do projeto
- Bibliotecas de código aberto utilizadas

---

**⭐ Se este projeto te ajudou, considere dar uma estrela no repositório!**

## 📚 Documentação Completa

Este projeto inclui documentação abrangente para diferentes aspectos:

| 📄 Documento | 📝 Descrição |
|--------------|--------------|
| [README.md](README.md) | Visão geral e introdução ao projeto |
| [📋 Instalação](INSTALACAO.md) | Guia detalhado de instalação e configuração |
| [🔧 API Referência](API_REFERENCIA.md) | Documentação técnica de modelos, views e forms |
| [🤝 Contribuição](CONTRIBUICAO.md) | Como contribuir para o projeto |
| [🚀 Deploy](DEPLOY.md) | Guia de deploy para produção |
| [🧪 Testes](TESTES.md) | Guia completo de testes e qualidade |
| [📝 Changelog](CHANGELOG.md) | Histórico de versões e mudanças |
| [📄 Licença](LICENSE) | Termos de uso (MIT License) |

## 🎯 Início Rápido

1. **Instalar:** Siga o [Guia de Instalação](INSTALACAO.md)
2. **Explorar:** Consulte a [API Referência](API_REFERENCIA.md)
3. **Contribuir:** Leia o [Guia de Contribuição](CONTRIBUICAO.md)
4. **Deploy:** Use o [Guia de Deploy](DEPLOY.md)
