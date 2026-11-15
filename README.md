# Entregas por etapa — Sistema de Gestão de Finanças Pessoais

Cópias dos arquivos do sistema, separadas na ordem em que as etapas foram desenvolvidas.
O sistema completo continua na pasta principal do projeto; aqui estão só os arquivos de cada entrega.

## 1. Criação das tabelas no banco
| Arquivo | O que faz |
|---|---|
| `database/schema.sql` | Script SQL que cria as 4 tabelas (`usuario`, `conta`, `categoria`, `lancamento`), com chaves primárias, chaves estrangeiras e relacionamentos |
| `install.php` | Instalador: cria o banco `financas_pessoais`, executa o `schema.sql` e cadastra um usuário de demonstração |

## 2. Estrutura do back-end (api.php, conexão PDO, sessão)
| Arquivo | O que faz |
|---|---|
| `api.php` | Ponto de entrada único da API: tabela de rotas, verificação do token CSRF e tratamento de erros |
| `config/config.php` | Dados de acesso ao MySQL (servidor, banco, usuário, senha) e fuso horário |
| `app/bootstrap.php` | Inicialização: carrega a configuração e registra o carregamento automático das classes |
| `app/Database.php` | Conexão com o MySQL via PDO (uma conexão por requisição, padrão Singleton) |
| `app/helpers.php` | Sessão (`iniciarSessao`, `usuarioLogado`), token CSRF, respostas em JSON e validações |

## 3. Login, cadastro e logout (back-end)
| Arquivo | O que faz |
|---|---|
| `app/controllers/AuthController.php` | Rotas `auth/sessao`, `auth/login`, `auth/cadastro` e `auth/logout`: validação, conferência da senha, criação e encerramento da sessão |
| `app/models/Usuario.php` | Acesso à tabela `usuario`: busca por e-mail, busca por id e cadastro com senha criptografada |

## Dependências entre as etapas
- A etapa 2 precisa do banco criado na etapa 1.
- A etapa 3 usa a conexão e as funções de sessão da etapa 2 (`Database.php` e `helpers.php`)
  e a tabela `usuario` da etapa 1.
- O cadastro (`AuthController::cadastro`) também cria a conta "Carteira" e as categorias padrão,
  usando os models `Conta.php` e `Categoria.php`, que ficam na pasta principal do projeto.
