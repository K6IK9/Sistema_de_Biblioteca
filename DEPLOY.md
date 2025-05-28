# 🚀 Guia de Deploy - Sistema de Gerenciamento de Biblioteca

## Preparação para Produção

### 1. Configurações de Segurança

#### Arquivo `settings.py` para Produção
Crie um arquivo `settings_production.py`:

```python
from .settings import *
import os

# SEGURANÇA
DEBUG = False
ALLOWED_HOSTS = ['seu-dominio.com', 'www.seu-dominio.com']

# Chave secreta (use variável de ambiente)
SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY')

# Banco de Dados de Produção
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('DB_NAME'),
        'USER': os.environ.get('DB_USER'),
        'PASSWORD': os.environ.get('DB_PASSWORD'),
        'HOST': os.environ.get('DB_HOST', 'localhost'),
        'PORT': os.environ.get('DB_PORT', '5432'),
    }
}

# Arquivos Estáticos
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

# HTTPS
SECURE_SSL_REDIRECT = True
SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
```

### 2. Variáveis de Ambiente
Crie um arquivo `.env`:

```env
DJANGO_SECRET_KEY=sua-chave-secreta-super-segura
DB_NAME=biblioteca_db
DB_USER=biblioteca_user
DB_PASSWORD=senha-super-segura
DB_HOST=localhost
DB_PORT=5432
EMAIL_HOST_USER=seu-email@gmail.com
EMAIL_HOST_PASSWORD=sua-senha-app
```

## Deploy em Diferentes Plataformas

### 🐳 Docker

#### Dockerfile
```dockerfile
FROM python:3.9

# Configurar diretório de trabalho
WORKDIR /app

# Copiar requirements e instalar dependências
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar código da aplicação
COPY . .

# Coletar arquivos estáticos
RUN python manage.py collectstatic --noinput

# Expor porta
EXPOSE 8000

# Comando para iniciar aplicação
CMD ["gunicorn", "librarymanagement.wsgi:application", "--bind", "0.0.0.0:8000"]
```

#### docker-compose.yml
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DEBUG=False
    depends_on:
      - db
    volumes:
      - static_volume:/app/staticfiles

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: biblioteca_db
      POSTGRES_USER: biblioteca_user
      POSTGRES_PASSWORD: senha-super-segura
    volumes:
      - postgres_data:/var/lib/postgresql/data

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - static_volume:/app/staticfiles
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - web

volumes:
  postgres_data:
  static_volume:
```

### ☁️ Heroku

#### Procfile
```
web: gunicorn librarymanagement.wsgi:application --log-file -
```

#### requirements.txt (adicionar)
```
gunicorn==20.1.0
dj-database-url==0.5.0
whitenoise==5.3.0
psycopg2-binary==2.9.1
```

#### Comandos de Deploy
```bash
# Instalar Heroku CLI
# Fazer login
heroku login

# Criar aplicação
heroku create nome-da-sua-app

# Configurar variáveis de ambiente
heroku config:set DJANGO_SECRET_KEY=sua-chave-secreta
heroku config:set DEBUG=False

# Adicionar PostgreSQL
heroku addons:create heroku-postgresql:hobby-dev

# Deploy
git push heroku main

# Executar migrações
heroku run python manage.py migrate

# Criar superusuário
heroku run python manage.py createsuperuser
```

### 🌐 DigitalOcean/VPS

#### 1. Configuração do Servidor
```bash
# Atualizar sistema
sudo apt update && sudo apt upgrade -y

# Instalar dependências
sudo apt install python3 python3-pip python3-venv nginx postgresql postgresql-contrib

# Configurar PostgreSQL
sudo -u postgres createuser --interactive
sudo -u postgres createdb biblioteca_db
```

#### 2. Deploy da Aplicação
```bash
# Clonar repositório
git clone https://github.com/seu-usuario/Django-librarymanagement.git
cd Django-librarymanagement

# Configurar ambiente virtual
python3 -m venv venv
source venv/bin/activate

# Instalar dependências
pip install -r requirements.txt
pip install gunicorn

# Configurar banco de dados
python manage.py migrate
python manage.py collectstatic

# Criar superusuário
python manage.py createsuperuser
```

#### 3. Configuração do Nginx
```nginx
server {
    listen 80;
    server_name seu-dominio.com;

    location /static/ {
        alias /path/to/your/app/staticfiles/;
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

#### 4. Systemd Service
```ini
[Unit]
Description=Gunicorn instance to serve biblioteca
After=network.target

[Service]
User=www-data
Group=www-data
WorkingDirectory=/path/to/your/app
Environment="PATH=/path/to/your/app/venv/bin"
ExecStart=/path/to/your/app/venv/bin/gunicorn --workers 3 --bind unix:/path/to/your/app/biblioteca.sock librarymanagement.wsgi:application
ExecReload=/bin/kill -s HUP $MAINPID
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## Monitoramento e Logs

### 1. Configuração de Logs
```python
# settings.py
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'INFO',
            'class': 'logging.FileHandler',
            'filename': 'biblioteca.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'INFO',
            'propagate': True,
        },
    },
}
```

### 2. Monitoramento com Sentry
```bash
pip install sentry-sdk[django]
```

```python
# settings.py
import sentry_sdk
from sentry_sdk.integrations.django import DjangoIntegration

sentry_sdk.init(
    dsn="sua-dsn-do-sentry",
    integrations=[DjangoIntegration()],
    traces_sample_rate=1.0,
    send_default_pii=True
)
```

## Backup e Recuperação

### 1. Backup do Banco de Dados
```bash
# PostgreSQL
pg_dump biblioteca_db > backup_$(date +%Y%m%d).sql

# Restaurar
psql biblioteca_db < backup_20231201.sql
```

### 2. Backup Automatizado
```bash
#!/bin/bash
# backup_script.sh

DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backups"

# Backup do banco
pg_dump biblioteca_db > $BACKUP_DIR/db_backup_$DATE.sql

# Backup dos arquivos estáticos
tar -czf $BACKUP_DIR/static_backup_$DATE.tar.gz /path/to/staticfiles

# Remover backups antigos (mais de 30 dias)
find $BACKUP_DIR -name "*.sql" -mtime +30 -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +30 -delete
```

## Otimização de Performance

### 1. Cache com Redis
```bash
pip install django-redis
```

```python
# settings.py
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}

SESSION_ENGINE = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"
```

### 2. Otimização de Queries
```python
# views.py - Usar select_related e prefetch_related
books = Book.objects.select_related('author').all()
issued_books = IssuedBook.objects.prefetch_related('student', 'book').all()
```

## SSL/HTTPS

### 1. Let's Encrypt (Certbot)
```bash
# Instalar certbot
sudo apt install certbot python3-certbot-nginx

# Obter certificado
sudo certbot --nginx -d seu-dominio.com

# Renovação automática
sudo crontab -e
# Adicionar: 0 12 * * * /usr/bin/certbot renew --quiet
```

## Checklist de Deploy

### Antes do Deploy
- [ ] Testar aplicação localmente
- [ ] Configurar variáveis de ambiente
- [ ] Atualizar requirements.txt
- [ ] Configurar settings de produção
- [ ] Testar migrações

### Durante o Deploy
- [ ] Fazer backup do banco atual
- [ ] Executar migrações
- [ ] Coletar arquivos estáticos
- [ ] Testar funcionalidades críticas
- [ ] Verificar logs

### Após o Deploy
- [ ] Monitorar performance
- [ ] Verificar logs de erro
- [ ] Testar todas as funcionalidades
- [ ] Configurar monitoramento
- [ ] Documentar mudanças

## Solução de Problemas

### Erro 500 (Server Error)
```bash
# Verificar logs
tail -f /var/log/nginx/error.log
tail -f biblioteca.log

# Verificar configuração
python manage.py check --deploy
```

### Problemas de Static Files
```bash
# Coletar novamente
python manage.py collectstatic --clear

# Verificar permissões
sudo chown -R www-data:www-data /path/to/staticfiles
```

### Performance Lenta
```bash
# Verificar queries
python manage.py shell
>>> from django.db import connection
>>> connection.queries

# Usar debug toolbar
pip install django-debug-toolbar
```
