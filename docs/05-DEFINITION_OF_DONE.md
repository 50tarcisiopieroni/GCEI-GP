# ✅ Definition of Done (DoD) -  Nossa Definição de "Pronto"

Este documento é o nosso acordo de equipe sobre o que significa que uma tarefa (Issue) está verdadeiramente **"Pronta"**. Para que um card seja movido para a coluna "Done" no nosso Kanban, ele deve atender a **todos** os critérios aplicáveis listados abaixo.

Este checklist serve como uma ferramenta de qualidade para o desenvolvedor (antes de submeter para revisão) e para o revisor (durante o code review).

### 📜 Código

- [ ] O código segue o **[Padrão de Nomenclatura](./03-PADRAO_DE_NOMENCLATURA.md)** definido para arquivos e funções.
- [ ] Não há código comentado, `console.log` de debug ou arquivos desnecessários no commit final.
- [ ] A lógica de negócio está na camada de **Service**, e o acesso a dados na camada de **Repository**.
- [ ] Se novas variáveis de ambiente foram adicionadas, elas foram incluídas nos arquivos de modelo (`back-model.env`, `front-model.env`) com um valor de exemplo.
- [ ] O código é legível e, se a lógica for particularmente complexa, há comentários claros explicando o "porquê".

### 🧪 Testes

- [ ] Novos testes unitários foram criados para cobrir a nova lógica de negócio implementada.
- [ ] Todos os testes existentes (novos e antigos) passam com sucesso na suíte de automação.
- [ ] A funcionalidade foi testada manualmente pelo desenvolvedor em seu ambiente local e funciona conforme os critérios de aceite da Issue.

### ✔️ Pull Request (PR) & Revisão

- [ ] O título do PR segue o padrão Conventional Commits, conforme o **[padrão de nomeclatura](03-PADRAO_DE_NOMENCLATURA.MD)**.
- [ ] O PR foi revisado e **aprovado** por pelo menos um outro membro da equipe.
- [ ] Todos os comentários e discussões na revisão de código foram resolvidos.
- [ ] Todas as verificações automáticas (GitHub Actions) passaram com sucesso (lint do título, testes automatizados, etc.).
- [ ] O código foi mergeado na branch `develop` após a aprovação.

### 📚 Documentação

- [ ] O `README.md` ou outros documentos relevantes na pasta `docs/` foram atualizados para refletir a mudança (se aplicável).
- [ ] Se for uma nova rota de API, a documentação da API foi atualizada.
- [ ] Se for um componente de frontend reutilizável, sua documentação de uso (props, etc.) foi adicionada.

### 👍 Aceitação Final
- [ ] As regras de aceitação definidas dentro da issue foram executadas.
- [ ] A funcionalidade foi validada em um ambiente de testes/homologação e funciona como esperado.
- [ ] A Issue correspondente no GitHub foi movida para a coluna **"Done"**.