## 📜 Padrão de Nomenclatura para Arquivos e Funções

Para garantir a consistência, legibilidade e manutenibilidade do nosso código, todo novo desenvolvimento e refatoração deve seguir os padrões de nomenclatura descritos abaixo.

### 📄 1. Nomenclatura de Arquivos

Todos os arquivos de `service` e `repository` devem seguir o padrão **`camelCase.js`**. O nome deve refletir a entidade que ele gerencia.

* **Exemplos:**
    * `productService.js`
    * `userRepository.js`
    * `orderEvaluationService.js`

### ⚙️ 2. Nomenclatura de Funções

A convenção de nomes para funções deve refletir a **responsabilidade específica de cada camada** da arquitetura.

#### 💡 Princípio Fundamental

* **Camada de Repository 🗄️:** Fala a língua do **banco de dados**. Os nomes são literais e descrevem operações de dados (CRUD).
* **Camada de Service ✨:** Fala a língua do **negócio**. Os nomes descrevem casos de uso e processos da aplicação.


#### 🗄️ Camada de Repository

**Responsabilidade:** Acesso e manipulação de dados no banco.

**Padrão:** `**verboDeAcesso + Entidade + [ByCondição]**`

* **Verbos Comuns:** `find`, `create`, `update`, `delete`, `count`.

* **Exemplos Padrão:**
    * `findAvaliacaoById(id)`: Busca um usuário pela chave primária.
    * `findAllAlunos(options)`: Busca uma lista de produtos, podendo aceitar filtros.
    * `createDocente(orderData)`: Cria um novo registro de pedido.
    * `updateDocentePassword(userId, newPasswordHash)`: Atualiza um campo específico de um usuário.
    * `deleteAvaliacaoById(id)`: Remove uma avaliação.

* **Para Funções que Incluem Associações (Eager Loading) 🔗:**
    Quando uma função retorna uma entidade junto com suas associações (usando `include`), devemos deixar isso explícito no nome.

    **Padrão Adicional:** `**...With[EntidadeAssociada]**`

    * **Exemplo Principal:** `findAvaliacaoByIdWithFormularios(id)`
        * **Significado:** Busca a entidade `Avaliacao` e, na mesma consulta, inclui a lista de `Formularios` associados a ela.
    * **Outro Exemplo:** `findProductByIdWithReviews(id)`
        * **Significado:** Busca o produto e inclui suas avaliações (`reviews`).


#### ✨ Camada de Service

**Responsabilidade:** Orquestrar a lógica e as regras de negócio.

**Padrão:** `**verboDeNegócio + Entidade + [Contexto]**`

* **Verbos Comuns:** `get`, `create`, `update` (representando um processo), e verbos mais específicos como `register`, `activate`, `deactivate`, `process`, `calculate`, `send`.

* **Exemplos:**
    * `registerNewUser(userData)`: Orquestra o processo de registro de um novo usuário (valida dados, criptografa senha, chama `userRepository.createUser`).
    * `getProductDetailsForCustomer(productId)`: Busca os dados de um produto e talvez os enriqueça com outras informações antes de retorná-los (chama `productRepository.findProductByIdWithReviews`).
    * `deactivateProduct(productId, adminId)`: Aplica a regra de negócio para desativar um produto (verifica permissões, registra o log da ação, chama `productRepository.updateProductById`).
    * `submitEvaluation(evaluationData)`: Processa o envio de uma nova avaliação (valida os dados, cria a avaliação e os formulários associados através de seus respectivos repositórios).

### 📊 Resumo Prático e Tabela Comparativa

| Operação de Negócio | Função no **`✨Service`**  | Função(s) no **`🗄️Repository`**  que seriam chamadas |
| :--- | :--- | :--- |
| Cadastrar um novo produto | `createNewProduct(productData)` | `findProductBySku(sku)`, `createProduct(productData)` |
| Buscar detalhes da avaliação | `getEvaluationDetails(evaluationId)` | `findAvaliacaoByIdWithFormularios(evaluationId)` |
| Publicar um artigo | `publishArticle(articleId, authorId)` | `findArticleById(articleId)`, `updateArticleById(articleId, ...)` |
| Cancelar um pedido | `cancelOrder(orderId, userId)` | `findOrderById(orderId)`, `updateOrderById(orderId, ...)` |

Aderir a estes padrões tornará nossa base de código mais previsível e fácil de navegar, acelerando o desenvolvimento e facilitando a manutenção. ✅
