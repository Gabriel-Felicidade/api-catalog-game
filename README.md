# API Catálogo de Jogos

Este projeto implementa uma **API de Catálogo de Jogos** usando o **Quarkus**, o *Supersonic Subatomic Java Framework*. Ele fornece operações RESTful completas (CRUD) para as entidades principais: **Jogo**, **Desenvolvedora** e **Gênero**.

A API utiliza as seguintes tecnologias do ecossistema Quarkus:
* **Quarkus REST (formerly RESTEasy Reactive):** Para serviços web de alta performance.
* **Hibernate ORM com Panache:** Para modelagem e persistência de dados, simplificando o uso dos padrões Active Record e Repository.
* **Jakarta Validation (Hibernate Validator):** Para validação de entidades.
* **H2 Database (em memória):** Para persistência de dados em desenvolvimento e testes.
* **SmallRye Fault Tolerance e Caffeine Cache:** Para controle de idempotência e Rate Limiting.
* **SmallRye OpenAPI/Swagger UI:** Para documentação interativa no endpoint `/q/swagger-ui`.

Se você deseja saber mais sobre o Quarkus, acesse o site oficial:  
[https://quarkus.io/](https://quarkus.io/)

---

## Executando a aplicação em modo de desenvolvimento

Você pode executar a aplicação em modo de desenvolvimento, o que permite **live coding** (recarga a quente), utilizando o comando:

`./mvnw quarkus:dev`

**Nota**: O Quarkus agora conta com uma **Dev UI**, acessível apenas no modo de desenvolvimento, no endereço:  
`http://localhost:8080/q/dev/`

---

## Empacotando e executando a aplicação

A aplicação pode ser empacotada com o comando:

`./mvnw package`

Esse processo irá gerar o arquivo `quarkus-run.jar` dentro do diretório `target/quarkus-app/`.  
Note que esse **não é um über-jar**, pois as dependências são copiadas separadamente para o diretório `target/quarkus-app/lib/`.

Você pode executar a aplicação empacotada com:

`java -jar target/quarkus-app/quarkus-run.jar`

### Criando um Executável Nativo

Para obter o máximo desempenho, você pode criar um executável nativo usando o GraalVM:

`./mvnw package -Dnative`

Se você **não tiver o GraalVM instalado**, é possível realizar a build nativa dentro de um container Docker (requer Docker):

`./mvnw package -Dnative -Dquarkus.native.container-build=true`

Após a build, você poderá executar o binário gerado diretamente (o nome do arquivo reflete o `artifactId` do `pom.xml`):

`./target/API-CaoAmigo-1.0.0-SNAPSHOT-runner`

Para mais informações sobre como construir executáveis nativos, acesse:  
[https://quarkus.io/guides/maven-tooling](https://quarkus.io/guides/maven-tooling)

---

## Endpoints Principais (API Catálogo de Jogos)

A API expõe endpoints REST sob o caminho base `/v1/`.

| Entidade | GET (Todos) | GET (ID) | POST (Criar) | PUT (Atualizar) | DELETE (Deletar) | GET (Pesquisa) |
|:---|:---|:---|:---|:---|:---|:---|
| **Jogo** | `/v1/jogos` | `/v1/jogos/{id}` | `/v1/jogos` | `/v1/jogos/{id}` | `/v1/jogos/{id}` | `/v1/jogos/search?q=...` |
| **Desenvolvedora** | `/v1/desenvolvedoras` | `/v1/desenvolvedoras/{id}` | `/v1/desenvolvedoras` | `/v1/desenvolvedoras/{id}` | `/v1/desenvolvedoras/{id}` | `/v1/desenvolvedoras/search?q=...` |
| **Gênero** | `/v1/generos` | `/v1/generos/{id}` | `/v1/generos` | `/v1/generos/{id}` | `/v1/generos/{id}` | `/v1/generos/search?q=...` |

### Funcionalidades Adicionais

* **Documentação OpenAPI (Swagger UI):** A documentação interativa está disponível em `http://localhost:8080/q/swagger-ui`.
* **Idempotência e Transações:** As operações `POST`, `PUT` e `DELETE` nos recursos exigem o cabeçalho `X-Idempotency-Key` e são transacionais.
* **Rate Limiting:** A API implementa um filtro de Rate Limiting que limita a **5 requisições por minuto** (por IP) nas rotas `/v1/`.
* **Inicialização de Dados:** O banco de dados H2 é populado automaticamente ao iniciar com o script `import.sql`.