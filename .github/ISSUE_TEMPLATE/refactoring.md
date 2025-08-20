---
name: "♻️ Refatoração Técnica"
about: Propor uma melhoria na estrutura, código ou performance
title: "[REFACTOR] Descrever a área a ser refatorada"
labels: "Refatoração ♻️, Code smell 👃"
assignees: ''

---

## 🚮 Qual parte do código precisa ser refatorada?

Descreva a área do sistema, os arquivos ou os componentes que são o alvo desta refatoração.

## 🤔 Por que esta refatoração é necessária?

Explique o problema atual. O código está difícil de manter? Existe um gargalo de performance? Há um "code smell" específico? Qual o benefício esperado?

## 💡 Proposta de Solução

Descreva a abordagem técnica que você sugere para a refatoração.
*(Ex: "Aplicar o padrão Service-Repository no `productsController`", "Extrair o componente `Card` para ser reutilizável", "Otimizar a query de busca de usuários").*

## ✅ Critérios de Aceitação

Como saberemos que a refatoração foi um sucesso e não quebrou nada?
- [ ] Todo o comportamento externo da aplicação permanece o mesmo.
- [ ] Todos os testes existentes continuam passando.
- [ ] Novos testes foram criados para validar a nova estrutura (se aplicável).
- [ ] A performance melhorou em X% (se aplicável).

## 📌 Lembrete de Qualidade
Lembre-se que, para esta tarefa ser considerada concluída, ela deverá atender a todos os critérios da nossa **[✅ Definição de "Pronto" (DoD)](../../docs/05-DEFINITION_OF_DONE.md)**.