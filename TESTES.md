# 🧪 Guia de Testes - Sistema de Gerenciamento de Biblioteca

## Configuração de Testes

### Ambiente de Teste
```python
# settings_test.py
from .settings import *

# Usar banco em memória para testes
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': ':memory:',
    }
}

# Desabilitar migrações desnecessárias
class DisableMigrations:
    def __contains__(self, item):
        return True
    def __getitem__(self, item):
        return None

MIGRATION_MODULES = DisableMigrations()

# Cache em memória
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
    }
}

# Desabilitar logs durante testes
LOGGING_CONFIG = None
```

## Testes de Models

### Arquivo: `library/tests/test_models.py`
```python
from django.test import TestCase
from django.contrib.auth.models import User
from django.core.exceptions import ValidationError
from datetime import date, timedelta
from library.models import StudentExtra, Book, IssuedBook

class StudentExtraModelTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(
            username='testuser',
            first_name='Test',
            last_name='User',
            password='testpass123'
        )
    
    def test_student_creation(self):
        """Teste criação de estudante"""
        student = StudentExtra.objects.create(
            user=self.user,
            enrollment='2023001',
            branch='Computer Science'
        )
        
        self.assertEqual(student.enrollment, '2023001')
        self.assertEqual(student.branch, 'Computer Science')
        self.assertEqual(str(student), 'Test[2023001]')
    
    def test_get_name_property(self):
        """Teste propriedade get_name"""
        student = StudentExtra.objects.create(
            user=self.user,
            enrollment='2023001',
            branch='Computer Science'
        )
        
        self.assertEqual(student.get_name, 'Test')
    
    def test_get_userid_property(self):
        """Teste propriedade getuserid"""
        student = StudentExtra.objects.create(
            user=self.user,
            enrollment='2023001',
            branch='Computer Science'
        )
        
        self.assertEqual(student.getuserid, self.user.id)

class BookModelTest(TestCase):
    def test_book_creation(self):
        """Teste criação de livro"""
        book = Book.objects.create(
            name='Django Guide',
            isbn=1234567890,
            author='John Doe',
            category='education'
        )
        
        self.assertEqual(book.name, 'Django Guide')
        self.assertEqual(book.isbn, 1234567890)
        self.assertEqual(book.author, 'John Doe')
        self.assertEqual(book.category, 'education')
        self.assertEqual(str(book), 'Django Guide[1234567890]')
    
    def test_book_categories(self):
        """Teste categorias válidas"""
        valid_categories = ['education', 'entertainment', 'comics', 'biography', 'history']
        
        for category in valid_categories:
            book = Book.objects.create(
                name=f'Test Book {category}',
                isbn=1234567890 + ord(category[0]),
                author='Test Author',
                category=category
            )
            self.assertEqual(book.category, category)

class IssuedBookModelTest(TestCase):
    def setUp(self):
        self.user = User.objects.create_user(
            username='student1',
            password='pass123'
        )
        self.student = StudentExtra.objects.create(
            user=self.user,
            enrollment='2023001',
            branch='CS'
        )
        self.book = Book.objects.create(
            name='Test Book',
            isbn=1234567890,
            author='Test Author',
            category='education'
        )
    
    def test_issued_book_creation(self):
        """Teste criação de empréstimo"""
        issued_book = IssuedBook.objects.create(
            enrollment='2023001',
            isbn='1234567890'
        )
        
        self.assertEqual(issued_book.enrollment, '2023001')
        self.assertEqual(issued_book.isbn, '1234567890')
        self.assertEqual(issued_book.issuedate, date.today())
        
        # Verificar se data de expiração é 15 dias após hoje
        expected_expiry = date.today() + timedelta(days=15)
        self.assertEqual(issued_book.expirydate, expected_expiry)
```

## Testes de Views

### Arquivo: `library/tests/test_views.py`
```python
from django.test import TestCase, Client
from django.urls import reverse
from django.contrib.auth.models import User, Group
from library.models import StudentExtra, Book, IssuedBook

class PublicViewsTest(TestCase):
    def setUp(self):
        self.client = Client()
    
    def test_home_view(self):
        """Teste página inicial"""
        response = self.client.get(reverse('home'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Biblioteca')
    
    def test_aboutus_view(self):
        """Teste página sobre nós"""
        response = self.client.get('/aboutus')
        self.assertEqual(response.status_code, 200)
    
    def test_contactus_view(self):
        """Teste página de contato"""
        response = self.client.get('/contactus')
        self.assertEqual(response.status_code, 200)

class AuthenticationViewsTest(TestCase):
    def setUp(self):
        self.client = Client()
    
    def test_admin_signup_view_get(self):
        """Teste GET para cadastro de admin"""
        response = self.client.get('/adminsignup')
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Cadastro de Administrador')
    
    def test_admin_signup_view_post(self):
        """Teste POST para cadastro de admin"""
        data = {
            'first_name': 'Admin',
            'last_name': 'User',
            'username': 'admin1',
            'password': 'adminpass123'
        }
        response = self.client.post('/adminsignup', data)
        self.assertEqual(response.status_code, 302)  # Redirect
        
        # Verificar se usuário foi criado
        self.assertTrue(User.objects.filter(username='admin1').exists())
        
        # Verificar se foi adicionado ao grupo ADMIN
        user = User.objects.get(username='admin1')
        self.assertTrue(user.groups.filter(name='ADMIN').exists())
    
    def test_student_signup_view_post(self):
        """Teste POST para cadastro de estudante"""
        data = {
            'first_name': 'Student',
            'last_name': 'User',
            'username': 'student1',
            'password': 'studentpass123',
            'enrollment': '2023001',
            'branch': 'Computer Science'
        }
        response = self.client.post('/studentsignup', data)
        
        # Verificar se usuário e estudante foram criados
        self.assertTrue(User.objects.filter(username='student1').exists())
        self.assertTrue(StudentExtra.objects.filter(enrollment='2023001').exists())

class AdminViewsTest(TestCase):
    def setUp(self):
        self.client = Client()
        
        # Criar admin
        self.admin_user = User.objects.create_user(
            username='admin',
            password='adminpass'
        )
        admin_group, _ = Group.objects.get_or_create(name='ADMIN')
        admin_group.user_set.add(self.admin_user)
        
        # Criar livro para testes
        self.book = Book.objects.create(
            name='Test Book',
            isbn=1234567890,
            author='Test Author',
            category='education'
        )
    
    def test_admin_login_required(self):
        """Teste se views de admin requerem login"""
        response = self.client.get('/addbook')
        self.assertEqual(response.status_code, 302)  # Redirect para login
    
    def test_addbook_view_authenticated(self):
        """Teste adicionar livro como admin logado"""
        self.client.login(username='admin', password='adminpass')
        response = self.client.get('/addbook')
        self.assertEqual(response.status_code, 200)
    
    def test_addbook_post(self):
        """Teste POST para adicionar livro"""
        self.client.login(username='admin', password='adminpass')
        
        data = {
            'name': 'New Book',
            'isbn': 9876543210,
            'author': 'New Author',
            'category': 'entertainment'
        }
        response = self.client.post('/addbook', data)
        
        # Verificar se livro foi criado
        self.assertTrue(Book.objects.filter(name='New Book').exists())
    
    def test_viewbook_view(self):
        """Teste visualizar livros"""
        self.client.login(username='admin', password='adminpass')
        response = self.client.get('/viewbook')
        
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Test Book')

class StudentViewsTest(TestCase):
    def setUp(self):
        self.client = Client()
        
        # Criar estudante
        self.student_user = User.objects.create_user(
            username='student',
            password='studentpass'
        )
        student_group, _ = Group.objects.get_or_create(name='STUDENT')
        student_group.user_set.add(self.student_user)
        
        self.student = StudentExtra.objects.create(
            user=self.student_user,
            enrollment='2023001',
            branch='CS'
        )
        
        # Criar livro emprestado
        self.book = Book.objects.create(
            name='Borrowed Book',
            isbn=1234567890,
            author='Author',
            category='education'
        )
        
        self.issued_book = IssuedBook.objects.create(
            enrollment='2023001',
            isbn='1234567890'
        )
    
    def test_student_view_own_books(self):
        """Teste estudante ver próprios livros"""
        self.client.login(username='student', password='studentpass')
        response = self.client.get('/viewissuedbookbystudent')
        
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Borrowed Book')
```

## Testes de Forms

### Arquivo: `library/tests/test_forms.py`
```python
from django.test import TestCase
from library.forms import ContactusForm, AdminSigupForm, StudentUserForm, BookForm

class ContactusFormTest(TestCase):
    def test_valid_form(self):
        """Teste formulário válido de contato"""
        form_data = {
            'Name': 'Test User',
            'Email': 'test@example.com',
            'Message': 'This is a test message.'
        }
        form = ContactusForm(data=form_data)
        self.assertTrue(form.is_valid())
    
    def test_invalid_email(self):
        """Teste email inválido"""
        form_data = {
            'Name': 'Test User',
            'Email': 'invalid-email',
            'Message': 'This is a test message.'
        }
        form = ContactusForm(data=form_data)
        self.assertFalse(form.is_valid())
        self.assertIn('Email', form.errors)
    
    def test_required_fields(self):
        """Teste campos obrigatórios"""
        form = ContactusForm(data={})
        self.assertFalse(form.is_valid())
        self.assertIn('Name', form.errors)
        self.assertIn('Email', form.errors)
        self.assertIn('Message', form.errors)

class BookFormTest(TestCase):
    def test_valid_book_form(self):
        """Teste formulário válido de livro"""
        form_data = {
            'name': 'Test Book',
            'isbn': 1234567890,
            'author': 'Test Author',
            'category': 'education'
        }
        form = BookForm(data=form_data)
        self.assertTrue(form.is_valid())
    
    def test_invalid_category(self):
        """Teste categoria inválida"""
        form_data = {
            'name': 'Test Book',
            'isbn': 1234567890,
            'author': 'Test Author',
            'category': 'invalid_category'
        }
        form = BookForm(data=form_data)
        self.assertFalse(form.is_valid())
```

## Testes de Integração

### Arquivo: `library/tests/test_integration.py`
```python
from django.test import TestCase, TransactionTestCase
from django.contrib.auth.models import User, Group
from library.models import StudentExtra, Book, IssuedBook
from datetime import date, timedelta

class LibraryWorkflowTest(TransactionTestCase):
    def test_complete_library_workflow(self):
        """Teste workflow completo da biblioteca"""
        
        # 1. Criar admin
        admin = User.objects.create_user(
            username='admin',
            password='adminpass'
        )
        admin_group, _ = Group.objects.get_or_create(name='ADMIN')
        admin_group.user_set.add(admin)
        
        # 2. Admin adiciona livro
        book = Book.objects.create(
            name='Django for Beginners',
            isbn=1234567890,
            author='William Vincent',
            category='education'
        )
        
        # 3. Criar estudante
        student_user = User.objects.create_user(
            username='student',
            password='studentpass'
        )
        student_group, _ = Group.objects.get_or_create(name='STUDENT')
        student_group.user_set.add(student_user)
        
        student = StudentExtra.objects.create(
            user=student_user,
            enrollment='2023001',
            branch='Computer Science'
        )
        
        # 4. Admin empresta livro para estudante
        issued_book = IssuedBook.objects.create(
            enrollment=student.enrollment,
            isbn=str(book.isbn)
        )
        
        # 5. Verificar empréstimo
        self.assertEqual(issued_book.enrollment, '2023001')
        self.assertEqual(issued_book.isbn, '1234567890')
        self.assertEqual(issued_book.issuedate, date.today())
        
        # 6. Verificar data de devolução
        expected_expiry = date.today() + timedelta(days=15)
        self.assertEqual(issued_book.expirydate, expected_expiry)
        
        # 7. Verificar se estudante pode ver o livro emprestado
        student_books = IssuedBook.objects.filter(enrollment=student.enrollment)
        self.assertEqual(student_books.count(), 1)
        self.assertEqual(student_books.first().isbn, str(book.isbn))

class FineCalculationTest(TestCase):
    def test_fine_calculation(self):
        """Teste cálculo de multa"""
        # Criar empréstimo vencido
        issued_book = IssuedBook.objects.create(
            enrollment='2023001',
            isbn='1234567890'
        )
        
        # Simular livro vencido há 5 dias
        issued_book.issuedate = date.today() - timedelta(days=20)
        issued_book.save()
        
        # Calcular multa (5 dias * R$ 10 = R$ 50)
        days_overdue = (date.today() - issued_book.issuedate).days - 15
        fine = max(0, days_overdue * 10)
        
        self.assertEqual(fine, 50)
```

## Executar Testes

### Comandos Básicos
```powershell
# Executar todos os testes
python manage.py test

# Executar testes específicos
python manage.py test library.tests.test_models

# Executar teste específico
python manage.py test library.tests.test_models.BookModelTest.test_book_creation

# Executar com verbosidade
python manage.py test --verbosity=2

# Executar com coverage
pip install coverage
coverage run --source='.' manage.py test
coverage report
coverage html  # Gera relatório HTML
```

### Configuração de Coverage
```powershell
# Instalar coverage
pip install coverage

# Executar testes com coverage
coverage run --source='.' manage.py test

# Ver relatório no terminal
coverage report

# Gerar relatório HTML
coverage html

# Ver relatório no navegador
start htmlcov\index.html  # Windows
```

## Testes de Performance

### Arquivo: `library/tests/test_performance.py`
```python
from django.test import TestCase
from django.test.utils import override_settings
from django.db import connection
from library.models import Book
import time

class PerformanceTest(TestCase):
    def setUp(self):
        # Criar muitos livros para teste
        books = []
        for i in range(1000):
            books.append(Book(
                name=f'Book {i}',
                isbn=1000000000 + i,
                author=f'Author {i}',
                category='education'
            ))
        Book.objects.bulk_create(books)
    
    def test_book_query_performance(self):
        """Teste performance de consulta de livros"""
        start_time = time.time()
        
        # Reset queries count
        connection.queries_log.clear()
        
        # Executar consulta
        books = list(Book.objects.all())
        
        end_time = time.time()
        query_time = end_time - start_time
        
        # Verificar se consulta não demora mais que 1 segundo
        self.assertLess(query_time, 1.0)
        
        # Verificar se usou apenas 1 query
        self.assertEqual(len(connection.queries), 1)
        
        # Verificar se retornou todos os livros
        self.assertEqual(len(books), 1000)
```

## Testes de Segurança

### Arquivo: `library/tests/test_security.py`
```python
from django.test import TestCase, Client
from django.contrib.auth.models import User, Group
from library.models import StudentExtra

class SecurityTest(TestCase):
    def setUp(self):
        self.client = Client()
        
        # Criar usuários
        self.admin = User.objects.create_user(
            username='admin',
            password='adminpass'
        )
        admin_group, _ = Group.objects.get_or_create(name='ADMIN')
        admin_group.user_set.add(self.admin)
        
        self.student_user = User.objects.create_user(
            username='student',
            password='studentpass'
        )
        student_group, _ = Group.objects.get_or_create(name='STUDENT')
        student_group.user_set.add(self.student_user)
        
        self.student = StudentExtra.objects.create(
            user=self.student_user,
            enrollment='2023001',
            branch='CS'
        )
    
    def test_student_cannot_access_admin_views(self):
        """Teste que estudante não pode acessar views de admin"""
        self.client.login(username='student', password='studentpass')
        
        admin_urls = ['/addbook', '/viewbook', '/issuebook', '/viewstudent']
        
        for url in admin_urls:
            response = self.client.get(url)
            # Deve ser redirecionado ou negado acesso
            self.assertIn(response.status_code, [302, 403])
    
    def test_unauthenticated_access(self):
        """Teste acesso sem autenticação"""
        protected_urls = [
            '/addbook', '/viewbook', '/issuebook', 
            '/viewstudent', '/viewissuedbookbystudent'
        ]
        
        for url in protected_urls:
            response = self.client.get(url)
            self.assertEqual(response.status_code, 302)  # Redirect para login
    
    def test_csrf_protection(self):
        """Teste proteção CSRF"""
        self.client.login(username='admin', password='adminpass')
        
        # Tentar POST sem CSRF token
        response = self.client.post('/addbook', {
            'name': 'Test Book',
            'isbn': 1234567890,
            'author': 'Test Author',
            'category': 'education'
        })
        
        # Deve falhar por falta de CSRF token
        self.assertEqual(response.status_code, 403)
```

## Configuração de CI/CD

### GitHub Actions (`.github/workflows/django.yml`)
```yaml
name: Django CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        python-version: [3.8, 3.9, 3.10]
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        pip install coverage
    
    - name: Run tests
      run: |
        coverage run --source='.' manage.py test
        coverage xml
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v1
      with:
        file: ./coverage.xml
```

## Relatórios de Teste

### Configuração do Coverage
```ini
# .coveragerc
[run]
source = .
omit = 
    */venv/*
    */migrations/*
    */tests/*
    manage.py
    */settings/*
    */wsgi.py
    */asgi.py

[report]
exclude_lines =
    pragma: no cover
    def __repr__
    raise AssertionError
    raise NotImplementedError
```

Este guia de testes fornece uma base sólida para garantir a qualidade e confiabilidade do sistema de gerenciamento de biblioteca.
