# Code Review Ana Júlia (sisAluguel)

Carlos Henrique Neimar Areas Ferreira

Link do repositorio: https://github.com/anajuliateixeiracandido/sisAluguel/tree/main

---

### 1. **Arquitetura**
   - Este projeto segue principalmente uma arquitetura **MVC (Model-View-Controller)** com os controladores (`Controller`) servindo para gerenciar as interações entre o sistema e os usuários, enquanto as configurações adicionais estão centralizadas na classe `WebConfig`.
   - O projeto contém:
     - **Controllers**: Classes que lidam com as requisições HTTP e organizam o fluxo de dados entre a camada de aplicação e o usuário.
     - **Configurações**: A classe `WebConfig` centraliza algumas configurações globais (e.g., CORS), mas outras configurações específicas (como segurança ou integração de terceiros) não estão visíveis, o que pode sugerir que a estrutura ainda está básica ou em desenvolvimento.
     - **Entidades e Lógica**: A lógica de negócios parece estar espalhada dentro dos controladores, sugerindo um acoplamento direto entre as camadas de controle e negócio.

### 2. **Sugestões de Melhorias Arquiteturais**
   - **Separação em Camadas**: Adotar uma abordagem em **camadas** pode melhorar a modularidade, permitindo que a aplicação evolua e se mantenha com menos esforço. Aqui está uma recomendação de estrutura:
     - **Camada de Serviço (Service Layer)**:
       - Mover a lógica de negócios para uma camada de serviços dedicada, com classes de serviço específicas (e.g., `AluguelService`, `ContratoService`). Essa camada ficaria responsável por toda a lógica relacionada a regras de negócio, validações complexas e processos de transformação de dados.
       - Benefício: Os controladores ficam responsáveis apenas por receber requisições e responder, enquanto a camada de serviço centraliza a lógica de negócio.
     - **Camada de Persistência (Repository Layer)**:
       - Introduzir repositórios que encapsulem as operações com o banco de dados. Isso adiciona uma abstração útil entre os dados e o restante da aplicação, facilitando a manutenção e testes de persistência.
       - Benefício: Um padrão de repositório modulariza ainda mais o código, facilita a implementação de testes e permite mais flexibilidade caso o banco de dados mude.
   - **DTOs (Data Transfer Objects)**:
     - Considerar a criação de objetos de transferência de dados (DTOs) para separar as entidades do banco de dados das requisições e respostas da API.
     - Benefício: Os DTOs adicionam uma camada de segurança e encapsulamento, evitando a exposição direta de informações e permitindo o controle dos dados que entram e saem da API.
   - **Implementação de Exceções Personalizadas**:
     - Criar uma hierarquia de exceções personalizadas (e.g., `AluguelNotFoundException`, `InvalidContratoException`) para tratar casos específicos com mensagens de erro adequadas.
     - Benefício: Melhora a clareza para os usuários e facilita a manutenção de códigos de resposta HTTP específicos por erro.

### 3. **Considerações sobre a Configuração**
   - A classe `WebConfig` é útil para configurações globais, mas à medida que o projeto cresce, dividir as configurações em classes especializadas pode evitar a sobrecarga e manter o código organizado:
     - **Configuração de Segurança (SecurityConfig)**: se houver uma implementação de autenticação ou autorização, considere incluir uma classe específica para isso.
     - **Configuração de CORS (CorsConfig)**: configurar os domínios permitidos e as regras de CORS pode ser isolado, caso seja necessário manipular múltiplos contextos.
   - **Padrão de Configuração Baseada em Propriedades**:
     - Utilizar o `application.properties` ou `application.yml` para configurações de URLs, parâmetros de segurança, chaves de autenticação, etc. Isso permite ajustes rápidos sem modificação direta no código.

### 4. **Implementação de Boas Práticas com Spring Boot**
   - **Injeção de Dependência**:
     - A injeção de dependência no Sp ring permite que as classes dependam de interfaces em vez de implementações concretas, seguindo o princípio da inversão de dependência.
     - Exemplo: `@Autowired` em uma interface `AluguelRepository` ou `AluguelService`, permitindo flexibilidade e facilidade para testes.
   - **Anotações de Mapeamento Consistentes**:
     - O uso de `@GetMapping`, `@PostMapping`, etc., nas rotas é adequado, mas a padronização das respostas (e.g., `@ResponseStatus(HttpStatus.CREATED)`) facilita o entendimento para quem consome a API e melhora a consistência nas respostas HTTP.

### 5. **Possíveis Melhorias em Termos de Escalabilidade**
   - **Divisão de Módulos**:
     - Caso o sistema cresça, dividi-lo em módulos distintos (como um módulo de aluguel, um módulo de clientes, etc.) ajudará na escalabilidade e isolará funcionalidades de forma mais eficiente.
   - **Consideração de Microsserviços**:
     - Caso o sistema cresça significativamente, considerar uma arquitetura de microsserviços com módulos independentes para cada funcionalidade principal pode ser vantajoso. Isso permitiria uma gestão de dados e lógica de forma isolada e escalável.
