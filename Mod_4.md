# 🎓 CURSO PROFISSIONAL DE C# 13 COM .NET 9

## **MÓDULO 4 — SEGURANÇA, AUTENTICAÇÃO, AUTORIZAÇÃO E DEPLOY PROFISSIONAL**

### 📘 **Ementa oficial**

Estudo e implementação de mecanismos de segurança aplicados à arquitetura de APIs e sistemas web em .NET 9. Autenticação baseada em tokens (JWT), controle de acesso por Claims e Roles, criptografia, hashing e proteção contra ataques comuns (XSS, CSRF, SQL Injection). Integração com ASP.NET Core Identity, OAuth 2.0 e OpenID Connect. Automação de build, containerização com Docker e deploy contínuo em ambiente produtivo.

---

### 🎯 **Objetivos de aprendizagem**

O aluno será capaz de:

1. Implementar **autenticação e autorização** robustas em APIs RESTful.
2. Configurar **Identity, JWT, Claims e Roles** corretamente.
3. Aplicar **práticas de segurança e criptografia** recomendadas pelo OWASP.
4. Criar pipelines de **build e deploy automatizados** (CI/CD).
5. Publicar um sistema completo e seguro em produção.

---

## 📅 PLANO DETALHADO DE AULAS EXPOSITIVAS

---

### **Aula 1 – Fundamentos de Segurança em Sistemas Web**

**Tempo:** 4 min

**Conteúdo:**

* Conceitos: Autenticação x Autorização x Criptografia
* Segurança em camadas (Defense in Depth)
* Princípios do OWASP Top 10
* Boas práticas no .NET 9: `IConfiguration`, `UserSecrets`, `DataProtection`
* Armazenamento de segredos e chaves

**Referências:**

* [OWASP Top 10 2025](https://owasp.org/www-project-top-ten/)
* Microsoft Docs: [ASP.NET Core Security Overview](https://learn.microsoft.com/aspnet/core/security/)

**Problema real:**

> “Tokens vazando em logs e variáveis de ambiente expostas.”
> **Solução:**
> Armazenar segredos via `dotnet user-secrets` e proteger variáveis no pipeline (GitHub Actions, Azure Key Vault).

**Exercício:**
Ative o recurso `user-secrets` e crie uma chave API falsa com:

```bash
dotnet user-secrets set "JwtKey" "MinhaChaveSuperSegura"
```

---

### **Aula 2 – Hashing e Criptografia**

**Tempo:** 4 min

**Conteúdo:**

* Hashing (SHA256, BCrypt, PBKDF2)
* Salting e verificação segura
* Criptografia simétrica (AES) e assimétrica (RSA)
* Assinatura digital de tokens e documentos
* Uso de `System.Security.Cryptography`

**Problema:**

> “Senhas armazenadas em texto puro ou em hash fraco (MD5/SHA1).”
> **Solução:**
> Uso de `PasswordHasher` do .NET Identity e algoritmos com salt dinâmico.

**Exemplo:**

```csharp
var hasher = new PasswordHasher<string>();
var hash = hasher.HashPassword(null, "senha123");
```

**Exercício:**
Implemente uma função que receba uma senha e retorne o hash usando `PasswordHasher`.

---

### **Aula 3 – ASP.NET Core Identity**

**Tempo:** 4 min

**Conteúdo:**

* O que é o ASP.NET Core Identity
* Estrutura de tabelas e entidades padrão
* Configuração de Identity no `Program.cs`
* Registro, Login e Logout
* Customização de campos (Nome, Cargo, Telefone, etc.)

**Problema:**

> “Sistema precisa de autenticação segura, mas sem reinventar a roda.”
> **Solução:**
> Usar o ASP.NET Core Identity como base, adaptando o modelo a cada domínio.

**Exemplo:**

```csharp
builder.Services.AddIdentity<ApplicationUser, IdentityRole>()
    .AddEntityFrameworkStores<AppDbContext>()
    .AddDefaultTokenProviders();
```

**Exercício:**
Crie a classe `ApplicationUser` derivada de `IdentityUser` e adicione o campo `NomeCompleto`.

---

### **Aula 4 – JWT (JSON Web Token)**

**Tempo:** 4 min

**Conteúdo:**

* Estrutura do JWT: Header, Payload e Signature
* Geração e validação de tokens
* Configuração do `JwtBearer` no .NET
* Claims e Expiração
* Armazenamento seguro de tokens

**Exemplo:**

```csharp
var tokenHandler = new JwtSecurityTokenHandler();
var key = Encoding.ASCII.GetBytes(config["JwtKey"]);
var tokenDescriptor = new SecurityTokenDescriptor
{
    Subject = new ClaimsIdentity(new[] { new Claim("id", user.Id) }),
    Expires = DateTime.UtcNow.AddHours(2),
    SigningCredentials = new SigningCredentials(
        new SymmetricSecurityKey(key),
        SecurityAlgorithms.HmacSha256Signature)
};
var token = tokenHandler.CreateToken(tokenDescriptor);
```

**Problema:**

> “Tokens inválidos, expiração incorreta e falhas de autenticação.”
> **Solução:**
> Gerar tokens com `SecurityTokenDescriptor` e validar com `JwtBearer`.

**Exercício:**
Implemente um endpoint `/login` que gere um token JWT válido com expiração de 2 horas.

---

### **Aula 5 – Autorização com Roles e Claims**

**Tempo:** 4 min

**Conteúdo:**

* Claims vs. Roles
* Autorização via `[Authorize]`, `[AllowAnonymous]` e `[Authorize(Roles="Admin")]`
* Políticas customizadas (`AddAuthorization`)
* Controle de acesso dinâmico baseado em Claims

**Exemplo:**

```csharp
[Authorize(Roles = "Admin")]
[HttpGet("admin/pedidos")]
public IActionResult PedidosAdmin() => Ok("Somente administradores!");
```

**Problema:**

> “Usuários com permissões erradas acessando endpoints sensíveis.”
> **Solução:**
> Aplicar autorização baseada em políticas e Claims específicas.

**Exercício:**
Crie uma policy chamada `FuncionarioAtivoPolicy` que só autoriza usuários com Claim `Ativo = true`.

---

### **Aula 6 – Proteção contra ataques comuns**

**Tempo:** 4 min

**Conteúdo:**

* SQL Injection (e como o EF Core protege por padrão)
* Cross-Site Scripting (XSS) e Content Security Policy
* CSRF (Cross-Site Request Forgery)
* Rate Limiting e Brute Force protection
* Middleware de segurança (`UseHttpsRedirection`, `UseHsts`)

**Problema:**

> “Ataques de brute force e exploração de endpoints públicos.”
> **Solução:**
> Aplicar rate limiting e validações de entrada (`FluentValidation`).

**Exercício:**
Configure `builder.Services.AddRateLimiter()` com limite de 5 requisições por minuto para endpoints anônimos.

---

### **Aula 7 – Logging e Auditoria**

**Tempo:** 4 min

**Conteúdo:**

* Logging com Serilog e Sink para arquivos e console
* Logs estruturados e contexto de requisição
* Auditoria de ações sensíveis (ex: login, alteração de senha)
* Armazenamento de logs no banco de dados ou Elastic Stack

**Exemplo:**

```csharp
Log.Information("Usuário {UserId} efetuou login às {Data}", user.Id, DateTime.UtcNow);
```

**Problema:**

> “Impossível rastrear ações suspeitas de usuários.”
> **Solução:**
> Auditoria automática com logs persistentes.

**Exercício:**
Implemente log de auditoria sempre que um novo pedido for criado.

---

### **Aula 8 – Publicação e Deploy com Docker**

**Tempo:** 4 min

**Conteúdo:**

* Criação de `Dockerfile` para APIs .NET 9
* Configuração de `docker-compose`
* Build, run e networking entre containers
* Estratégia multi-stage build para otimização
* Deploy no servidor (Hostinger, Azure, VPS)

**Exemplo:**

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
COPY . .
ENTRYPOINT ["dotnet", "Lanchonete.API.dll"]
```

**Problema:**

> “Aplicação funciona localmente, mas falha em produção.”
> **Solução:**
> Usar containers idênticos entre desenvolvimento e produção.

**Exercício:**
Crie o `Dockerfile` do projeto da Lanchonete e execute localmente com `docker-compose up`.

---

### **Aula 9 – CI/CD com GitHub Actions**

**Tempo:** 4 min

**Conteúdo:**

* Conceitos de Integração Contínua (CI) e Entrega Contínua (CD)
* Criação de workflow YAML no GitHub Actions
* Build, test e deploy automatizados
* Uso de secrets e variáveis de ambiente
* Deploy automatizado para servidor SSH ou Docker Hub

**Exemplo:**

```yaml
name: build_and_deploy
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: 9.0.x
      - run: dotnet build --configuration Release
```

**Problema:**

> “Deploy manual e propenso a erro.”
> **Solução:**
> Automatizar pipeline com testes e build a cada push.

**Exercício:**
Crie um workflow `.github/workflows/build.yml` que execute `dotnet test` e `dotnet publish`.

---

### **Aula 10 – Monitoramento e Observabilidade**

**Tempo:** 4 min

**Conteúdo:**

* Health Checks e endpoints `/health`
* Métricas com Prometheus e Grafana
* Application Insights e OpenTelemetry
* Alertas e logs centralizados
* Diagnóstico de falhas em produção

**Problema:**

> “API fora do ar e ninguém percebe.”
> **Solução:**
> Implementar monitoramento contínuo e alertas automáticos.

**Exemplo:**

```csharp
builder.Services.AddHealthChecks();
app.MapHealthChecks("/health");
```

**Exercício:**
Adicione um endpoint `/health` e configure logs básicos de inicialização.

---

### **Aula 11 – Empacotamento e Publicação Final**

**Tempo:** 4 min

**Conteúdo:**

* `dotnet publish` e artefatos de release
* Compactação e versionamento
* Deploy em servidor Linux via `scp` ou `rsync`
* Scripts automatizados de reinício (Systemd, PM2, etc.)
* Boas práticas de rollback

**Exercício:**
Publique a API da Lanchonete com:

```bash
dotnet publish -c Release -o ./out
```

E execute via Docker ou `systemctl`.

---

### **Aula 12 – Revisão e Integração Final**

**Tempo:** 4 min

**Descrição:**
Unir tudo: segurança + deploy + monitoramento.
O sistema agora possui:

* Login e autenticação JWT
* Controle de acesso por Roles e Claims
* Logs e auditoria
* Deploy automatizado com Docker
* Endpoint de saúde monitorado

**Exercício final:**
Publique o projeto no GitHub com documentação (README) descrevendo:

* Setup local
* Autenticação e rotas
* Deploy e monitoramento

---

## 📘 **Encerramento do Módulo 4**

**Competências adquiridas:**

* Implementação de autenticação segura (JWT + Identity)
* Proteção contra ameaças OWASP
* Deploy Dockerizado com CI/CD automatizado
* Monitoramento, logging e auditoria real
* Aplicação pronta para produção

