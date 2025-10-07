# Design Pattern Singleton

---

## Sumário

1. [Introdução aos Design Patterns](#1-introdução-aos-design-patterns)
2. [História dos Design Patterns](#2-história-dos-design-patterns)
3. [Classificação dos Design Patterns](#3-classificação-dos-design-patterns)
4. [O Pattern Singleton](#4-o-pattern-singleton)
5. [Atividade Prática](#5-atividade-prática)
6. [Referências Complementares](#6-referências-complementares)

---

## 1. Introdução aos Design Patterns

### O que são Design Patterns?

Design Patterns (Padrões de Projeto) são soluções consolidadas e reutilizáveis para problemas recorrentes no desenvolvimento de software orientado a objetos. Eles representam as melhores práticas utilizadas por desenvolvedores experientes ao longo dos anos.

### Para que servem?

Os Design Patterns servem para:

- **Reutilizar soluções comprovadas**: Evitar reinventar a roda para problemas já conhecidos
- **Facilitar a comunicação**: Criar um vocabulário comum entre desenvolvedores
- **Melhorar a manutenibilidade**: Produzir código mais organizado e compreensível
- **Promover boas práticas**: Aplicar princípios como baixo acoplamento e alta coesão
- **Acelerar o desenvolvimento**: Reduzir o tempo gasto em decisões de design

### Definição Clássica

Segundo Christopher Alexander, arquiteto que inspirou o conceito:

> "Um Pattern descreve um problema que se repete várias vezes em um determinado meio, e em seguida descreve o núcleo da sua solução, de modo que esta solução possa ser usada milhares e milhares de vezes."

---

## 2. História dos Design Patterns

### Origem na Arquitetura

O conceito de padrões nasceu na arquitetura civil com Christopher Alexander nos anos 1970. Ele identificou padrões recorrentes no design de edifícios e cidades que poderiam ser documentados e reutilizados.

### A Gang of Four (GoF)

Em **1994**, quatro autores revolucionaram a engenharia de software:

- **Erich Gamma**
- **Richard Helm**
- **Ralph Johnson**
- **John Vlissides**

Eles publicaram o livro seminal **"Design Patterns: Elements of Reusable Object-Oriented Software"**, conhecido como o **GoF book**, que catalogou 23 padrões de projeto fundamentais.

### Impacto na Indústria

O livro da GoF tornou-se uma referência essencial e estabeleceu um vocabulário comum para desenvolvedores discutirem soluções de design. Desde então, muitos outros padrões foram documentados e novos catálogos surgiram.

---

## 3. Classificação dos Design Patterns

Os 23 padrões do GoF são classificados em duas dimensões:

### Por Escopo

- **Padrões de Classe**: Tratam do relacionamento entre classes e subclasses (herança)
- **Padrões de Objeto**: Tratam de relacionamentos entre objetos (composição)

### Por Propósito

#### Padrões Criacionais
Tratam do processo de **criação de objetos**, abstraindo a instanciação.

**Exemplos:** Singleton, Factory Method, Abstract Factory, Builder, Prototype

#### Padrões Estruturais
Tratam da **composição de classes e objetos** para formar estruturas maiores.

**Exemplos:** Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy

#### Padrões Comportamentais
Caracterizam como **classes e objetos interagem** e distribuem responsabilidades.

**Exemplos:** Strategy, Observer, Command, Iterator, Template Method, State, Visitor, Chain of Responsibility, Mediator, Memento, Interpreter

---

## 4. O Pattern Singleton

### Definição

O padrão **Singleton** é um padrão criacional que garante que:

1. Uma classe tenha **apenas uma instância** de si mesma
2. Forneça um **ponto global de acesso** a essa instância

### Quando usar?

Use o Singleton quando:

- Deve existir exatamente uma instância de uma classe no sistema
- Essa instância única deve ser acessível de forma global
- A inicialização deve ser controlada (por exemplo, lazy initialization)

### Exemplos de Uso Real

- **Gerenciadores de configuração**: Um único objeto carrega e mantém as configurações do sistema
- **Pool de conexões**: Uma única instância gerencia todas as conexões com banco de dados
- **Logger**: Um único registro centralizado de logs do sistema
- **Fila de impressão**: Uma única fila para gerenciar documentos a serem impressos
- **Cache global**: Uma única instância de cache compartilhada pela aplicação

### Estrutura

```
┌─────────────────┐
│   Singleton     │
├─────────────────┤
│ -instance       │ ← Atributo estático privado
├─────────────────┤
│ -Singleton()    │ ← Construtor privado
│ +getInstance()  │ ← Método estático público
└─────────────────┘
```

### Implementação Básica em Java

```java
public class Singleton {
    // Instância única (estática e privada)
    private static Singleton instance;
    
    // Construtor privado impede instanciação externa
    private Singleton() {
        // Inicialização
    }
    
    // Método público para obter a instância
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### Lazy Initialization

**Lazy initialization** (inicialização preguiçosa) é uma técnica onde a instância do Singleton só é criada quando realmente necessária, não no momento da carga da classe.

**Vantagens:**
- Economiza recursos se a instância nunca for usada
- Permite adiar inicializações pesadas

**Desvantagens:**
- Requer tratamento especial em ambientes multi-thread

### Thread-Safe Singleton

Em ambientes multi-thread, a implementação básica pode criar múltiplas instâncias. Soluções:

#### Opção 1: Synchronized Method

```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### Opção 2: Double-Checked Locking

```java
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

#### Opção 3: Eager Initialization

```java
public class Singleton {
    private static final Singleton instance = new Singleton();
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

### Vantagens

- Controle rigoroso sobre a instância única
- Acesso global controlado
- Economia de memória (apenas uma instância)
- Permite refinamento através de subclasses (com cuidado)

### Desvantagens

- Viola o Princípio da Responsabilidade Única (controla sua criação e funcionalidade)
- Pode dificultar testes unitários (estado global)
- Pode mascarar design ruim (acesso global excessivo)
- Requer atenção especial em ambientes multi-thread

---

## 5. Atividade Prática

### Contexto

Um desenvolvedor precisa implementar um sistema de **Fila de Impressão** onde:

- Deve existir apenas **uma fila** no sistema
- Qualquer parte do sistema que enviar documentos deve usar a **mesma fila**
- A fila deve gerenciar documentos a serem impressos

### Diagrama UML

```
┌─────────────────────────────────────┐
│           FilaImpressao             │
├─────────────────────────────────────┤
│ -instance: FilaImpressao            │
│ -documentos: List<String>           │
├─────────────────────────────────────┤
│ -FilaImpressao()                    │
│ +getInstance(): FilaImpressao       │
│ +adicionarDocumento(doc: String)    │
│ +removerDocumento(): String         │
│ +removerTodosDocumentos(): void     │
│ +listarDocumentos(): List<String>   │
│ +tamanhoFila(): int                 │
└─────────────────────────────────────┘
```

### Atividade

Implemente a classe `FilaImpressao` seguindo o padrão Singleton.

**Requisitos:**

1. A classe deve garantir que apenas uma instância seja criada
2. Não é necessário implementar todos os métodos do diagrama UML
3. O método `getInstance()` deve usar lazy initialization

**Perguntas para reflexão:**

1. Qual design pattern deve ser usado para resolver este problema?
2. Qual é o propósito deste pattern?
3. O que é "lazy initialization" e por que é útil neste caso?
4. Como você garantiu que apenas uma instância da classe pode ser criada?
5. Quais problemas poderiam surgir em um ambiente multi-thread e como resolvê-los?

### Template para Implementação

```java
import java.util.ArrayList;
import java.util.List;

public class FilaImpressao {
    // TODO: Implementar atributos
    
    // TODO: Implementar construtor privado
    
    // TODO: Implementar getInstance()
    
    // TODO: Implementar adicionarDocumento(String documento)
    
    // TODO: Implementar removerDocumento()
    
    // TODO: Implementar removerTodosDocumentos()
    
    // TODO: Implementar listarDocumentos()
    
    // TODO: Implementar tamanhoFila()
}
```

### Teste da Implementação

Crie uma classe `TesteFila` para validar sua implementação:

```java
public class TesteFila {
    public static void main(String[] args) {
        // Obter a instância da fila
        FilaImpressao fila1 = FilaImpressao.getInstance();
        FilaImpressao fila2 = FilaImpressao.getInstance();
        
        // Verificar se é a mesma instância
        System.out.println("fila1 == fila2? " + (fila1 == fila2));
        
        // Adicionar documentos
        fila1.adicionarDocumento("Documento1.pdf");
        fila1.adicionarDocumento("Documento2.pdf");
        fila1.adicionarDocumento("Documento3.pdf");
        
        // Verificar se fila2 também tem os documentos
        System.out.println("Tamanho da fila: " + fila2.tamanhoFila());
        System.out.println("Documentos: " + fila2.listarDocumentos());
        
        // Remover um documento
        System.out.println("Removendo: " + fila1.removerDocumento());
        System.out.println("Tamanho da fila: " + fila2.tamanhoFila());
        
        // Remover todos
        fila2.removerTodosDocumentos();
        System.out.println("Tamanho da fila após limpar: " + fila1.tamanhoFila());
    }
}
```



### Desafio Extra

Implemente uma versão **thread-safe** da `FilaImpressao` usando uma das técnicas apresentadas:

- Synchronized method
- Double-checked locking
- Eager initialization

Compare as vantagens e desvantagens de cada abordagem para este caso de uso específico.

---

### 6. Entrega

Voce deve submeter via TEAMS um documento (word/pdf) que contenha :

- O código completo da sua implementação para a classe FilaImpressao
- As cinco respostas para as perguntas de reflexão


## 7. Referências Complementares

### Livros

1. **Gamma, E., Helm, R., Johnson, R., Vlissides, J.** (1994). *Design Patterns: Elements of Reusable Object-Oriented Software*. Addison-Wesley.
   - O livro clássico da Gang of Four, referência fundamental

2. **Freeman, E., Robson, E., Bates, B., Sierra, K.** (2004). *Head First Design Patterns*. O'Reilly Media.
   - Abordagem visual e didática, excelente para iniciantes

3. **Bloch, J.** (2018). *Effective Java* (3rd Edition). Addison-Wesley.
   - Item 3 trata especificamente do Singleton com Enum

4. **Martin, R.C.** (2008). *Clean Code: A Handbook of Agile Software Craftsmanship*. Prentice Hall.
   - Boas práticas gerais de programação

### Artigos e Tutoriais Online

5. **Refactoring.Guru - Singleton**
   - URL: https://refactoring.guru/design-patterns/singleton
   - Tutorial interativo com exemplos em várias linguagens

6. **SourceMaking - Singleton Design Pattern**
   - URL: https://sourcemaking.com/design_patterns/singleton
   - Explicações detalhadas com diagramas UML

7. **Baeldung - Singletons in Java**
   - URL: https://www.baeldung.com/java-singleton
   - Implementações práticas em Java com análises

8. **Oracle Java Documentation**
   - URL: https://docs.oracle.com/javase/tutorial/
   - Documentação oficial da linguagem Java

### Vídeos

9. **Derek Banas - Singleton Design Pattern Tutorial**
   - Canal YouTube: Derek Banas
   - Explicação prática com exemplos de código

10. **Christopher Okhravi - Singleton Pattern**
    - Canal YouTube: Christopher Okhravi
    - Série completa sobre design patterns

### Repositórios GitHub

11. **iluwatar/java-design-patterns**
    - URL: https://github.com/iluwatar/java-design-patterns
    - Implementações de todos os padrões GoF em Java

12. **kamranahmedse/design-patterns-for-humans**
    - URL: https://github.com/kamranahmedse/design-patterns-for-humans
    - Explicações simplificadas de design patterns

### Cursos Online

13. **Coursera - Design Patterns (University of Alberta)**
    - Curso completo sobre padrões de projeto

14. **Pluralsight - Design Patterns Library**
    - Biblioteca de cursos sobre cada padrão individual

15. **Udemy - Java Design Patterns**
    - Vários cursos práticos sobre patterns em Java

### Comunidades e Fóruns

16. **Stack Overflow**
    - Tag: [design-patterns]
    - Discussões práticas e soluções de problemas

17. **Reddit - r/programming**
    - Discussões sobre boas práticas e padrões

---

## Licença

Este material é disponibilizado para fins educacionais.

**Autor:** Prof. Ms. Claudio Souza Nunes  

---

## Contribuições

Encontrou algum erro ou tem sugestões de melhoria? Abra uma **issue** ou envie um **pull request**!

---

**Bons estudos! 🚀**
