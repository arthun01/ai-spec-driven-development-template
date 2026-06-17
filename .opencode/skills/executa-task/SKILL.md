---
name: executa-task
description: Implementa tarefas de funcionalidades lendo o contexto do PRD/TechSpec, analisando dependências, executando a implementação com testes e verificações de qualidade. Marca as tarefas como concluídas em tasks.md e aciona revisão ao finalizar. Use quando o usuário solicitar implementar uma tarefa, executar uma tarefa ou começar a trabalhar em um número de tarefa específico. Não use para criar tarefas, rodar QA, revisão de código ou correção de bugs avulsos.
---

# Execução de Tarefas

## Procedimentos

**Passo 1: Configuração Pré-Tarefa (Obrigatório)**
1. Confirme que o slug da funcionalidade e o número da tarefa foram fornecidos.
2. Leia o arquivo da tarefa em `./ai-sdd/prd-[feature-slug]/tasks/[num]_task.md`.
3. Leia o PRD em `./ai-sdd/prd-[feature-slug]/prd.md` para contexto de negócio.
4. Leia a Tech Spec em `./ai-sdd/prd-[feature-slug]/techspec.md` para decisões técnicas.
5. Identifique dependências de tarefas anteriores e verifique se estão concluídas em `tasks.md`.
6. **[Design — ler design.md]** Verifique `./ai-sdd/prd-[feature-slug]/design.md`:
   - Se não existir ou estiver marcado como `N/A`: defina `has_design = false`. Para tasks com UI, gere componentes usando Tailwind de forma isolada.
   - Se existir com telas: defina `has_design = true`. Leia a tabela `## Telas`. Para cada linha relevante à task, identifique o modo pela coluna **Origem**:
     - Origem = `Paper` → tela em **modo Paper** → será usada pela `paper-to-react` (o ID do artboard está na coluna Referência).
     - Origem externa (`Figma`, `Screenshot`, `Texto`, etc.) → tela em **modo externo** → a Referência (URL, path, ou apontador) será usada como guia visual.
7. NÃO pule nenhuma dessas leituras — a tarefa será invalidada sem contexto completo.

**Passo 2: Carregar Skills e Padrões (Obrigatório)**
1. Leia `AGENTS.md` para reforçar convenções, comandos e padrões React Native/TS.
2. Identifique regras em `.opencode/rules/` relevantes para as tecnologias da tarefa (components, hooks, state-management).
3. Consulte as diretrizes em `.opencode/skills/vercel-react-best-practices/AGENTS.md` para otimizar a performance do React Native (data-fetching, renders, cache, etc).
4. Se alguma tela relevante à task tem Origem = Paper: carregue também `paper-to-react`.
5. Consulte documentação de bibliotecas envolvidas quando necessário.

**Passo 3: Análise da Tarefa (Obrigatório)**
1. Analise a tarefa identificando:
   - **Objetivo**: O que esta tarefa entrega e por quê.
   - **Requisitos**: Quais RF-XXX do PRD são atendidos.
   - **Decisões técnicas**: O que a Tech Spec define para esta área.
   - **Dependências**: Tarefas anteriores e código existente necessário.
   - **Arquivos**: Quais arquivos serão criados ou modificados.
   - **Riscos**: Pontos de atenção e possíveis complicações.
2. Gere um resumo conciso antes de iniciar a implementação.

**Passo 4: Plano de Abordagem (Obrigatório)**
1. Defina uma abordagem numerada passo a passo com base na análise.
2. Cada passo deve ser concreto e verificável.
3. Inclua ordem de implementação respeitando dependências internas.
4. Inicie a implementação imediatamente após definir o plano — NÃO aguarde aprovação.

**Passo 5: Implementação (Obrigatório)**
1. Implemente seguindo o plano definido no Passo 4.
2. Regras de implementação:
   - Siga todos os padrões do `AGENTS.md` (React Native 18+, TypeScript, Expo, NativeWind).
   - Componentize adequadamente.
   - Implemente soluções de **causa raiz** — sem workarounds.
   - Prefira utilizar hooks customizados para separar lógica da UI.
3. **[Geração de Views — por tela, conforme o modo em design.md]**:
   - Tela com Origem = Paper → execute `paper-to-react` para essa tela (converte artboard em TSX + Tailwind).
   - Tela com Origem externa → gere TSX + Tailwind manualmente **usando a Referência do `design.md` como guia visual**.
4. Siga a Tech Spec para decisões de arquitetura e design.
5. Referencie requisitos do PRD (RF-XXX) nos comentários quando relevante.

**Passo 6: Testes (Obrigatório)**
1. Crie testes para toda funcionalidade implementada:
   - **Componentes**: Testes com React Native Testing Library (RTL).
   - **Hooks/Lógica**: Testes puros em Expost ou `@testing-library/react`.
   - **E2E**: Fluxos do usuário quando aplicável.
2. Execute todos os testes: `npm run test` (ou equivalente).
3. Todos os testes devem passar — corrija falhas antes de prosseguir.

**Passo 7: Verificações Automatizadas (Obrigatório)**
1. Execute lint e formatação: `npm run lint` ou comando equivalente do projeto.
2. Verifique os tipos TypeScript: `npm run typecheck` (se existir).
3. Corrija todos os problemas apontados.
4. Re-execute os testes após correções: `npm run test`.

**Passo 8: Marcar Tarefa como Concluída (Obrigatório)**
1. Após implementação, testes passando e Linting limpo:
   - Atualize o status da tarefa em `./ai-sdd/prd-[feature-slug]/tasks.md` (⬜ → ✅).
   - Marque subtarefas concluídas na tarefa individual.

**Passo 9: Relatar Resultado**
1. Resuma o que foi implementado:
   - Arquivos criados/modificados.
   - Testes criados e resultado da execução.
   - Requisitos do PRD atendidos (RF-XXX).
   - Resultado do Lint e Typescript.
2. Informe que a tarefa está pronta para revisão via skill `executa-review`.

## Princípios Fundamentais
- Contexto completo antes de implementar — leia PRD, Tech Spec e tarefa.
- Soluções de causa raiz, nunca workarounds.
- Testes são parte integrante da implementação, não uma fase posterior.
- Siga os padrões do projeto (`AGENTS.md`) rigorosamente.
- Código limpo e verificado (ESLint/TS + testes) antes de marcar como concluído.
- A implementação deve ser fiel à Tech Spec e atender os requisitos do PRD.

## Lista de Verificação de Qualidade
- [ ] PRD, Tech Spec e arquivo da tarefa lidos completamente.
- [ ] Dependências de tarefas anteriores verificadas.
- [ ] Rules e AGENTS.md consultados.
- [ ] `design.md` lido; modo identificado por tela.
- [ ] Plano de abordagem definido.
- [ ] Implementação segue padrões React Native (Tipagem TS estrita, Componentização funcional, Tailwind).
- [ ] Implementação segue a Tech Spec.
- [ ] Testes de UI/Hooks criados e passando.
- [ ] `npm run lint` executado sem erros.
- [ ] `npm run test` executado sem falhas.
- [ ] Tarefa marcada como concluída em `tasks.md`.
- [ ] Resultado relatado ao usuário.

## Tratamento de Erros
- Se o arquivo da tarefa não existir, interrompa e informe o usuário.
- Se o PRD ou Tech Spec estiver ausente, interrompa e direcione para a skill correta.
- Se as dependências não estiverem concluídas, avise o usuário e pergunte se deseja prosseguir.
- Se os testes falharem, corrija os problemas antes de marcar a tarefa como concluída.
- Se o Lint reportar erros, corrija-os antes de finalizar.
- Se surgir ambiguidade na tarefa, consulte o PRD e Tech Spec — se ainda ambíguo, pergunte ao usuário.
