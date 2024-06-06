# Injeção de Dependências com Spring: Boas Práticas e Exemplos

<img src="injection.png" alt="Injection" width="20%"/>

A injeção de dependências é um dos pilares do desenvolvimento de software moderno, especialmente em frameworks robustos como o Spring. Neste artigo, exploraremos o conceito de injeção de dependências, como ele funciona no Spring, e as melhores práticas para utilizá-lo de maneira eficaz. 

## O que são Dependências e Injeção de Dependências

Dependências, em termos de programação, referem-se aos objetos (ou outras classes) que uma classe precisa para funcionar corretamente. Por exemplo, a classe `PedidoService` pode depender de métodos da classe `PedidoRepository` para acessar dados de pedidos.

<img src="sample-chart.jpeg" alt="Dependency Injection Chart" width="50%"/>

A injeção de dependências (DI - Dependency Injection) é um padrão de projeto que permite que a criação de dependências seja delegada a um contêiner ou framework. Em vez de a própria classe criar suas dependências, elas são "injetadas" pelo framework, promovendo um design mais modular e testável.

### Benefícios da Injeção de Dependências:
- **Desacoplamento**: Facilita a substituição de implementações.
- **Testabilidade**: Simplifica a criação de testes unitários.
- **Manutenção**: Facilita a manutenção e evolução do código.

## Como elas funcionam no contexto do Spring

<img src="chart-sample-2.png" alt="Dependency Injection Chart 2" width="50%"/>

O Spring é um dos frameworks Java mais populares para a implementação de DI, utilizando um contêiner de inversão de controle (IoC) para gerenciar as dependências dos beans.

### Exemplificando com Código

Para ilustrar, consideremos o seguinte exemplo simples de injeção de dependências usando Spring:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class PedidoService { //classe dependente

    private final PedidoRepository pedidoRepository;

    @Autowired //construtor
    public PedidoService(PedidoRepository pedidoRepository) { //classe que possui as dependências
        this.pedidoRepository = pedidoRepository;
    }

    public void processarPedido(Pedido pedido) {
        pedidoRepository.salvar(pedido);
    }
}

import org.springframework.stereotype.Repository;

@Repository
public class PedidoRepository {

    public void salvar(Pedido pedido) {
        // Lógica para salvar o pedido no banco de dados
    }
}
```

Neste exemplo:
- `PedidoService` depende de `PedidoRepository`.
- A dependência é injetada pelo Spring através do construtor, marcado com `@Autowired`.

## O que são Boas Práticas

Boas práticas são métodos e técnicas reconhecidos na indústria que ajudam a garantir a qualidade, manutenção e eficiência do código. No contexto da injeção de dependências com Spring, elas ajudam a construir aplicações robustas e escaláveis.

Algumas das principais Boas Práticas em Java Spring são:
- **Injeção pelo Construtor**: Preferível para garantir a imutabilidade.
- **Uso de Interfaces**: Facilita o desacoplamento e substituição de implementações.
- **Configuração por Anotações**: Simplifica a configuração do Spring.
- **Uso de Perfis**: Permite diferentes configurações para diferentes ambientes.

## Aplicando as Boas Práticas para Injeção de Dependências no Spring

### Injeção pelo Construtor

Utilizar a injeção pelo construtor é uma boa prática porque:
- **Imutabilidade**: Evita que as dependências sejam alteradas após a construção do objeto.
- **Obrigatoriedade**: Garante que todas as dependências necessárias sejam fornecidas.

#### Exemplo:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ClienteService { //classe dependente

    private final ClienteRepository clienteRepository;

    @Autowired //construtor
    public ClienteService(ClienteRepository clienteRepository) { //classe que contém as dependências
        this.clienteRepository = clienteRepository;
    }

    public Cliente obterCliente(Long id) {
        return clienteRepository.findById(id).orElse(null);
    }
}
```

### Uso de Interfaces

O uso de interfaces facilita a substituição de implementações, promovendo o desacoplamento.

#### Exemplo:

```java
import org.springframework.stereotype.Repository;

public interface ClienteRepository { //interface
    Cliente findById(Long id);
}

@Repository
public class ClienteRepositoryImpl implements ClienteRepository {

    @Override
    public Cliente findById(Long id) {
        // Implementação para encontrar cliente
        return new Cliente();
    }
}
```

### Configuração por Anotações

Anotações como `@Service`, `@Repository`, e `@Controller` ajudam a definir beans de forma clara e concisa.

#### Exemplo:

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller //anotação
@RequestMapping("/clientes")
public class ClienteController {

    private final ClienteService clienteService;

    @Autowired
    public ClienteController(ClienteService clienteService) {
        this.clienteService = clienteService;
    }

    @GetMapping
    public String listarClientes(Model model) {
        model.addAttribute("clientes", clienteService.listarTodos());
        return "clientes";
    }
}
```

### Uso de Perfis

O Spring permite configurar diferentes perfis para diferentes ambientes (dev, test, prod), facilitando a adaptação da aplicação conforme o ambiente.

#### Exemplo:

```java
import org.springframework.context.annotation.Profile;
import org.springframework.stereotype.Service;

@Service
@Profile("dev") //perfil para o ambiente dev
public class DevEmailService implements EmailService {
    // Implementação específica para ambiente de desenvolvimento
}

@Service
@Profile("prod") //perfil para o ambiente prod
public class ProdEmailService implements EmailService {
    // Implementação específica para ambiente de produção
}
```

## Overview e Considerações finais

A injeção de dependências é uma técnica fundamental para a construção de aplicações Java robustas e escaláveis, especialmente quando utilizada com o Spring Framework. Neste artigo, exploramos o funcionamento dessa técnica, destacando boas práticas essenciais que garantem um código mais limpo, desacoplado e testável. Adotar a injeção pelo construtor, utilizar interfaces, configurar beans por anotações e aplicar perfis apropriados são estratégias que elevam a qualidade do desenvolvimento e facilitam a manutenção a longo prazo. Dominar essas práticas não só aprimora a eficiência do seu código, mas também contribui para a criação de sistemas mais resilientes e adaptáveis às mudanças. Esses métodos ajudam a criar aplicações mais modulares, testáveis e fáceis de se manter.

---

O conteúdo deste artigo foi gerado pelo ChatGPT utilizando engenharia de prompts. Seu planejamento, estruturação, diagramação e revisões, por sua vez, foram feitos totalmente por mãos humanas.

Gostou do artigo? Acompanhe meu trabalho no [LinkedIn](https://www.linkedin.com/in/gui-hcastro/) e [GitHub](https://github.com/GuihCastro)!

    #Java #SpringFramework #DependencyInjection #DesenvolvimentoAgil
