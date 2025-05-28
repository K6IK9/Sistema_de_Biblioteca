# 📋 Guia de Instalação - Sistema de Gerenciamento de Biblioteca

## Pré-requisitos

Antes de instalar o sistema, certifique-se de ter:

- **Python 3.7+** instalado
- **pip** (gerenciador de pacotes Python)
- **Git** para clonar o repositório
- **Navegador web** moderno

## Instalação Passo a Passo

### 1. Clone o Repositório
```bash
git clone https://github.com/BurhanMohammad/Django-librarymanagement.git
cd Django-librarymanagement
```

### 2. Configuração do Ambiente Virtual

#### Windows
```bash
# Criar ambiente virtual
python -m venv venv

# Ativar ambiente virtual
venv\Scripts\activate
```

#### Linux/macOS
```bash
# Criar ambiente virtual
python3 -m venv venv

# Ativar ambiente virtual
source venv/bin/activate
```

### 3. Instalar Dependências
```bash
pip install -r requirements.txt
```

### 4. Configuração do Banco de Dados
```bash
# Criar migrações
python manage.py makemigrations

# Aplicar migrações
python manage.py migrate
```

### 5. Criar Usuário Administrador (Opcional)
```bash
python manage.py createsuperuser
```
Siga as instruções para criar um superusuário.

### 6. Iniciar o Servidor
```bash
python manage.py runserver
```

### 7. Acessar o Sistema
Abra seu navegador e acesse: `http://127.0.0.1:8000`

## Configuração Adicional

### Configurar Arquivos Estáticos (Produção)
```bash
python manage.py collectstatic
```

### Verificar Instalação
```bash
python manage.py check
```

## Solução de Problemas

### Erro: "No module named 'django'"
```bash
pip install django==3.0.5
```

### Erro: "Port already in use"
```bash
python manage.py runserver 8001
```

### Erro de Migração
```bash
python manage.py migrate --run-syncdb
```

## Contas de Teste

Após a instalação, você pode criar contas através da interface web:

1. **Administrador:** Use a opção "Cadastre-se como Administrador"
2. **Estudante:** Use a opção "Cadastre-se como Estudante"

## Próximos Passos

1. Faça login como administrador
2. Adicione alguns livros ao sistema
3. Crie contas de estudantes
4. Teste o sistema de empréstimos
