# 🔄️ Fluxo de trabalho

Este projeto segue uma estratégia simplificada e eficaz de controle de versões com Git. A estrutura das branches e a construção de Pull Request são organizada para facilitar o desenvolvimento colaborativo e garantir estabilidade no código principal.

## 🌿 Branches principais

| Branch     | Finalidade                                                                 |
|------------|----------------------------------------------------------------------------|
| `main`     | Contém a versão **estável** e pronta para produção do projeto              |
| `develop`  | Integra o desenvolvimento contínuo; base para novas funcionalidades e correções |

## 🌱 Branches auxiliares

Use branches específicas para organizar melhor o desenvolvimento. Elas devem ser criadas a partir da `develop`:

| Padrão de Nome                  | Descrição                                                |
|---------------------------------|----------------------------------------------------------|
| `feature/xxx-nome-da-feature`   | Desenvolvimento de uma nova funcionalidade               |
| `bugfix/xxx-nome-do-bug`        | Correção de bugs identificados                           |
| `refactor/xxx-nome-do-refactor` | Melhorias internas no código sem alterar comportamento   |
| `doc/xxx-nome-da-doc`           | Atualizações ou melhorias na documentação                |
| `test/xxx-nome-do-teste`        | Adição ou modificação de testes                          |
| `spike/xxx-nome-topico`         | Exploração técnica ou estudo de ferramentas/estratégias  |

>[!IMPORTANT]
>Sempre adicione o código da issue antes do nome da branch!

## 🔁 Fluxo de trabalho

1. Assuma uma issue para executar
2. Crie uma branch a partir de `develop`.
3. Faça commits pequenos e descritivos.
4. Ao finalizar a tarefa, abra um **Pull Request** para `develop`, seguindo o [padrão de titulo](#️padrão-para-títulos-de-pull-requests).
5. Após testes e validações, o conteúdo de `develop` pode ser integrado à `main` em um ponto de release estável.

>Não use acentos nem caracteres especiais no nome da branch.

## 🏷️ Padrão para Títulos de Pull Requests

Para mantermos nosso histórico organizado e automatizarmos a criação de notas de versão (release notes), todos os títulos de Pull Requests **devem** seguir o padrão **[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)**.

A estrutura é: `<tipo>(escopo): descrição`

* **`tipo`**: Tipo da alteração que foi executada conforme a [lista de tipos](#descrição-de-tipos-aceitos)
* **`escopo`** (opcional): A área do código que foi alterada (ex: `Controller`, `Auth`, `Header`).
* **`descrição`**: Um resumo claro e curto da mudança, a descrição deve ter um limite de 100 character

### Descrição de tipos aceitos

* **`feat` ✨ (Feature):** Adiciona uma nova funcionalidade visível para o usuário.

* **`fix` 🐞 (Bug Fix):** Corrige um bug ou um comportamento inesperado no código.

* **`docs` 📚 (Documentation):** Apenas mudanças na documentação (ex: README, guias, comentários no código).

* **`style` 🎨 (Styling):** Mudanças na formatação do código que não afetam sua lógica (ex: espaços, ponto e vírgula, linting).

* **`refactor` ♻️ (Refactoring):** Alteração no código que não corrige um bug nem adiciona uma funcionalidade, mas melhora a estrutura ou legibilidade.

* **`test` 🧪 (Tests):** Adiciona ou corrige testes automatizados.

* **`perf` ⚡️ (Performance):** Uma mudança no código que melhora a performance.

* **`ci` ⚙️ (Continuous Integration):** Mudanças nos arquivos e scripts de configuração de CI (ex: GitHub Actions, Travis CI).

* **`build` 📦 (Build System):** Mudanças que afetam o sistema de build ou dependências externas (ex: Webpack, npm, Docker).

* **`chore` 🧹 (Chore):** Outras tarefas de manutenção que não modificam o código fonte ou os testes (ex: atualizar dependências, limpar arquivos)

* **`revert` ⏪ (Revert):** Reverte um commit anterior.
### Exemplos:

- `feat(avaliacoes): Adiciona rota para exclusão de avaliações`
- `fix(auth): Corrige validação de token expirado`
- `refactor(services): Aplica padrão de repositório no AlunoService`
- `docs(contributing): Adiciona guia sobre rituais da equipe`
- `chore(deps): Atualiza dependência do Sequelize para a versão 6.37.3`
- `fix: Corrige erro de digitação na mensagem de boas-vindas`
