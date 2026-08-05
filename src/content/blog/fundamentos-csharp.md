---
title: 'Fundamentos de C#: do primeiro projeto a métodos e exceções'
description: 'Uma introdução prática a C#, .NET, tipos, entrada de dados, decisões, repetições, métodos e tratamento de erros.'
pubDate: '2026-08-05'
---

Aprender C# não significa apenas memorizar palavras-chave. O objetivo é compreender como uma
aplicação é organizada, como os dados são representados e como transformar regras em código
legível e confiável.

Este artigo apresenta uma revisão progressiva dos fundamentos da linguagem. Os exemplos utilizam
aplicações de console porque elas permitem concentrar a atenção na lógica antes de introduzir
interfaces gráficas, APIs ou bancos de dados.

## C#, .NET e o projeto

C# é a linguagem na qual escrevemos as instruções e os modelos da aplicação. O .NET fornece o
compilador, o runtime e as ferramentas necessárias para criar, executar, testar e publicar o
programa.

| Elemento | Responsabilidade |
| --- | --- |
| C# | Expressar dados, instruções e regras do sistema. |
| .NET SDK | Criar, restaurar, compilar, testar e publicar projetos. |
| Runtime .NET | Executar a aplicação compilada. |
| Projeto | Reunir código, dependências e configurações em uma unidade compilável. |
| Solução | Agrupar um ou mais projetos relacionados. |

Para criar uma aplicação de console pelo terminal:

```powershell
dotnet new console -n CentralChamados
cd CentralChamados
dotnet build
dotnet run
```

O arquivo `CentralChamados.csproj` descreve o projeto. O `Program.cs` contém o ponto inicial do
programa. Em projetos modernos, é possível começar diretamente com instruções:

```csharp
Console.WriteLine("Central de Chamados");
Console.WriteLine("-------------------");
```

Compile frequentemente. Erros de compilação indicam que o código não respeita alguma regra da
linguagem; quanto menor a alteração desde a última compilação, mais fácil será localizar a causa.

## Sintaxe e nomes

Uma instrução normalmente termina com `;`. Blocos de código são delimitados por chaves:

```csharp
bool isActive = true;

if (isActive)
{
    Console.WriteLine("Solicitante ativo.");
}
```

Por convenção, classes, métodos e propriedades usam **PascalCase**. Variáveis locais e parâmetros
usam **camelCase**:

```csharp
int ticketNumber = 1042;
string requesterName = "Marina Souza";

Console.WriteLine($"Chamado {ticketNumber} - {requesterName}");
```

Prefira nomes que revelem intenção. `estimatedCost` comunica mais que `value`, assim como
`ClassifyPriority` explica melhor uma ação do que `Process`.

## Variáveis e tipos

O tipo determina quais valores uma variável aceita e quais operações podem ser realizadas.

| Tipo | Uso comum | Exemplo |
| --- | --- | --- |
| `int` | Números inteiros | `int ticketNumber = 1042;` |
| `decimal` | Dinheiro e valores decimais precisos | `decimal estimatedCost = 250.50m;` |
| `double` | Cálculos aproximados | `double distance = 3.14;` |
| `bool` | Estados verdadeiro ou falso | `bool isActive = true;` |
| `char` | Um único caractere | `char channel = 'E';` |
| `string` | Texto | `string title = "Falha de acesso";` |

O sufixo `m` informa que um literal é `decimal`. Para valores financeiros, prefira `decimal` a
`double`, pois cálculos de centavos precisam permanecer previsíveis.

```csharp
int ticketNumber = 1042;
string title = "Falha de acesso";
decimal estimatedCost = 250.50m;
bool isActive = true;
char channel = 'E';
```

O uso de `var` não torna a variável dinâmica. O compilador continua definindo um tipo estático:

```csharp
var department = "Tecnologia"; // inferido como string
```

Use `var` quando o tipo for evidente. Não o utilize para esconder a intenção do código.

## Entrada e conversão segura

`Console.ReadLine()` retorna o texto digitado pelo usuário. Antes de usar esse valor como número,
é necessário convertê-lo.

```csharp
Console.Write("Número do chamado: ");
string input = Console.ReadLine() ?? string.Empty;

if (int.TryParse(input, out int ticketNumber))
{
    Console.WriteLine($"Chamado informado: {ticketNumber}");
}
else
{
    Console.WriteLine("Digite um número inteiro válido.");
}
```

`TryParse` devolve `true` ou `false` sem interromper o programa quando a conversão falha. Esse
comportamento é adequado para dados digitados por pessoas, pois erros de entrada são esperados.

Para campos de texto obrigatórios, use `string.IsNullOrWhiteSpace`:

```csharp
Console.Write("Título: ");
string title = Console.ReadLine() ?? string.Empty;

if (string.IsNullOrWhiteSpace(title))
{
    Console.WriteLine("O título é obrigatório.");
}
```

### Números no formato brasileiro

Separadores decimais e formatos monetários dependem de cultura. Para aceitar `250,50` e exibir
valores em reais, use `pt-BR` tanto na conversão quanto na formatação:

```csharp
using System.Globalization;

CultureInfo brazilianCulture = CultureInfo.GetCultureInfo("pt-BR");

Console.Write("Custo estimado: ");
string costInput = Console.ReadLine() ?? string.Empty;

if (decimal.TryParse(costInput, brazilianCulture, out decimal estimatedCost))
{
    Console.WriteLine(
        $"Custo: {estimatedCost.ToString("C", brazilianCulture)}");
}
else
{
    Console.WriteLine("Use o formato brasileiro. Exemplo: 250,50");
}
```

Conversão e formatação devem seguir a mesma convenção. Isso evita aceitar um formato e apresentar
outro inesperadamente.

## Operadores e expressões

Operadores permitem calcular valores e combinar condições:

```csharp
int estimatedHours = 3;
int availableHours = 5;
bool requesterIsActive = true;
bool departmentIsBlocked = false;

int remainingHours = availableHours - estimatedHours;
bool hasCapacity = remainingHours >= 0;

bool canRegister =
    requesterIsActive &&
    hasCapacity &&
    !departmentIsBlocked;
```

Os operadores mais comuns são:

- Aritméticos: `+`, `-`, `*`, `/` e `%`.
- Comparação: `==`, `!=`, `>`, `>=`, `<` e `<=`.
- Lógicos: `&&`, `||` e `!`.

Divida regras complexas em variáveis booleanas com nomes claros. É mais fácil compreender
`hasCapacity && requesterIsActive` do que interpretar uma expressão extensa sem contexto.

## Decisões

Use `if`, `else if` e `else` quando o programa precisar escolher um caminho:

```csharp
int impact = 3;
int urgency = 2;
string priority;

if (impact == 3 && urgency == 3)
{
    priority = "Alta";
}
else if (impact >= 2 || urgency >= 2)
{
    priority = "Média";
}
else
{
    priority = "Baixa";
}
```

Quando uma expressão precisa produzir diretamente um valor, `switch` pode tornar a regra mais
compacta:

```csharp
int score = 7;

string status = score switch
{
    < 0 or > 10 => "Inválida",
    >= 7 => "Aprovado",
    >= 4 => "Recuperação",
    _ => "Reprovado"
};
```

Valide estados inválidos cedo. Essa técnica, conhecida como *guard clause*, reduz níveis de
indentação e deixa a regra principal mais visível.

## Repetições

Cada estrutura de repetição atende a uma situação:

- `for`: quando existe um contador ou quantidade conhecida.
- `foreach`: para percorrer todos os elementos de uma sequência.
- `while`: enquanto uma condição for verdadeira.
- `do-while`: quando o bloco precisa executar pelo menos uma vez.

```csharp
string[] departments = { "RH", "TI", "Financeiro" };

foreach (string department in departments)
{
    Console.WriteLine(department);
}
```

Uma aplicação de console pode utilizar `do-while` para registrar vários chamados:

```csharp
string option;
int registeredTickets = 0;

do
{
    Console.WriteLine("Registrando um chamado...");
    registeredTickets++;

    Console.Write("Deseja registrar outro? S/N: ");
    option = (Console.ReadLine() ?? string.Empty).Trim().ToUpperInvariant();
}
while (option == "S");

Console.WriteLine($"Total registrado: {registeredTickets}");
```

Garanta que a condição possa mudar. Um laço cuja condição nunca se altera resulta em repetição
infinita.

## Métodos e responsabilidades

Métodos dividem o programa em ações menores. A assinatura informa o tipo de retorno, o nome e os
parâmetros necessários:

```csharp
string ClassifyPriority(int impact, int urgency)
{
    if (impact == 3 && urgency == 3)
    {
        return "Alta";
    }

    if (impact >= 2 || urgency >= 2)
    {
        return "Média";
    }

    return "Baixa";
}

string priority = ClassifyPriority(impact: 3, urgency: 2);
```

Um bom método:

- representa uma responsabilidade específica;
- possui um nome que descreve uma ação;
- recebe apenas os dados necessários;
- devolve um resultado quando existe algo útil a retornar;
- não depende de variáveis globais escondidas.

Em uma Central de Chamados, leitura, validação, classificação, encaminhamento e apresentação são
responsabilidades diferentes. Separá-las torna o fluxo principal mais fácil de ler e testar.

## Validações e exceções

Erros de digitação devem continuar sendo tratados com `TryParse`. Exceções representam falhas ou
violações de contrato que impedem a operação de continuar normalmente.

```csharp
string ClassifyPriority(int impact, int urgency)
{
    if (impact is < 1 or > 3)
    {
        throw new ArgumentOutOfRangeException(
            nameof(impact),
            "O impacto deve estar entre 1 e 3.");
    }

    if (urgency is < 1 or > 3)
    {
        throw new ArgumentOutOfRangeException(
            nameof(urgency),
            "A urgência deve estar entre 1 e 3.");
    }

    if (impact == 3 && urgency == 3)
    {
        return "Alta";
    }

    return impact >= 2 || urgency >= 2 ? "Média" : "Baixa";
}
```

Capture somente as exceções que podem ser tratadas naquele ponto:

```csharp
try
{
    string priority = ClassifyPriority(impact: 3, urgency: 2);
    Console.WriteLine($"Prioridade: {priority}");
}
catch (ArgumentOutOfRangeException exception)
{
    Console.WriteLine($"Entrada inválida: {exception.Message}");
}
finally
{
    Console.WriteLine("Tentativa de cadastro encerrada.");
}
```

Evite blocos como `catch (Exception) { }`. Silenciar uma falha não resolve o problema; apenas
remove as informações necessárias para diagnosticá-lo.

## Projeto prático: Central de Chamados

Uma forma eficiente de praticar esses conceitos é evoluir o mesmo projeto em pequenas etapas:

1. Criar o projeto Console e apresentar um cabeçalho.
2. Representar um chamado com variáveis e tipos adequados.
3. Receber e validar os dados digitados.
4. Calcular capacidade e condições de atendimento.
5. Classificar impacto, urgência e prioridade.
6. Registrar vários chamados na mesma execução.
7. Separar as responsabilidades em métodos.
8. Proteger os contratos com exceções específicas.

Ao final, o programa deve possuir:

- número, título, solicitante, departamento, custo, impacto e urgência;
- validações com mensagens compreensíveis;
- classificação de prioridade baixa, média ou alta;
- controle de capacidade e departamentos bloqueados;
- repetição para cadastrar vários chamados;
- resumo com contadores;
- métodos com nomes e responsabilidades claros;
- tratamento de falhas sem esconder exceções.

O valor desse exercício não está apenas no resultado final. A evolução gradual permite perceber
quando o código começa a concentrar responsabilidades demais e quando uma regra merece ser
extraída para um método.

## Próximos passos

Depois de dominar essa base, avance para:

- classes, objetos e encapsulamento;
- coleções genéricas, especialmente `List<T>`;
- interfaces e injeção de dependência;
- leitura e gravação de arquivos;
- testes automatizados;
- APIs web com ASP.NET Core;
- persistência de dados.

Para aprofundar:

1. [Tour pelo C# — Microsoft Learn](https://learn.microsoft.com/dotnet/csharp/tour-of-csharp/)
2. [Tipos internos do C#](https://learn.microsoft.com/dotnet/csharp/language-reference/builtin-types/built-in-types)
3. [Instruções de seleção](https://learn.microsoft.com/dotnet/csharp/language-reference/statements/selection-statements)
4. [Instruções de iteração](https://learn.microsoft.com/dotnet/csharp/language-reference/statements/iteration-statements)
5. [Exceções e tratamento de erros](https://learn.microsoft.com/dotnet/csharp/fundamentals/exceptions/)

