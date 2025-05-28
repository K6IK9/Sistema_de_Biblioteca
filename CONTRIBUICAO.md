# 🔧 Guia de Contribuição - Sistema de Gerenciamento de Biblioteca

## Como Contribuir

Agradecemos seu interesse em contribuir para o projeto! Este guia irá ajudá-lo a começar.

## 📋 Pré-requisitos para Desenvolvedores

- Python 3.7+
- Django 3.0.5
- Git
- Editor de código (recomendado: VS Code)
- Conhecimento básico de Django e Python

## 🚀 Configuração do Ambiente de Desenvolvimento

### 1. Fork e Clone
```bash
# Fork o repositório no GitHub e clone sua versão
git clone https://github.com/SEU_USUARIO/Django-librarymanagement.git
cd Django-librarymanagement

# Adicione o repositório original como upstream
git remote add upstream https://github.com/BurhanMohammad/Django-librarymanagement.git
```

### 2. Configuração Local
```bash
# Crie e ative ambiente virtual
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate  # Windows

# Instale dependências
pip install -r requirements.txt

# Configure banco de dados
python manage.py makemigrations
python manage.py migrate

# Crie superusuário para testes
python manage.py createsuperuser
```

## 📝 Padrões de Código

### Python/Django
- Siga a **PEP 8** para formatação de código
- Use **snake_case** para variáveis e funções
- Use **PascalCase** para classes
- Documente funções complexas
- Mantenha linhas com máximo de 79 caracteres

### HTML/CSS
- Use indentação de 2 espaços
- Nomes de classes em **kebab-case**
- Comentários em português
- Estruture CSS de forma modular

### JavaScript
- Use **camelCase** para variáveis
- Adicione comentários para lógica complexa
- Evite jQuery desnecessário

## 🔄 Fluxo de Trabalho

### 1. Criar Branch para Feature
```bash
# Sempre trabalhe em uma branch separada
git checkout -b feature/nome-da-funcionalidade

# Ou para correção de bug
git checkout -b bugfix/descricao-do-bug
```

### 2. Fazer Alterações
- Faça commits pequenos e frequentes
- Use mensagens de commit descritivas
- Teste suas alterações localmente

### 3. Commit e Push
```bash
# Adicione arquivos modificados
git add .

# Commit com mensagem descritiva
git commit -m "feat: adicionar funcionalidade de pesquisa avançada"

# Push para sua branch
git push origin feature/nome-da-funcionalidade
```

### 4. Pull Request
- Crie um Pull Request no GitHub
- Descreva claramente as mudanças
- Adicione screenshots se aplicável
- Aguarde revisão do código

## 📊 Tipos de Contribuição

### 🐛 Correção de Bugs
1. Identifique o problema
2. Reproduza o bug localmente
3. Implemente a correção
4. Adicione testes se necessário
5. Teste a correção

### ✨ Novas Funcionalidades
1. Discuta a feature em uma issue primeiro
2. Implemente seguindo os padrões
3. Adicione testes
4. Atualize documentação
5. Teste completamente

### 📚 Documentação
- Melhore README.md
- Adicione comentários no código
- Crie tutoriais
- Traduza documentação

### 🎨 Melhorias de UI/UX
- Melhore design responsivo
- Adicione acessibilidade
- Otimize performance
- Melhore experiência do usuário

## 🧪 Testes

### Executar Testes
```bash
# Executar todos os testes
python manage.py test

# Executar testes específicos
python manage.py test library.tests.TestBookModel

# Verificar cobertura de código
coverage run --source='.' manage.py test
coverage report
```

### Escrever Testes
- Teste models, views e forms
- Use nomes descritivos
- Teste casos normais e extremos
- Mantenha testes independentes

## 📋 Mensagens de Commit

Use o padrão Conventional Commits:

```
tipo(escopo): descrição

[corpo opcional]

[rodapé opcional]
```

### Tipos de Commit
- `feat`: Nova funcionalidade
- `fix`: Correção de bug
- `docs`: Documentação
- `style`: Formatação de código
- `refactor`: Refatoração
- `test`: Adição de testes
- `chore`: Tarefas de manutenção

### Exemplos
```bash
feat(auth): adicionar autenticação por email
fix(books): corrigir cálculo de multa
docs(readme): atualizar instruções de instalação
style(forms): formatar código seguindo PEP8
```

## 🔍 Revisão de Código

### Para Revisores
- Verifique funcionalidade
- Teste código localmente
- Verifique padrões de código
- Dê feedback construtivo
- Aprove quando estiver pronto

### Para Autores
- Responda a comentários
- Faça alterações solicitadas
- Teste após mudanças
- Mantenha comunicação clara

## 📚 Recursos Úteis

### Documentação
- [Django Documentation](https://docs.djangoproject.com/)
- [Python PEP 8](https://www.python.org/dev/peps/pep-0008/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)

### Ferramentas
- **Black**: Formatação automática de código Python
- **Flake8**: Verificação de estilo e erros
- **Pylint**: Análise estática de código

## 🎯 Roadmap do Projeto

### Próximas Funcionalidades
- [ ] Sistema de notificações por email
- [ ] API REST para integração
- [ ] Dashboard com gráficos
- [ ] Sistema de reservas
- [ ] Histórico detalhado de ações
- [ ] Integração com código de barras
- [ ] Sistema de avaliação de livros
- [ ] Modo escuro

### Melhorias Técnicas
- [ ] Testes automatizados (CI/CD)
- [ ] Docker para desenvolvimento
- [ ] Cache com Redis
- [ ] Otimização de queries
- [ ] Logs estruturados
- [ ] Monitoramento de performance

## 🏆 Reconhecimento

### Hall da Fama
Contribuidores serão reconhecidos:
- No README do projeto
- Em releases especiais
- Na documentação
- Com badges especiais

### Como ser Reconhecido
- Contribuições regulares
- Código de qualidade
- Ajuda à comunidade
- Documentação clara

## 📞 Contato

- **Issues:** Para bugs e sugestões
- **Discussions:** Para perguntas gerais
- **Email:** Para questões sensíveis

## 🤝 Código de Conduta

- Seja respeitoso e inclusivo
- Aceite feedback construtivo
- Ajude outros desenvolvedores
- Mantenha discussões profissionais
- Respeite diferentes pontos de vista

---

**Obrigado por contribuir! Juntos podemos tornar este projeto ainda melhor! 🚀**
