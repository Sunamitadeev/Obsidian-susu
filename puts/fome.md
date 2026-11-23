---
name: analise-curriculo-vs-vaga
description: Use when the user shares a resume/CV and a job posting and wants to know if the resume would pass ATS keyword screening or a recruiter's 10-second read, wants a match score, or asks "why am I not getting interviews for this job".
---

# Análise de Currículo vs. Descrição de Vaga

## Overview

Simula, em uma única resposta, os dois filtros que um currículo enfrenta antes de virar entrevista: a triagem automática de um ATS (palavras-chave) e a leitura de 10 segundos de um recrutador humano. Termina em uma nota de 0 a 100 com rubrica explícita, para que a lacuna fique acionável, não vaga.

Framework: RISEN (Role, Instructions, Steps, End goal, Narrowing).

## When to Use

- O usuário cola um currículo + uma vaga e pergunta se "está bom" para aquela vaga.
- O usuário quer saber por que não está sendo chamado para entrevistas.
- Antes de reescrever qualquer seção do currículo — esta análise gera os insumos (palavras-chave ausentes, pontos fracos) para [[reescrita-experiencia-curriculo]].
- Não use isto se o usuário só quer uma reescrita direta sem comparar contra uma vaga específica — nesse caso vá direto para a reescrita.

## How to Use

1. Peça (se ainda não tiver): área/função da vaga, nome da empresa, descrição completa da vaga, texto completo do currículo.
2. Preencha os 4 colchetes do prompt abaixo.
3. Rode a análise em uma conversa nova (ou nesta mesma, se for a primeira etapa).
4. Guarde a lista de palavras-chave ausentes e os pontos de atenção da Etapa 4 — eles alimentam diretamente a skill [[reescrita-experiencia-curriculo]].

## Prompt

```
Aja como um recrutador sênior com mais de 10 anos de experiência contratando para a área de
estagio em empresas como a [inserir nome da empresa]. Você domina tanto o
comportamento de triagem humana (leitura de 10 segundos) quanto o funcionamento de sistemas
ATS de filtragem por palavras-chave.

Sua tarefa é avaliar meu currículo em relação à descrição de vaga abaixo e me dizer exatamente o
que me impediria de ser chamado para a entrevista.

DESCRIÇÃO DA VAGA:
[Inserir a descrição da vaga aqui]

MEU CURRÍCULO:
[Inserir o seu currículo aqui]

Siga esta ordem de análise, usando o título de cada etapa como seção da sua resposta:

1. PERFIL IDEAL: antes de avaliar, extraia da descrição da vaga o perfil do candidato ideal:
requisitos obrigatórios, requisitos desejáveis e as 10 palavras-chave mais prováveis de serem
usadas como filtro por um ATS.

2. TRIAGEM ATS: compare essas palavras-chave com o texto literal do meu currículo. Liste as
ausentes em ordem de importância (no máximo 5; se houver menos, liste menos). Para cada uma,
indique em qual seção do currículo ela deve entrar e escreva uma frase de exemplo pronta para uso.

3. TRIAGEM DE 10 SEGUNDOS: lendo apenas título profissional, empresa atual, últimos cargos e
formação, aponte os 3 elementos que mais gerariam dúvida ou rejeição imediata em um recrutador,
e por quê.

4. LEITURA PROFUNDA: para cada seção do currículo, classifique como forte ou fraca. Cite o
trecho exato que sustenta o julgamento. Para cada seção fraca, mostre a reescrita no formato
antes/depois.

5. COMPARAÇÃO COM O PERFIL IDEAL: compare meu currículo com o perfil extraído na etapa 1.
Separe as lacunas em dois grupos: as corrigíveis apenas reescrevendo o currículo e as lacunas reais
de experiência que exigem outra estratégia (projetos, certificações, reposicionamento).

6. PONTUAÇÃO FINAL: só depois de concluir as etapas anteriores, dê uma nota de 0 a 100 usando
esta rubrica: aderência aos requisitos obrigatórios (0-40), compatibilidade com ATS (0-20), impacto e
resultados quantificados (0-20), clareza e escaneabilidade (0-20). Mostre a pontuação de cada
critério separadamente e indique qual correção única aumentaria mais a nota total.

Regras:
- Baseie cada afirmação em trechos literais da vaga ou do currículo. Não presuma nada que não
esteja escrito.
- Se uma informação essencial estiver ausente (datas, senioridade, resultados), aponte a ausência
em vez de inventar.
- Seja brutalmente honesto: sem elogios de cortesia e sem suavizar problemas. Toda crítica deve vir
acompanhada de uma correção acionável.
- Prefiro corrigir problemas agora a ser ignorado depois.
```

## Quick Reference

| Etapa | Produz |
|---|---|
| 1. Perfil ideal | Requisitos + 10 palavras-chave do ATS |
| 2. Triagem ATS | Até 5 palavras-chave ausentes, com onde inserir |
| 3. Triagem de 10s | 3 elementos que causam rejeição imediata |
| 4. Leitura profunda | Seções fortes/fracas com antes/depois |
| 5. Comparação com perfil ideal | Lacunas corrigíveis vs. lacunas reais de experiência |
| 6. Pontuação final | Nota 0-100 por rubrica + correção de maior impacto |

## Common Mistakes

- Pular a Etapa 1 e ir direto à nota — a rubrica perde a referência do que era "ideal" para a vaga.
- Deixar o usuário colar a vaga resumida ("dev Python sênior") em vez do texto completo — as 10 palavras-chave saem pobres.
- Inventar métricas ou conquistas que não estão no currículo para justificar uma nota melhor — vá contra a regra explícita do prompt.