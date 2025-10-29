# 🎓 CURSO PROFISSIONAL DE C# 13 COM .NET 9

## **MÓDULO 2 — PROGRAMAÇÃO ORIENTADA A OBJETOS AVANÇADA**

### 📘 **Ementa oficial**

Estudo aprofundado dos princípios da programação orientada a objetos (POO) aplicados à linguagem C# 13 e ao runtime do .NET 9. Análise e implementação dos pilares da orientação a objetos (abstração, encapsulamento, herança e polimorfismo), princípios SOLID, injeção de dependência e padrões de projeto. Ênfase em arquitetura limpa, design evolutivo e legibilidade de código corporativo.

---

### 🎯 **Objetivos de aprendizagem**

O aluno será capaz de:

1. Projetar classes, interfaces e hierarquias com responsabilidade clara.
2. Aplicar os princípios **SOLID** em cenários reais.
3. Entender e implementar **herança, polimorfismo e abstração** de forma correta.
4. Criar aplicações modulares usando **injeção de dependência** e **padrões de projeto**.
5. Refatorar código procedural para orientação a objetos moderna.

---

## 📅 PLANO DETALHADO DE AULAS EXPOSITIVAS

---

### **Aula 1 – Conceitos Fundamentais da POO**

**Tempo:** 4 min

**Conteúdo:**

* O que é a Programação Orientada a Objetos e por que ela existe
* Diferença entre paradigma procedural e orientado a objetos
* Classes, objetos, atributos, métodos e mensagens
* Pilares da POO (Abstração, Encapsulamento, Herança, Polimorfismo)
* Como o CLR (Common Language Runtime) gerencia objetos em memória

**Referências:**

* Grady Booch, *Object-Oriented Analysis and Design with Applications*
* Microsoft Learn – [C# Classes and Objects](https://learn.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/classes)

**Problema real:** “O código é funcional, mas impossível de manter.”
**Solução:** Empresas como a Microsoft e a Spotify aplicam abstração e responsabilidade única para criar módulos desacoplados e testáveis.

**Exercício:**
Implemente uma classe `Cliente` com propriedades e um método `Apresentar()`.
Instancie 3 clientes e exiba seus dados no console.

---

### **Aula 2 – Abstração e Modelagem de Domínio**

**Tempo:** 4 min

**Conteúdo:**

* Identificação de entidades e comportamentos
* Modelagem de classes com base no domínio (DDD simplificado)
* Diferença entre classes concretas e abstratas
* Reuso de comportamento e extensão de modelos
* Encapsulamento de detalhes internos (ocultação de dados)

**Problema:** “Domínio mal definido e classes genéricas que fazem de tudo.”
**Solução:** Aplicação de abstração e interfaces para reduzir acoplamento e aumentar coesão.

**Exemplo prático:**

```csharp
public abstract class Pagamento
{
    public decimal Valor { get; set; }
    public abstract void Processar();
}

public class Pix : Pagamento
{
    public override void Processar() => Console.WriteLine($"Pagando R$ {Valor} via PIX");
}
```

**Exercício:**
Implemente uma classe abstrata `Funcionario` e classes derivadas `Gerente` e `Atendente` com comportamentos diferentes.

---

### **Aula 3 – Encapsulamento e Modificadores de Acesso**

**Tempo:** 4 min

**Conteúdo:**

* Propriedades (`get`/`set`) e campos privados
* Encapsulamento de lógica de validação
* Modificadores (`public`, `private`, `protected`, `internal`)
* Imutabilidade e propriedades calculadas
* Auto-properties e init-only properties (`init;`)

**Problema:** “Dados sensíveis expostos e lógica espalhada.”
**Solução:** Encapsular o estado interno e validar dados nas propriedades.

**Exemplo prático:**

```csharp
public class Conta
{
    private decimal saldo;
    public decimal Saldo => saldo;
    public void Depositar(decimal valor)
    {
        if (valor <= 0) throw new ArgumentException("Valor inválido");
        saldo += valor;
    }
}
```

**Exercício:**
Crie uma classe `ContaBancaria` que permita depósitos e saques com validação e saldo protegido.

---

### **Aula 4 – Herança e Reuso de Código**

**Tempo:** 4 min

**Conteúdo:**

* Herança simples e hierarquias
* Uso de `base` e sobrescrita de métodos (`override`, `virtual`)
* Construtores em classes derivadas
* Herança vs. composição — quando usar cada uma
* Problemas do acoplamento vertical

**Problema:** “Classes gigantes e duplicadas em vários módulos.”
**Solução:** Extração de classes base comuns e uso de composição para especialização.

**Exemplo:**

```csharp
public class Pessoa
{
    public string Nome { get; set; }
    public virtual void Falar() => Console.WriteLine("Falando...");
}

public class Professor : Pessoa
{
    public override void Falar() => Console.WriteLine("Explicando um conceito...");
}
```

**Exercício:**
Implemente uma hierarquia de veículos (`Veiculo`, `Carro`, `Moto`) e sobrescreva o método `Mover()`.

---

### **Aula 5 – Polimorfismo e Interfaces**

**Tempo:** 4 min

**Conteúdo:**

* Polimorfismo de sobrescrita e de interface
* Contratos de código (`interface`)
* Implementações explícitas e múltiplas interfaces
* Dependência em abstrações (DIP - SOLID)
* Uso de `sealed`, `abstract` e `virtual`

**Problema:** “Módulos difíceis de testar porque dependem de classes concretas.”
**Solução:** Interfaces e injeção de dependência — base da arquitetura moderna.

**Exemplo:**

```csharp
public interface INotificacao
{
    void Enviar(string mensagem);
}

public class EmailService : INotificacao
{
    public void Enviar(string mensagem)
        => Console.WriteLine($"Enviando email: {mensagem}");
}
```

**Exercício:**
Crie duas implementações de `INotificacao`: uma para `Email` e outra para `SMS`.

---

### **Aula 6 – Princípios SOLID**

**Tempo:** 4 min

**Conteúdo:**

* S — Single Responsibility: uma classe deve ter uma única razão para mudar.
* O — Open/Closed: aberta para extensão, fechada para modificação.
* L — Liskov Substitution: subtipos devem poder substituir supertipos.
* I — Interface Segregation: dividir interfaces muito grandes.
* D — Dependency Inversion: depender de abstrações, não implementações.

**Problema:** “Alterar um módulo quebra os outros.”
**Solução:** Aplicar SOLID para modularizar e permitir extensão sem impacto colateral.

**Exemplo:**

```csharp
public interface IRelatorio { void Gerar(); }
public class RelatorioPDF : IRelatorio { public void Gerar() { /*...*/ } }
public class RelatorioHTML : IRelatorio { public void Gerar() { /*...*/ } }
```

**Exercício:**
Refatore o código de `ContaBancaria` para respeitar SRP e DIP.

---

### **Aula 7 – Injeção de Dependência (IoC)**

**Tempo:** 4 min

**Conteúdo:**

* O que é Inversão de Controle
* Injeção via construtor, método e propriedade
* `IServiceCollection` e `ServiceProvider` no .NET
* Ciclo de vida (Singleton, Scoped, Transient)
* Configuração em `Program.cs`

**Problema:** “Classe depende de implementação fixa, difícil de trocar.”
**Solução:** Usar injeção de dependência para desacoplar e permitir testes.

**Exemplo:**

```csharp
builder.Services.AddScoped<INotificacao, EmailService>();
```

**Exercício:**
Monte um projeto console usando `Microsoft.Extensions.DependencyInjection` para injetar uma interface `ILogService`.

---

### **Aula 8 – Padrões de Projeto (Design Patterns)**

**Tempo:** 4 min

**Conteúdo:**

* Padrões criacionais: Factory Method, Singleton
* Padrões estruturais: Adapter, Decorator
* Padrões comportamentais: Strategy, Observer
* Aplicações reais em sistemas .NET (Ex: MediatR, ASP.NET Middleware)

**Problema:** “Código duplicado e difícil de expandir.”
**Solução:** Aplicar padrões que se repetem em larga escala (Strategy em cálculos, Observer em notificações).

**Exemplo:**

```csharp
public interface ICalculo { decimal Calcular(decimal valor); }

public class CalculoDesconto : ICalculo
{
    public decimal Calcular(decimal valor) => valor * 0.9m;
}
```

**Exercício:**
Implemente o padrão **Strategy** para calcular o valor final de um pedido com desconto variável.

---

### **Aula 9 – POO Aplicada: Simulando um Sistema Real**

**Tempo:** 4 min

**Descrição:**
Simule um **sistema de pedidos de lanchonete** (começo do seu projeto final):

* Entidades: `Cliente`, `Pedido`, `Item`, `Produto`
* Serviços: `PagamentoService`, `EntregaService`
* Interfaces: `IPagamento`, `INotificacao`
* Princípios: SRP, DIP e injeção de dependência

**Exemplo simplificado:**

```csharp
public interface IPagamento { void Processar(decimal valor); }

public class PagamentoCartao : IPagamento
{
    public void Processar(decimal valor)
        => Console.WriteLine($"Pagando R$ {valor:F2} no cartão.");
}
```

**Desafio:**
Monte a estrutura completa (sem persistência ainda).

---

### **Aula 10 – Revisão e Refatoração Orientada a Objetos**

**Tempo:** 4 min

**Conteúdo:**

* Detectando “code smells”
* Técnicas de refatoração (Extract Class, Replace Inheritance with Composition)
* Testabilidade e modularização
* Boas práticas de nomenclatura e documentação

**Problema:** “Crescimento exponencial da complexidade.”
**Solução:** Revisar arquitetura, dividir responsabilidades e preparar o código para persistência (Módulo 3).

**Exercício final:**
Refatore o sistema da lanchonete para aplicar **Strategy (pagamentos)** e **Dependency Injection (notificações)** corretamente.

---

## 📘 **Encerramento do Módulo 2**

**Competências adquiridas:**

* Domínio completo da POO moderna no C# 13
* Aplicação prática dos princípios SOLID
* Capacidade de projetar sistemas desacoplados e testáveis
* Preparação para trabalhar com **DDD, CQRS e EF Core (Módulo 3)**

