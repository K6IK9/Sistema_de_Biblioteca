# 📝 Changelog - Sistema de Gerenciamento de Biblioteca

Todas as mudanças notáveis neste projeto serão documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/),
e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [Não Lançado]

### 🔄 Planejado
- Sistema de notificações por email
- API REST para integração
- Dashboard com gráficos e métricas
- Sistema de reservas online
- Histórico detalhado de ações
- Integração com código de barras
- Sistema de avaliação de livros
- Modo escuro na interface
- App móvel (React Native)
- Relatórios avançados (PDF/Excel)

### 🔧 Melhorias Técnicas Planejadas
- Implementação de testes automatizados (CI/CD)
- Containerização com Docker
- Cache com Redis
- Logs estruturados
- Monitoramento com Sentry
- Backup automatizado

## [2.1.0] - 2024-12-01

### ✨ Adicionado
- **Documentação Completa:**
  - README.md atualizado com instruções detalhadas
  - Guia de instalação (INSTALACAO.md)
  - Referência da API (API_REFERENCIA.md)
  - Guia de contribuição (CONTRIBUICAO.md)
  - Guia de deploy (DEPLOY.md)
  - Guia de testes (TESTES.md)
  - Changelog completo (CHANGELOG.md)

- **Sistema de Multas Aprimorado:**
  - Cálculo automático de multas após 15 dias
  - Taxa de R$ 10,00 por dia de atraso
  - Interface melhorada para visualização de multas

- **Segurança:**
  - Validação CSRF em todos os formulários
  - Controle de acesso baseado em grupos
  - Sanitização de dados de entrada

### 🔧 Melhorado
- Interface do usuário mais responsiva
- Navegação intuitiva com breadcrumbs
- Mensagens de feedback mais claras
- Performance de consultas ao banco de dados

### 🐛 Corrigido
- Erro ao calcular datas de vencimento
- Problema de autenticação em algumas views
- Layout quebrado em dispositivos móveis
- Validação de formulários inconsistente

## [2.0.0] - 2024-06-15

### ✨ Adicionado
- **Sistema de Categorias:**
  - 5 categorias de livros: Educação, Entretenimento, Quadrinhos, Biografia, História
  - Filtros por categoria na listagem
  - Ícones visuais para cada categoria

- **Dashboard Administrativo:**
  - Painel de controle centralizado
  - Estatísticas em tempo real
  - Gráficos de empréstimos

- **Sistema de Pesquisa:**
  - Busca por nome do livro
  - Busca por autor
  - Busca por categoria
  - Resultados paginados

### 🔧 Melhorado
- Design responsivo com Bootstrap
- Validação de formulários em tempo real
- Performance geral do sistema
- Experiência do usuário

### 🐛 Corrigido
- Duplicação de registros de empréstimo
- Erro ao deletar livros com empréstimos ativos
- Problema de encoding em nomes com acentos

### ⚠️ Breaking Changes
- Modelo Book agora inclui campo category (migração necessária)
- URLs reorganizadas (verificar links externos)

## [1.5.0] - 2024-03-20

### ✨ Adicionado
- **Integração RazorPay:**
  - Pagamento online de multas
  - Gateway de pagamento seguro
  - Histórico de transações

- **Sistema de Grupos:**
  - Separação clara entre ADMIN e STUDENT
  - Permissões baseadas em grupos
  - Controle de acesso refinado

### 🔧 Melhorado
- Interface de login modernizada
- Mensagens de erro mais descritivas
- Validação de dados aprimorada

## [1.4.0] - 2024-01-10

### ✨ Adicionado
- **Gestão de Estudantes:**
  - CRUD completo de estudantes
  - Filtros por departamento
  - Histórico individual de empréstimos

- **Sistema de Devolução:**
  - Marcar livros como devolvidos
  - Cálculo automático de multas
  - Relatórios de devolução

### 🔧 Melhorado
- Performance de consultas complexas
- Interface de administração
- Mensagens de confirmação

## [1.3.0] - 2023-11-05

### ✨ Adicionado
- **Empréstimo de Livros:**
  - Sistema completo de empréstimos
  - Data de devolução automática (+15 dias)
  - Controle de status (emprestado/disponível)

- **Validação Avançada:**
  - Verificação de ID duplicado em tempo real
  - Validação sem recarregamento de página
  - Feedback visual imediato

### 🐛 Corrigido
- Erro ao salvar datas de empréstimo
- Problema de timezone
- Validação de ISBN duplicado

## [1.2.0] - 2023-09-15

### ✨ Adicionado
- **Gestão de Livros:**
  - Adicionar novos livros
  - Editar informações existentes
  - Remover livros do acervo
  - Listagem com paginação

- **Sistema de Autenticação:**
  - Login separado para admin e estudante
  - Cadastro de novos usuários
  - Redirecionamento baseado no tipo de usuário

### 🔧 Melhorado
- Design da interface com CSS customizado
- Responsividade em dispositivos móveis
- Navegação mais intuitiva

## [1.1.0] - 2023-07-20

### ✨ Adicionado
- **Modelos de Dados:**
  - StudentExtra para informações complementares
  - Book para controle do acervo
  - IssuedBook para empréstimos

- **Formulários Django:**
  - AdminSigupForm para cadastro de administradores
  - StudentUserForm e StudentExtraForm para estudantes
  - BookForm para gestão de livros

### 🔧 Melhorado
- Estrutura do banco de dados
- Relacionamentos entre modelos
- Validação de dados

## [1.0.0] - 2023-05-01

### ✨ Inicial
- **Configuração Base:**
  - Projeto Django 3.0.5 configurado
  - Estrutura de templates
  - Arquivos estáticos organizados
  - Configurações de desenvolvimento

- **Páginas Básicas:**
  - Página inicial
  - Sobre nós
  - Contato
  - Templates base

- **Autenticação Simples:**
  - Sistema de login básico
  - Diferenciação entre tipos de usuário
  - Proteção de rotas

---

## 🏷️ Tipos de Mudanças

- **✨ Adicionado** para novas funcionalidades
- **🔧 Melhorado** para mudanças em funcionalidades existentes
- **🔄 Modificado** para funcionalidades depreciadas
- **🗑️ Removido** para funcionalidades removidas
- **🐛 Corrigido** para correções de bugs
- **🔒 Segurança** para correções de vulnerabilidades

## 📋 Convenções de Versionamento

Este projeto usa [Versionamento Semântico](https://semver.org/):

- **MAJOR** (X.0.0): Mudanças incompatíveis na API
- **MINOR** (0.X.0): Funcionalidades adicionadas de forma compatível
- **PATCH** (0.0.X): Correções de bugs compatíveis

## 🚀 Como Contribuir

Para contribuir com o projeto:

1. Fork o repositório
2. Crie uma branch para sua feature (`git checkout -b feature/NovaFuncionalidade`)
3. Commit suas mudanças (`git commit -m 'Adiciona nova funcionalidade'`)
4. Push para a branch (`git push origin feature/NovaFuncionalidade`)
5. Abra um Pull Request

## 📞 Suporte

- **Issues:** [GitHub Issues](https://github.com/BurhanMohammad/Django-librarymanagement/issues)
- **Discussões:** [GitHub Discussions](https://github.com/BurhanMohammad/Django-librarymanagement/discussions)
- **Documentação:** [Wiki do Projeto](https://github.com/BurhanMohammad/Django-librarymanagement/wiki)
