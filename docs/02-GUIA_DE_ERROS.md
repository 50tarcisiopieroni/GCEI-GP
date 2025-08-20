# 🛡️ Guia de Boas Práticas para Tratamento de Erros

Bem-vindo(a) ao projeto\! Este documento serve como o guia definitivo para o tratamento de erros em nossa API. Seguir estas diretrizes é fundamental para mantermos um código limpo, seguro, consistente e fácil de dar manutenção.

## 💡 Filosofia Principal

Nossa abordagem é baseada no **Princípio da Responsabilidade Única** e na **Separação de Preocupações**. Em resumo:

1.  🧠 **Serviços** cuidam da lógica de negócio e da *origem* dos erros.
2.  🎬 **Controllers**  orquestram o fluxo e *delegam* os erros.
3.  🚦 **Middleware centralizado** no backend *formata e envia* a resposta de erro final.
4.  🧱 **Error Boundaries** no react um componente que trata erros na arvore de componente, gerando uma UX melhor.

Isso evita código repetido e garante que a resposta de erro da nossa API seja sempre padronizada e no front a experiência do usuário é preservada.

## ⚖️ 1. Erros Operacionais vs. Erros de Programação

A distinção mais importante que fazemos é entre dois tipos de erros:

### ✅ Erros Operacionais (Previsíveis)
São erros que fazem parte do fluxo esperado da aplicação. Eles não são bugs.

  * **Exemplos:** "Email já cadastrado", "Usuário não encontrado", "Token inválido".
  * **Como tratar:** Lançando uma instância de uma de nossas classes de erro específicas (`ValidationError`, `NotFoundError`, etc.), que herdam da classe base `AppError` no backenf e no front é apresentando um showToast, com mensagem específica, ou apresenta o erro direto no input..

### 🐛 Erros de Programação (Inesperados)

São bugs no nosso código. Coisas que não deveriam acontecer.

  * **Exemplos:** `TypeError: Cannot read properties of null`, falha de conexão com o banco.
  * **Como tratar:** Eles são capturados automaticamente pelo nosso middleware, que envia uma resposta genérica para não expor detalhes do sistema e no fronte é capturada pelo erroBoundaries e apresentado uma tela comum para o usuário.


## 📚 2. Nossas Classes de Erro no backend (Express)

Temos uma classe base, `AppError`, e um conjunto de classes específicas que herdam dela. **A regra é: sempre prefira usar a classe de erro mais específica para cada situação.**

### Classe Base: `AppError`

É o molde para todos os nossos erros operacionais. Ela não deve ser usada diretamente, a menos que nenhum dos erros específicos abaixo se aplique.

### Catálogo de Erros Específicos

####  ❌ `ValidationError (400 Bad Request)`

  * **Quando usar:** Quando os dados enviados pelo cliente falham em uma regra de validação.
  * **Característica:** A mensagem de erro é **obrigatória** e deve ser específica.
  * **Exemplo:**
    ```javascript
    if (!cadastradoPor) {
      throw new ValidationError('Docente responsável pela solicitação não informado.');
    }
    ```

#### 🔑 `UnauthorizedError (401 Unauthorized)`

  * **Quando usar:** Quando o usuário tenta executar uma ação que exige autenticação, mas ele não está logado ou seu token é inválido/expirado.
  * **Importante:** Não confunda com `403 Forbidden`, que é para quando o usuário está autenticado, mas não tem permissão para *aquele* recurso.
  * **Exemplo:**
    ```javascript
    if (!docenteAutenticado) {
      throw new UnauthorizedError('É necessário autenticação para acessar este recurso.');
    }
    ```

#### 🚫 `ForbiddenError (403 Forbidden)`

  * **Quando usar:** Quando o usuário está **autenticado** (logado), mas **não possui o nível de permissão** necessário para acessar um recurso ou executar uma ação específica. A identidade do usuário é conhecida, mas o acesso é negado.
  * **Característica:** A mensagem de erro é opcional, mas recomendada para explicar por que o acesso foi negado.
  * **Exemplo:**
    ```javascript
    // Exemplo em um service que protege uma rota de admin
    if (!isHole(ADMIN)) {
      throw new ForbiddenError('Acesso restrito a administradores.');
    }
    ```

#### 🔎 `NotFoundError (404 Not Found)`

  * **Quando usar:** Quando uma busca por um recurso específico (por ID, por exemplo) não encontra nada no banco de dados.
  * **Característica:** Aceita o nome do recurso para criar uma mensagem clara.
  * **Exemplo:**
    ```javascript
    const avaliacao = await findById(id);
    if (!avaliacao) {
      throw new NotFoundError('Avaliação'); // Gera a mensagem "Avaliação não encontrado(a)."
    }
    ```

 #### 💥 `ConflictError (409 Conflict)`

  * **Quando usar:** Quando uma ação viola uma regra de unicidade do banco de dados (ex: tentar cadastrar um aluno ja cadastrado).
  * **Exemplo:**
    ```javascript
    const alunoExistente = await findAluno(aluno);
    if (alunoExistente) {
      throw new ConflictError('Aluno já cadastrado.');
    }
    ```


## 🚦 3. O Middleware de Erro Centralizado no backend (`errorHandler`)

Este é o **único lugar** no código que envia uma resposta de erro para o cliente. Ele recebe qualquer erro e decide o que fazer, tratando todas as nossas classes de erro de forma polimórfica.
Veja a implementação desle no arquivo [`erro.handler.js`](./backend/src/middlewares/error.handler.js).

## 🎬 4. O Papel do Controller

Com o `errorHandler` no lugar, o controller fica extremamente simples. Sua única responsabilidade em caso de erro é **capturá-lo e delegá-lo** usando `next(error)`.

```javascript
class ExemploController {
  async criar(req, res, next) {
    try {
      const novoRegistro = await registoService.criar(req.body);
      return res.status(201).json(novoRegistro);
    } catch (error) {
      // Apenas delega QUALQUER erro para o errorHandler.
      return next(error);
    }
  }
}
```

## 🎯 5. Tratamento de Erros no Frontend (React)

Enquanto o backend é responsável por *identificar* e *categorizar* os erros, o frontend é responsável por *interpretá-los* e *apresentá-los* de forma clara e amigável para o usuário.

Nossa estratégia no React se baseia em três pilares: **Consumo da API**, **Apresentação ao Usuário (UX)** e **Contenção de Erros de UI**.

### 📲 A. Consumindo Erros da API

Toda a comunicação com o backend deve ser centralizada em um "service". Ex: um arquivo `services/alunoService.js` que usa o Axios. Este cliente retornatar a resposta da requisição e os erros devem ser tratados na UI.

**Fluxo recomendado:**

1.  Um componente React chama uma função do nosso cliente de API (ex: `alunoService.createAluno(data)`).
2.  O cliente de API faz a requisição dentro de um bloco `try...catch`.
3.  Se a requisição falhar, o bloco `catch` analisa o objeto de erro (`error.response`) para decidir o que fazer.

```javascript
import React, { useState, useEffect } from 'react';
import alunoService from '../services/alunoService'; // 1. Importa o serviço

const ListaDeAlunos = () => {
  const [alunos, setAlunos] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchAlunos = async () => {
      try {
        setLoading(true);
        // 2. Usa a função do serviço de forma limpa e direta
        const data = await alunoService.getAll();
        setAlunos(data);
      } catch (err) {
        // O tratamento de erro viria aqui, conforme o guia de erros
        showToast('error', 'Algo deu errado!', 'Algo deu errado em nossos servidores. Por favor, tente novamente mais tarde.')
        console.error(err);
      } finally {
        setLoading(false);
      }
    };

    fetchAlunos();
  }, []);

  if (loading) return <p>Carregando...</p>;
  if (error) return <p>{error}</p>;

  return (
    <div>
      <h1>Lista de Alunos</h1>
      <ul>
        {alunos.map((aluno) => (
          <li key={aluno.id}>{aluno.nome}</li>
        ))}
      </ul>
    </div>
  );
};

export default ListaDeAlunos;
 ```

**Padrão de Tratamento por Status Code:**

* **`400 (Bad Request / ValidationError)`**: O erro contém mensagens específicas de validação. Essas mensagens devem ser extraídas e exibidas perto dos campos do formulário correspondentes ou apresentar um toast com uma mensagem explicativa.
* **`401 (Unauthorized)`**: O token do usuário é inválido ou expirou. O sistema deve automaticamente redirecionar o usuário para a tela de login.
* **`404 (Not Found)`**: O recurso solicitado não existe. Exiba um componente ou uma mensagem de "Não Encontrado" para o usuário.
* **`409 (Conflict)`**: Conflito de dados (ex: email já cadastrado). Exiba a mensagem de erro específica vinda da API em um toast.
* **`5xx (Server Error)`**: Erro genérico no servidor. Não mostre detalhes técnicos. Exiba um toas com uma mensagem amigável e genérica, como "Oops! Algo deu errado em nossos servidores. Por favor, tente novamente mais tarde."
* **Erro de Rede**: Se `error.response` não existir, provavelmente é um erro de conexão (sem internet, CORS, etc.). Exiba uma mensagem como "Não foi possível conectar ao servidor. Verifique sua conexão com a internet."

### 🎨 B. Apresentando Erros para o Usuário (Padrões de UX)

Para manter a consistência visual, devemos usar componentes padronizados para exibir feedback.

* **Notificações "Toast"**: Use para erros genéricos ou de rede (ex: "Falha ao salvar a avaliação").
* **Mensagens Inline**: Perfeitas para erros de validação de formulários. Exiba a mensagem de erro diretamente abaixo do campo `input` correspondente.
* **Componentes de Estado de Erro**: Quando uma página inteira ou um componente grande não pode ser carregado (ex: uma lista de alunos), em vez de uma tela em branco, exiba um componente que mostre uma mensagem clara ("Não foi possível carregar os dados") e, se possível, um botão "Tentar Novamente".

### 💥 C. Lidando com Erros de Renderização (Error Boundaries)

E se o erro acontecer no próprio código JavaScript do React, causando uma quebra na renderização? Para isso, usamos **Error Boundaries**.

Um Error Boundary é um componente React que captura erros de JavaScript em qualquer lugar de sua árvore de componentes filhos, registra esses erros e exibe uma UI de fallback em vez da árvore de componentes que quebrou.

## 🥇 Regras de Ouro (Resumo)

1.  **SEMPRE** use `try...catch` nos seus controllers.
2.  Dentro do `catch`, **SEMPRE** use `return next(error);`.
3.  Para erros de negócio, **SEMPRE** use a classe de erro específica mais apropriada (`ValidationError`, `NotFoundError`, etc.).
4.  **NUNCA** formate uma resposta de erro dentro de um controller.
5.  **TODAS** as respostas da API, de sucesso ou erro, devem retornar um corpo em formato JSON.
6. Use mensagens claras e amigáveis no toast.
7. Nunca exiba mensagens de erro com código fonte para o usuário.