Segue o **material completo em Markdown** com todos os passos para criar um projeto Spring Boot com banco **H2**, incluindo comandos `curl` para realizar as operações de **POST**, **PUT (update)** e **DELETE**, além da orientação para trocar para o MySQL.

---

# 📘 Projeto Spring Boot com banco H2 (JPA)

## ✅ Passo a passo para criar o projeto

### 1. Criar projeto Spring no VS Code

---

### 2. Dependências necessárias (`pom.xml`)

```xml
<!-- Spring Data JPA -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
    <version>3.5.0</version>
</dependency>

<!-- Spring Web -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>3.5.0</version>
</dependency>

<!-- H2 Database -->
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>2.3.232</version>
    <scope>test</scope>
</dependency>
````

---

### 3. Configuração do `application.properties`

```properties
spring.application.name=demo
spring.datasource.url=jdbc:h2:mem:meubanco
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=true
spring.h2.console.path=/h2
spring.jpa.open-in-view=false
spring.jpa.hibernate.ddl-auto=update
```

---

### 4. Criar estrutura de diretórios e classes

#### 🗂 Pacotes:

* `com.example.demo.model`
* `com.example.demo.repository`
* `com.example.demo.controller`

---

#### ✅ Model - `Produto.java`

```java
package com.example.demo.model;

import jakarta.persistence.*;

@Entity
public class Produto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nome;
    private Double preco;

    // Getters e Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }

    public Double getPreco() { return preco; }
    public void setPreco(Double preco) { this.preco = preco; }
}
```

---

#### ✅ Repository - `ProdutoRepository.java`

```java
package com.example.demo.repository;

import com.example.demo.model.Produto;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProdutoRepository extends JpaRepository<Produto, Long> {
}
```

---

#### ✅ Controller - `ProdutoController.java`

```java
package com.example.demo.controller;

import com.example.demo.model.Produto;
import com.example.demo.repository.ProdutoRepository;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/produtos")
public class ProdutoController {

    private final ProdutoRepository repository;

    public ProdutoController(ProdutoRepository repository) {
        this.repository = repository;
    }

    @GetMapping
    public List<Produto> listar() {
        return repository.findAll();
    }

    @PostMapping
    public Produto criar(@RequestBody Produto produto) {
        return repository.save(produto);
    }

    @PutMapping("/{id}")
    public Produto atualizar(@PathVariable Long id, @RequestBody Produto produto) {
        produto.setId(id);
        return repository.save(produto);
    }

    @DeleteMapping("/{id}")
    public void remover(@PathVariable Long id) {
        repository.deleteById(id);
    }
}
```

---

## ▶️ Rodar o Projeto

Certifique-se de que o Maven esteja instalado corretamente e execute o projeto:

 no Windows:

```cmd
mvnw.cmd spring-boot:run
```

---

## 🌐 Testes via `curl` no terminal (CMD/PowerShell)

### 🔄 Inserir (POST)

```cmd
curl -X POST http://localhost:8080/produtos -H "Content-Type: application/json" -d "{\"nome\":\"Café\",\"preco\":14.5}"
```

### 🛠 Atualizar (PUT)

```cmd
curl -X PUT http://localhost:8080/produtos/1 -H "Content-Type: application/json" -d "{\"nome\":\"Café Premium\",\"preco\":18.0}"
```

### ❌ Remover (DELETE)

```cmd
curl -X DELETE http://localhost:8080/produtos/1
```

---

## 🔍 Acessar no navegador

* H2 Console: [http://localhost:8080/h2](http://localhost:8080/h2)
* Endpoint REST: [http://localhost:8080/produtos](http://localhost:8080/produtos)

---

## 📚 Atividade Prática

Crie um CRUD para a entidade `Cliente`, com os campos:

* `id`
* `nome`
* `email`
* `telefone`

Siga os mesmos passos:

1. Criar a classe `Cliente`
2. Criar o `ClienteRepository`
3. Criar o `ClienteController`
4. Testar com comandos `curl` semelhantes aos usados com `Produto`.

---

## 🔄 Trocando o banco H2 por MySQL

Se quiser usar o MySQL:

### 1. Alterar dependência no `pom.xml`:

```xml
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>8.3.0</version>
</dependency>
```

### 2. Atualizar `application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/meubanco
spring.datasource.username=root
spring.datasource.password=sua_senha
spring.jpa.hibernate.ddl-auto=update
spring.jpa.open-in-view=false
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

> Certifique-se de que o banco `meubanco` existe no MySQL.

---

## ✅ Conclusão

Este guia mostra como construir um CRUD completo com Spring Boot, JPA e banco H2, testando com `curl`, e preparando a base para usar MySQL. Ideal para projetos didáticos, técnicos e aplicações reais.

---

