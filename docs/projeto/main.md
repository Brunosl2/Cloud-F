# **Projeto - Cloud F**

- link para o video: [Video](https://youtu.be/60M0lqnCr9g)
> O vídeo contém uma demonstração de como funciona a api, mostrando os dados em tempo real acessando o banco Postgres
- link para o repositorio: [Repositorio](https://github.com/juliadn/cloudf-projeto.git)

## KIT-F

Bruno Locatelli

Julia Diniz

## **Descrição do Projeto**

### **Tecnologias Principais**

- **FastAPI**: Framework web moderno e de alto desempenho para construção da API
- **PostgreSQL**: Sistema gerenciador de banco de dados relacional
- **JWT**: Implementação de autenticação segura
- **Docker**: Containerização e orquestração da aplicação

###  **Funcionalidades Principais**

- Registro e autenticação de usuários
- Gerenciamento de despesas pessoais
- Categorização de gastos
- API RESTful com endpoints documentados
- Autenticação via JWT
- Proteção de senhas com hash

###  **Arquitetura da Aplicação**

A aplicação segue uma arquitetura modular, com separação clara de responsabilidades:

- **Models**: Definição dos modelos de dados e relacionamentos
- **Routes**: Endpoints da API e lógica de negócios
- **Auth**: Gerenciamento de autenticação e autorização
- **Database**: Configuração e conexão com o banco de dados
- **Schemas**: Validação e serialização de dados

###  **Infraestrutura Containerizada**

A infraestrutura é totalmente containerizada utilizando Docker e Docker Compose, garantindo:

- Ambiente de desenvolvimento consistente
- Fácil configuração e execução
- Isolamento de serviços (API e banco de dados)
- Portabilidade entre diferentes ambientes
- Simplificação do processo de deploy


### **Execução com Docker Compose**
![Arquitetura Docker](docker.png){width=200px style="display: block; margin: 0 auto;"}

Para executar o projeto localmente, siga os passos abaixo:

1\. **Clone o Repositório**
```bash
# Configurações do PostgreSQL
POSTGRES_USER=seu_usuario
POSTGRES_PASSWORD=sua_senha
POSTGRES_DB=cloud_f
POSTGRES_HOST=db
POSTGRES_PORT=5432

# Configurações da API
API_HOST=0.0.0.0
API_PORT=8000
SECRET_KEY=sua_chave_secreta
```

3\. **Inicie os Containers**
```bash
docker compose up -d
```
   
O comando irá baixar as imagens do Docker Hub e iniciar os serviços da aplicação e do banco de dados.

4\. **Acesse a Documentação da API**
   
Abra seu navegador e acesse:
   
http://localhost:8000/docs

A imagem da API está disponível no [Docker Hub](https://hub.docker.com/repository/docker/juliadn/minha-api/general)

Ou baixe a imagem direto do docker pull:
```bash
docker pull juliadn/minha-api
```

## 🔐 Autenticação com JWT

A aplicação utiliza autenticação baseada em tokens JWT (JSON Web Tokens) para proteger as rotas da API. As senhas dos usuários são armazenadas de forma segura usando hash bcrypt no momento do cadastro.

### Endpoints de Autenticação

#### Registro de Usuário
```http
POST /usuarios/
Content-Type: application/json

{
"nome": "Nome do Usuário",
"email": "usuario@exemplo.com",
"senha": "senha123"
}
```

#### Login
```http
POST /api/v1/auth/login
Content-Type: application/json

{
   "email": "usuario@exemplo.com",
   "password": "senha123"
}
```

A resposta do login incluirá o token JWT:
```json
{
   "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
   "token_type": "bearer"
}
```

### Usando o Token

Para acessar rotas protegidas, inclua o token no header Authorization de todas as requisições:
```http
GET /api/v1/protected-route
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## ☁️ Deploy na Nuvem (AWS Lightsail)

A aplicação foi implantada com sucesso na AWS Lightsail, utilizando os serviços de **containers** e **banco de dados gerenciado**.

### Componentes utilizados:

- `fastapi-service`: container Docker com a API publicada via Docker Hub
- `fastapi-db`: instância de banco PostgreSQL 17.5 (40GB SSD), configurada com variáveis de ambiente

### Passos de Deploy:

1. Upload da imagem no Docker Hub (`juliadn/minha-api`)
2. Criação do container via painel do Lightsail
3. Configuração das variáveis de ambiente no console do container
4. Criação da instância de banco PostgreSQL via painel
5. Conexão da API com o banco via DNS e credenciais da AWS
6. Validação com acesso à documentação Swagger pública

### Screenshots do Deploy

#### 📦 Container da API em execução
![Container FastAPI](container-aws.png)

> **Status do Container FastAPI**
> 
> O container está ativo e operacional no ambiente AWS Lightsail. Detalhes técnicos:
> 
> | Item | Valor |
> |------|-------|
> | Status | 🟢 Running |
> | Container | fastapi-service |
> | Imagem | juliadn/minha-api |
> | Porta | 8000 |
> 
> ✅ **Status**: Container pronto para produção


#### 🛢️ Banco PostgreSQL disponível
![Banco PostgreSQL](fast-db.png)
> **Status do Banco PostgreSQL**
> 
> O banco de dados está ativo e configurado no ambiente AWS Lightsail. Detalhes técnicos:
> 
> | Item | Valor |
> |------|-------|
> | Status | 🟢 Running |
> | Tipo | PostgreSQL 17.5 |
> | Armazenamento | 40GB SSD |
> | Porta | 5432 |
> 
> ✅ **Status**: Banco de dados pronto para produção


#### 💰 Custos do Deploy

![Preços AWS Lightsail](aws_pricing.png)

> **Análise de Custos**
> 
> O deploy foi realizado utilizando os seguintes recursos da AWS Lightsail:
> 
> - Container: $7/mês (512MB RAM, 1 vCPU)
> - PostgreSQL: $15/mês (1GB RAM, 40GB SSD)
> 
> **Total mensal**: $22/mês
> 
> 💡 **Observação**: Os preços podem variar dependendo da região e configurações específicas escolhidas.





