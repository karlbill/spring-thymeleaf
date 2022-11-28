# Spring com Thymeleaf
Estudo de Spring Boot com Thymeleaf (Template Engine padrão do Spring)

Link: https://spring.io/projects

## Ferramentas do projeto 
1. Java JDK 8
2. Gerenciador de dependências/deploy: Gradle [link](https://gradle.org) 
3. IDE: VS Code
4. Banco de dados: MySQL

## Geração do projeto
[Link do Spring IO - Spring Initializer](https://www.start.spring.io)

Obs: foi utilizada a facilidade do VS Code de acessar o Spring Initializer diretamente pela IDE e configurar todo o projeto com as dependências necessárias: Thymeleaf, Spring Data JPA, Spring Web, DevTools

## Criação de uma página Hello World
1. Criação da camada Controller que conterá um Controller HelloWorldController.java:
```
package br.com.karlbill.webapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/")
public class HelloWorldController {
    
    @GetMapping
    public ModelAndView index(Model model){ // Para o Thymeleaf, sempre retorne um ModelAndView (HTML)
        ModelAndView mv = new ModelAndView("/hello");
        
        return mv;
    }
}
```

2. Criação da página hello.html em resources/templates:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello</title>
</head>
<body>
    <h1>Hello World!</h1>
</body>
</html>
```

## Executando a aplicação:
Ao executar o programa pela primeira vez, será apresentada a seguinte mensagem de erro:

![image](https://user-images.githubusercontent.com/39681960/204164115-b57db757-78c0-4fa3-9b2d-d049183998eb.png)

Para evitar o erro (não tratá-lo), pode-se acrescentar o seguinte parâmetro à Anotação SpringBootApplication do método principal:
```
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class })
```

Obs: esse erro ocorre porque nós estamos utilizando o Spring Data JPA mas ainda não configuramos nenhum banco de dados para nosso projeto. Então, com a anotação acima, apenas dissemos para o compilador desconsiderar a dependência do Spring Data JPA, por enquanto.

## Configurando o Banco de Dados
Primeiramente, adicionamos o Driver do MySQL, versão 5.1.13:
```
implementation group: 'mysql', name: 'mysql-connector-java', version: '5.1.13'
```

Em seguida, adicionamos as propriedades de conexão com o Banco de Dados:
```
spring.jpa.hibernate.ddl-auto=create
spring.datasource.url=jdbc:mysql://localhost:[port]/[database_name]
spring.datasource.username=[your_user]
spring.datasource.password=[your_password]
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

## Outra forma de renderização: retornar String
Podemos renderizar um arquivo .html, retornando o nome do arquivo como uma String na Action de um Controller. Por exemplo:
```
@GetMapping("/new")
public String newPath(Model model){
  return "new";
}
```
> Assim, o arquivo new.html será buscado na pasta resources > templates.

## Renderização de uma lista de nomes
1. Adicionamos o Thymeleaf na tag <html> do arquivo onde iremos utilizá-lo:
```
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```
2. Criamos uma lista através de tags HTML, com a diferença da passagem de objetos através dos recursos do Thymeleaf:
```
<body>
    <h1>New Page!</h1>
    
    <ul>
        <li th:each="name: ${list}">
            <p th:text="${name}"></p>
        </li>
    </ul>
</body>
```
> Veja que utilizamos Interpolação para que o objeto passado no Controller seja recebido via Thymeleaf para renderização.

3. A Action do Controller cria a lista que será passada ao Thymeleaf, através de um método addAttribute do objeto Model:
```
@GetMapping("/new")
    public String newPath(Model model){
        List<String> list = new ArrayList<>();
        
        list.add("Carlos");
        list.add("Guilherme");
        list.add("Marques");
        list.add("Borges");
        list.add("Abdalla");
        
        model.addAttribute("list", list);
        
        return "new";
    }
```
> Veja que o método addAttribute trabalha com uma Estrutura de Dados do tipo Map (chave, valor).

## Adicionando arquivos estáticos
Como exemplo, faremos a adição do framework **Bootstrap** à aplicação:
1. [Bootstrap download](https://getbootstrap.com/docs/4.1/getting-started/download/)
> Ou baixar diretamente pelo terminal do Linux: 
```wget https://github.com/twbs/bootstrap/releases/download/v4.1.3/bootstrap-4.1.3-dist.zip ```
2. Criamos uma subpasta static/css para receber o arquivo bootstrap.mim.css
3. Criamos uma subpasta static/js para receber o arquivo bootstrap.min.js
4. Também é necessário incluir a biblioteca do JQuery na subpasta /static/js: ``` wget https://code.jquery.com/jquery-3.6.1.min.js ```

Com todas as bibliotecas incluídas no projeto, vamos configurar os arquivos HTML para receberem os arquivos estáticos:
1. Adicionando a estilização do Bootstrap ao arquivo new.html:
```
<link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
```
2. Adicionando os scripts Javascript (antes do fechamento da tag </body>:
```
<script th:src="@{/js/jquery-3.6.1.min.js}"></script>
<script th:src="@{/js/bootstrap.min.js}"></script>
```

## Trabalhando com Layouts no Thymeleaf
Para utilizarmos layouts no Thymeleaf, um dos mais comumente utilizados é o do projeto Ultraq:
```
<html lang="en" xmlns:th="http://www.thymeleaf.org"
    xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout>
```
































