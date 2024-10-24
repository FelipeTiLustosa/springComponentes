## Spring Componentes
No **Spring**, os **componentes** são classes que o framework **gerencia** para você. Esses componentes podem ser serviços, controladores, repositórios, entre outros, e são identificados por meio de **anotações** que indicam ao Spring para instanciar, gerenciar e injetá-los automaticamente onde for necessário.

### Analogia clara: "Oficina e Ferramentas"

Imagine que você tem uma **oficina** e várias **ferramentas** para realizar trabalhos diferentes. Essas ferramentas podem ser chaves de fenda, martelos, alicates, etc. Em vez de você ter que buscar cada ferramenta e mantê-las organizadas, você tem um **gerente de ferramentas** (o Spring) que cuida de tudo isso para você.

- O gerente organiza as ferramentas.
- Quando você precisar de uma ferramenta (componente), o gerente entrega a você automaticamente.
- Você só precisa pedir, e ele já sabe qual ferramenta entregar e quando.

Na oficina, as ferramentas têm diferentes funções:
- **Ferramenta para apertar parafusos**: (No Spring, isso seria um **Service**).
- **Ferramenta para segurar peças**: (No Spring, isso poderia ser um **Repository**).
- **Ferramenta para cortar materiais**: (No Spring, isso poderia ser um **Controller**).

O **Spring** gerencia essas ferramentas e entrega a ferramenta certa quando você precisar.

---

Agora, vamos trazer isso para o mundo do Spring com um exemplo mais técnico.

### Tipos de componentes no Spring

Existem vários tipos de componentes no Spring, que são classes que o Spring vai gerenciar automaticamente para você. Vou listar os mais comuns:

1. **@Component**: É a anotação genérica. Ela transforma a classe em um componente gerenciado pelo Spring, e o Spring cria uma instância dela e cuida do ciclo de vida.
2. **@Service**: É um tipo específico de componente que indica que a classe tem **lógica de negócios**. 
3. **@Repository**: Indica que a classe é responsável pela interação com o banco de dados. Ela encapsula a lógica de acesso aos dados.
4. **@Controller**: Indica que a classe é um controlador, responsável por lidar com requisições HTTP (no caso de uma aplicação web).

### Exemplo Prático

Aqui está um exemplo prático com três componentes diferentes:

#### 1. **Service** - Responsável pela lógica de negócios:
```java
import org.springframework.stereotype.Service;

@Service
public class ProdutoService {

    public double calcularDesconto(double preco, double porcentagemDesconto) {
        return preco - (preco * (porcentagemDesconto / 100));
    }
}
```

- Aqui, a classe `ProdutoService` é marcada com `@Service`. Isso indica ao Spring que essa classe faz parte da lógica de negócios e será gerenciada por ele.

#### 2. **Repository** - Responsável por acessar o banco de dados:
```java
import org.springframework.stereotype.Repository;

@Repository
public class ProdutoRepository {

    public void salvarProduto(Produto produto) {
        // Lógica para salvar produto no banco de dados
    }
}
```

- A classe `ProdutoRepository` tem a anotação `@Repository`, indicando que ela interage com o banco de dados.

#### 3. **Controller** - Lida com requisições HTTP (no caso de aplicações web):
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProdutoController {

    private final ProdutoService produtoService;

    // O Spring injeta o ProdutoService automaticamente aqui
    public ProdutoController(ProdutoService produtoService) {
        this.produtoService = produtoService;
    }

    @GetMapping("/produto/desconto")
    public String calcularDesconto() {
        double preco = 200.0;
        double desconto = produtoService.calcularDesconto(preco, 10.0);
        return "Preço com desconto: " + desconto;
    }
}
```

- A classe `ProdutoController` é marcada com `@RestController`, que indica ao Spring que ela lida com as requisições HTTP.
- Observe que o `ProdutoService` é **injetado automaticamente** no construtor do controlador pelo Spring. Isso é a **injeção de dependência** em ação: você não precisa instanciar o `ProdutoService` manualmente.

### Funcionamento do Spring

O que acontece aqui é o seguinte:

1. O Spring vê que você anotou as classes com `@Service`, `@Repository` e `@RestController`.
2. Ele cria instâncias dessas classes e gerencia o ciclo de vida delas. Isso significa que ele vai garantir que essas instâncias estejam disponíveis quando você precisar.
3. Quando o controlador (`ProdutoController`) precisa da lógica de negócios (`ProdutoService`), o Spring automaticamente fornece a instância correta, sem você precisar criá-la manualmente.

### Como pedir as ferramentas (Injeção de Dependência)

No nosso exemplo, o Spring age como o **gerente de ferramentas** (o "gerente" é o **Container Spring**, também chamado de **Inversion of Control Container**). Quando o controlador `ProdutoController` precisa de uma ferramenta (como o `ProdutoService`), o Spring injeta essa ferramenta para você.

- Você só precisa pedir a ferramenta, e o Spring sabe qual ferramenta entregar, seja ela uma lógica de negócios (service), uma interação com banco de dados (repository), ou qualquer outro componente.

### Resumo

- **Componentes** são classes que o Spring gerencia automaticamente.
- Você usa anotações como `@Service`, `@Repository` e `@Controller` para informar ao Spring que classes devem ser tratadas como componentes.
- O Spring cuida de instanciar essas classes e gerenciar o ciclo de vida delas, para que você não precise se preocupar com isso.
- Isso permite que o Spring injete automaticamente dependências nas suas classes (como `ProdutoService` sendo injetado em `ProdutoController`).

### Analogia final:

- Na sua **oficina** (projeto Spring), você tem várias **ferramentas** (componentes).
- Você define quais são as ferramentas e o **gerente** (Spring) cuida de entregá-las para você no momento certo, organizando tudo nos bastidores.
