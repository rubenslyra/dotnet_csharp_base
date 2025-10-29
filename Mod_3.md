# 🎓 CURSO PROFISSIONAL DE C# 13 COM .NET 9

## **MÓDULO 3 — ARQUITETURA CLEAN, DDD, CQRS E PERSISTÊNCIA PROFISSIONAL**

### 📘 **Ementa oficial**

Estudo dos princípios da arquitetura limpa aplicados à plataforma .NET 9. Organização de código em camadas independentes (Domain, Application, Infrastructure, Presentation). Modelagem de domínio com base em DDD (Entidades, Value Objects, Repositórios e Serviços de Domínio). Aplicação dos padrões CQRS e Repository com Entity Framework Core 9. Ênfase em escalabilidade, testabilidade e manutenção de longo prazo.

---

### 🎯 **Objetivos de aprendizagem**

O aluno será capaz de:

1. Projetar e implementar **arquiteturas em camadas limpas** (Clean Architecture).
2. Compreender e aplicar os **conceitos de Domain-Driven Design**.
3. Criar **repositórios desacoplados** com EF Core 9.
4. Aplicar **CQRS** para separar comandos e consultas.
5. Integrar padrões modernos de injeção, logging e testes.

---

## 📅 PLANO DETALHADO DE AULAS EXPOSITIVAS

---

### **Aula 1 – O que é Arquitetura Clean e por que ela domina o mercado**

**Tempo:** 4 min

**Conteúdo:**

* Conceito de Arquitetura Clean (Robert C. Martin – Uncle Bob)
* Separação de responsabilidades e independência de frameworks
* Comparação entre MVC, Onion e Clean Architecture
* Estrutura de pastas base (Domain, Application, Infrastructure, API)
* Fluxo de dependência (de dentro para fora)

**Referências:**

* *Clean Architecture* — Robert C. Martin
* Jason Taylor, “Clean Architecture Template for .NET”
* Microsoft Docs: [Architect modern web applications with .NET](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/)

**Problema real:**

> “Aplicações monolíticas com dependências cruzadas entre módulos.”
> **Solução:**
> Isolamento das regras de negócio na camada **Domain**, sem dependência de frameworks.

**Exercício:**
Crie a estrutura inicial de diretórios para o sistema de lanchonete:

```
src/
 ├── Lanchonete.Domain/
 ├── Lanchonete.Application/
 ├── Lanchonete.Infrastructure/
 └── Lanchonete.API/
```

---

### **Aula 2 – O Domínio como o Coração do Sistema**

**Tempo:** 4 min

**Conteúdo:**

* Conceito de **Entidade** e **Value Object**
* Definição do domínio: *Pedido*, *Cliente*, *Produto*
* Identidade, igualdade e imutabilidade
* Regras de negócio puras e coerentes
* Entidades anêmicas: o erro mais comum

**Exemplo:**

```csharp
public class Produto
{
    public Guid Id { get; private set; } = Guid.NewGuid();
    public string Nome { get; private set; }
    public decimal Preco { get; private set; }

    public Produto(string nome, decimal preco)
    {
        if (preco <= 0) throw new ArgumentException("Preço inválido.");
        Nome = nome;
        Preco = preco;
    }
}
```

**Problema real:**

> “Regra de negócio espalhada por controllers.”
> **Solução:**
> Centralizar lógica no domínio, deixando Application e Infra cuidarem de orquestração e persistência.

**Exercício:**
Implemente as entidades `Cliente`, `Produto` e `Pedido` dentro de `Lanchonete.Domain`.

---

### **Aula 3 – Value Objects e Regras de Negócio Ricas**

**Tempo:** 4 min

**Conteúdo:**

* Diferença entre Entidade e Value Object
* Imutabilidade e igualdade estrutural
* Exemplo: `CPF`, `Email`, `Dinheiro`, `Telefone`
* Uso de Records no C# 13 para simplificar Value Objects

**Exemplo:**

```csharp
public record Email
{
    public string Endereco { get; }
    public Email(string endereco)
    {
        if (!endereco.Contains("@")) throw new ArgumentException("Email inválido.");
        Endereco = endereco;
    }
}
```

**Problema real:**

> “Dados inconsistentes ou inválidos no banco.”
> **Solução:**
> Encapsular validações nos Value Objects, garantindo integridade desde a criação.

**Exercício:**
Crie um `record CPF` que valide o formato e impeça valores inválidos.

---

### **Aula 4 – Repositórios e Persistência com EF Core 9**

**Tempo:** 4 min

**Conteúdo:**

* Padrão Repository e abstração de dados
* Interface base (`IRepository<T>`)
* Implementação genérica com EF Core 9
* Mapeamento Fluent API e Migrations
* Contexto de banco de dados desacoplado

**Exemplo:**

```csharp
public interface IRepository<T> where T : class
{
    Task<T?> GetByIdAsync(Guid id);
    Task AddAsync(T entity);
    Task SaveChangesAsync();
}
```

**Problema real:**

> “Controllers acessando DbContext diretamente.”
> **Solução:**
> Camada de repositório centralizada com interfaces no domínio e implementação isolada na infraestrutura.

**Exercício:**
Implemente `IProdutoRepository` e `ProdutoRepository` com EF Core.

---

### **Aula 5 – Application Layer e o Padrão CQRS**

**Tempo:** 4 min

**Conteúdo:**

* Separação de leitura (Query) e escrita (Command)
* Handlers e MediatR como mediador de fluxo
* DTOs e View Models
* Boas práticas: não expor entidades do domínio

**Exemplo:**

```csharp
public record CriarPedidoCommand(Guid ClienteId, List<Guid> Produtos);
public class CriarPedidoHandler : IRequestHandler<CriarPedidoCommand, Guid>
{
    private readonly IPedidoRepository _repo;
    public CriarPedidoHandler(IPedidoRepository repo) => _repo = repo;

    public async Task<Guid> Handle(CriarPedidoCommand request, CancellationToken ct)
    {
        var pedido = new Pedido(request.ClienteId);
        await _repo.AddAsync(pedido);
        await _repo.SaveChangesAsync();
        return pedido.Id;
    }
}
```

**Problema real:**

> “Fluxo de dados confuso e difícil de testar.”
> **Solução:**
> Usar CQRS e MediatR para isolar operações e permitir testes unitários independentes.

**Exercício:**
Implemente um Command `CriarProdutoCommand` com seu respectivo Handler.

---

### **Aula 6 – Services, Domain Events e Validação**

**Tempo:** 4 min

**Conteúdo:**

* Application Services vs. Domain Services
* Eventos de domínio e notificações internas
* Validações e erros ricos (`FluentValidation`, notificações de domínio)
* Encapsulamento de lógica em Services reutilizáveis

**Problema:**

> “Regras espalhadas e sem coesão.”
> **Solução:**
> Mover a regra para `DomainService`, disparar eventos quando entidades mudam de estado.

**Exemplo:**

```csharp
public class PedidoDomainService
{
    public void FecharPedido(Pedido pedido)
    {
        if (!pedido.Itens.Any()) throw new InvalidOperationException("Pedido vazio.");
        pedido.Status = StatusPedido.Fechado;
    }
}
```

**Exercício:**
Crie um `PedidoDomainService` que valide se há estoque antes de fechar um pedido.

---

### **Aula 7 – Logging, Exceptions e Unit of Work**

**Tempo:** 4 min

**Conteúdo:**

* Logging estruturado (Serilog)
* Unit of Work e transações
* Exception handling global
* Patterns `Try/Catch` + Logs + Rollback

**Problema:**

> “Transações quebradas e logs dispersos.”
> **Solução:**
> Implementar um `UnitOfWork` e centralizar logs via Serilog e middleware.

**Exemplo:**

```csharp
public interface IUnitOfWork
{
    Task CommitAsync();
}
```

**Exercício:**
Implemente uma classe `EfUnitOfWork` e use-a no Handler de Pedido.

---

### **Aula 8 – Infraestrutura e Banco de Dados**

**Tempo:** 4 min

**Conteúdo:**

* Configuração de `DbContext`
* Mapeamento de entidades e relacionamentos
* Connection strings e variáveis de ambiente
* Seeds e migrations automatizadas (`dotnet ef`)
* Integração com SQL Server / PostgreSQL

**Problema:**

> “Ambientes divergentes e erros em deploy.”
> **Solução:**
> Separar configurações por ambiente (`appsettings.Development.json`, etc.) e usar migrations consistentes.

**Exercício:**
Configure `LanchoneteDbContext` e aplique a primeira migration com `dotnet ef migrations add Init`.

---

### **Aula 9 – API Layer e Controllers Limpos**

**Tempo:** 4 min

**Conteúdo:**

* Estrutura de controllers em APIs RESTful
* Versionamento de API e Swagger
* Retornos consistentes (DTOs e status codes)
* Filtros e Middlewares globais
* Boas práticas de endpoints

**Exemplo:**

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProdutosController : ControllerBase
{
    private readonly IMediator _mediator;
    public ProdutosController(IMediator mediator) => _mediator = mediator;

    [HttpPost]
    public async Task<IActionResult> Criar([FromBody] CriarProdutoCommand cmd)
        => Ok(await _mediator.Send(cmd));
}
```

**Problema:**

> “Controllers pesados e sem consistência.”
> **Solução:**
> Deixar a camada API fina e delegar toda lógica para Commands e Queries.

**Exercício:**
Crie um `PedidosController` com endpoint para criar pedidos via MediatR.

---

### **Aula 10 – Testes de Unidade e Integração**

**Tempo:** 4 min

**Conteúdo:**

* Testes de domínio com xUnit
* Mocking de repositórios (Moq)
* Testes de API com WebApplicationFactory
* Estratégia AAA (Arrange, Act, Assert)
* Cobertura de código

**Problema:**

> “Refatorar código sem saber se quebrou algo.”
> **Solução:**
> Testes unitários automatizados de domínio e handlers CQRS.

**Exemplo:**

```csharp
[Fact]
public void Nao_Deve_Criar_Pedido_Vazio()
{
    var pedido = new Pedido(Guid.NewGuid());
    Assert.Throws<InvalidOperationException>(() => pedido.Fechar());
}
```

**Exercício:**
Crie um teste unitário para verificar se o método `FecharPedido()` dispara exceção em pedido sem itens.

---

### **Aula 11 – Documentação e Padronização**

**Tempo:** 4 min

**Conteúdo:**

* Swagger/OpenAPI 3.0
* Versionamento e documentação automática
* Convenções de endpoints REST
* Logging e rastreabilidade de requisições (Request ID)
* Documentação de projeto (`README`, diagramas UML, camadas)

**Exercício:**
Adicione Swagger e gere a documentação automática da API.

---

### **Aula 12 – Mini-Projeto Final: Sistema de Lanchonete Completo**

**Tempo:** 4 min

**Descrição:**
Monte a arquitetura final:

```
Lanchonete.Domain/        → Entidades, ValueObjects, Services
Lanchonete.Application/   → Commands, Queries, Handlers
Lanchonete.Infrastructure/→ DbContext, EF Core, Repositories
Lanchonete.API/           → Controllers, Swagger
```

**Funcionalidades mínimas:**

* Cadastro de produto e cliente
* Criação e fechamento de pedido
* Validação de estoque
* Pagamento (Strategy)
* Logging e testes

**Problema:** “Como manter o sistema vivo, modular e testável?”
**Solução:** Clean Architecture + DDD + CQRS → escalabilidade real, manutenibilidade e clareza.

**Exercício final:**
Implemente o projeto completo e publique no GitHub com README técnico e diagrama de camadas.

---

## 📘 **Encerramento do Módulo 3**

**Competências adquiridas:**

* Modelagem e implementação com DDD
* Aplicação real de Clean Architecture
* Domínio do padrão CQRS e repositórios
* Persistência desacoplada com EF Core 9
* Base sólida para Módulo 4 (Segurança, Deploy e Automação)
