# 🎓 CURSO PROFISSIONAL DE C# 13 COM .NET 9

## **MÓDULO 1 — FUNDAMENTOS MODERNOS DA LINGUAGEM C# E PLATAFORMA .NET**

### 📘 **Ementa oficial**

Introdução prática e conceitual à linguagem C# 13 e à plataforma .NET 9, abordando estrutura de execução, tipos de dados, variáveis, operadores, controle de fluxo, coleções, strings, memória e tratamento de exceções. Ênfase em boas práticas, performance, imutabilidade e segurança de tipo. Aplicação imediata em projetos de linha de produção.

### 🎯 **Objetivos de aprendizagem**

O aluno será capaz de:

1. Compreender o funcionamento interno do .NET Runtime (CLR, JIT, GC).
2. Dominar a criação e o uso de tipos e variáveis no C# 13.
3. Escrever código limpo, expressivo e seguro contra erros comuns.
4. Aplicar estruturas de controle, operadores modernos e boas práticas de memória.
5. Construir programas assíncronos, robustos e performáticos.

---

## 📅 PLANO DETALHADO DE AULAS EXPOSITIVAS

### **Aula 1 – Introdução ao .NET 9 e ao C# 13**

**Tempo:** 4 min

**Conteúdo:**

* Histórico da plataforma .NET (Framework → Core → Unified Runtime)
* Estrutura do SDK (.NET CLI, templates, runtime, assemblies)
* Relação entre .NET 9, C# 13 e o compilador Roslyn
* Tipos de projetos e entry point (`Program.cs`, top-level statements)
* Novidades do C# 13: interceptors, params collections, primary constructors em classes comuns

**Referências:**

* Microsoft Learn – [.NET Fundamentals](https://learn.microsoft.com/dotnet/fundamentals)
* *Pro C# 13 and the .NET 9 Platform* – Apress

**Problema real:** “Projetos legados lentos e difíceis de manter.”
**Solução de mercado:** Migração para .NET 9 reduz consumo de memória e aumenta throughput (ex.: StackOverflow migrou em 2023 e relatou 25 % de ganho).

**Exercício:**
Crie um app console que exiba a versão do runtime e a data atual com interpolação de strings.

---

### **Aula 2 – Variáveis, Tipos e Inferência de Tipos**

**Tempo:** 4 min

**Conteúdo:**

* Declaração e escopo de variáveis (`var`, `readonly`, `const`)
* Tipos primitivos (inteiros, ponto flutuante, booleanos, caracteres)
* Tipos referenciais e valor
* Inferência de tipo (`var`, `dynamic`, `object`)
* Conversões implícitas e explícitas, boxing/unboxing
* Imutabilidade e stack vs. heap

**Problema real:** “Erros de cast e lentidão em coleções heterogêneas.”
**Solução:** Uso de genéricos e inferência forte (`var`), evitando boxing desnecessário.

**Exercício:**
Implemente um programa que leia valores de diferentes tipos e demonstre conversões seguras e inseguras.

---

### **Aula 3 – Tipos de Dados e Estruturas de Memória**

**Tempo:** 4 min

**Conteúdo:**

* Value Types vs. Reference Types
* Estruturas (`struct`), classes e records
* Nullable Value Types (`int?`, `DateTime?`)
* Tipos imutáveis e readonly structs
* Tipos dinâmicos e late binding
* Uso de `Span<T>` e `Memory<T>` para otimização de memória

**Referências:**

* Stephen Toub – Performance Improvements in .NET 9
* JetBrains Rider Blog – High-performance .NET

**Problema:** “APIs lentas com alto GC pressure.”
**Solução:** Uso de `Span<T>` em vez de cópias de array e pooling de memória.

**Exercício:**
Implemente uma função que inverta uma string usando `ReadOnlySpan<char>`.

---

### **Aula 4 – Operadores e Expressões**

**Tempo:** 4 min

**Conteúdo:**

* Operadores aritméticos, relacionais e lógicos
* Operadores condicionais (`?:`, `??`, `??=`, `?.`)
* Operadores `is`, `as`, e pattern matching básico
* Expressões lambda (`=>`) e expressões switch
* Prioridade e associatividade

**Problema:** “Código extenso de if/else e duplicação de lógica.”
**Solução:** Substituir blocos por `switch expressions` e patterns, reduzindo 40 % de linhas.

**Exercício:**
Crie um programa que classifique a nota de 0 a 10 em conceitos (A, B, C, D, F) usando switch expressions.

---

### **Aula 5 – Estruturas de Controle de Fluxo**

**Tempo:** 4 min

**Conteúdo:**

* Estruturas condicionais: `if`, `else if`, `switch`
* Estruturas de repetição: `for`, `foreach`, `while`, `do-while`
* Interrupções de loop: `break`, `continue`, `return`
* Iteradores (`yield return`) e `IEnumerable`
* `goto` e porque evitá-lo

**Problema:** “Código procedural difícil de manter.”
**Solução:** Abstrair lógica em funções pequenas com controle explícito e uso de iteradores.

**Exercício:**
Simule um sorteio com 5 números aleatórios entre 1 e 60 sem repetições usando `HashSet<int>`.

---

### **Aula 6 – Coleções e Genéricos**

**Tempo:** 4 min

**Conteúdo:**

* Arrays, `List<T>`, `Dictionary<TKey,TValue>`, `Queue<T>`, `Stack<T>`
* Iteração e busca em coleções
* Generics e constraints (`where T : class`)
* Diferença entre `IEnumerable`, `ICollection` e `IList`
* LINQ básico (`Select`, `Where`, `OrderBy`, `GroupBy`)

**Problema:** “Consultas ineficientes e uso incorreto de listas.”
**Solução:** LINQ e coleções genéricas otimizadas → menos CPU e GC.

**Exercício:**
Monte uma lista de produtos e calcule a média de preços com LINQ.

---

### **Aula 7 – Strings e Interpolação Avançada**

**Tempo:** 4 min

**Conteúdo:**

* `string` imutável e interning
* `StringBuilder` e concatenação performática
* Interpolação e formatação avançada (`$"{var:N2}"`)
* Strings verbatim e raw strings (C# 11 +)
* Comparações culturais (`CultureInfo`, `StringComparison`)

**Problema:** “Aplicações multilíngues com erros de acentuação.”
**Solução:** Uso de `CultureInfo.InvariantCulture` e Unicode normalization.

**Exercício:**
Crie um formatador de preço internacional (BRL, USD, EUR) com interpolação e CultureInfo.

---

### **Aula 8 – Tratamento de Exceções e Depuração**

**Tempo:** 4 min

**Conteúdo:**

* Hierarquia de exceções (`System.Exception`)
* Blocos `try` / `catch` / `finally`
* Lançamento (`throw`) e exceções personalizadas
* `when` filters (`catch(Exception ex) when(...)`)
* Logging de erros com `ILogger` e `Console.WriteLine`
* Boas práticas de tratamento

**Problema:** “APIs travam silenciosamente.”
**Solução:** Logging estruturado com Serilog e filtros condicionais.

**Exercício:**
Crie um programa que leia um arquivo e trate exceções de arquivo não encontrado e erro de formato.

---

### **Aula 9 – Métodos, Parâmetros e Overload**

**Tempo:** 4 min

**Conteúdo:**

* Estrutura de métodos e retorno de valores
* Passagem por valor, referência e saída (`ref`, `out`)
* Parâmetros opcionais e nomeados
* Sobrecarga e assinaturas distintas
* Funções locais e expressões lambda anônimas

**Problema:** “Código duplicado em métodos semelhantes.”
**Solução:** Sobrecarga de métodos e refatoração funcional.

**Exercício:**
Implemente uma calculadora com overloads para inteiros, decimais e double.

---

### **Aula 10 – Programação Assíncrona e Await/Async**

**Tempo:** 4 min

**Conteúdo:**

* Programação concorrente em .NET
* Modelo de tarefas (`Task`, `Task<T>`, `ValueTask`)
* Palavra-chave `await` e cancellation tokens
* Problemas de deadlocks e soluções
* Async streams (`await foreach`)

**Problema:** “APIs lentas em operações I/O.”
**Solução:** Async I/O com `HttpClient` e `ValueTask` reduz alocações.

**Exercício:**
Crie um método que consome uma API pública ([https://api.agify.io](https://api.agify.io)) e exibe a resposta assíncronamente.

---

### **Aula 11 – Boas Práticas, Comentários e Estilo de Código**

**Tempo:** 4 min

**Conteúdo:**

* Convenções de nome (PascalCase, camelCase, *snake_case*)
* Comentários XML docs e triple-slash `///`
* Linters: EditorConfig, Roslyn Analyzers, StyleCop
* Convenções de arquitetura: pastas, namespaces, usings globais
* Introdução ao conceito de *Clean Code*

**Problema:** “Equipes com estilos divergentes e código confuso.”
**Solução:** Padronização CI/CD com linters automáticos.

**Exercício:**
Crie um arquivo `.editorconfig` com suas regras de estilo e rode `dotnet format`.

---

### **Aula 12 – Mini Projeto Prático: “Fundamentos .NET”**

**Tempo:** 4 min

**Descrição:**
Monte um aplicativo console que:

* Leia nome e idade do usuário;
* Use pattern matching para classificar faixa etária;
* Grave os dados em um arquivo TXT de forma assíncrona;
* Faça tratamento de exceções e log;
* Aplique interpolação, records e nullable types.

**Problemas enfrentados:**

* “Como garantir que o arquivo seja gravado sem bloqueio de thread?”
* **Solução:** usar `await File.WriteAllTextAsync()` com tratamento de erro em `try/catch`.

**Referência:** Estrutura base para projetos de utilidade real (emulando microserviços de registro ou logging de usuários).

---

## 📘 **Encerramento do Módulo 1**

**Competências adquiridas:**

* Escrita de código limpo, performático e seguro
* Entendimento profundo dos tipos e memória
* Uso avançado de estruturas de controle e exceções
* Aplicação de padrões modernos e async I/O
* Preparação para POO Avançada, LINQ e Arquitetura Clean


Se quiser, posso agora preparar o **MÓDULO 2 – Programação Orientada a Objetos Avançada**, já mantendo o mesmo formato (ementa, aulas curtas, referências, problemas × soluções, e exercícios práticos).
Deseja que eu avance com o Módulo 2, ou prefere que eu monte antes o **gabarito completo do Módulo 1 (Parte 2)**?
