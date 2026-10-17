---
name: med
description: >
  Suíte clínica para médicos. Recebe um problema clínico (queixa, caso, sintoma, prescrição, paper,
  diagnóstico) e entrega resposta direta e curta: hipóteses prováveis, conduta inicial, sinais de
  alarme, quando referenciar; oferece aprofundar em 8 templates por sub-comando: /med [caso], /med dx,
  soap, rx, alta, prot, paper, explica, aula, help, list. Use SEMPRE que pedir "/med" ou gatilhos
  clínicos: "que conduta", "como abordar", "monta o diferencial", "diagnóstico diferencial", "nota
  soap", "evolução", "revisa essa prescrição", "interações dessa receita", "nota de alta", "sumário de
  alta", "qual o protocolo de", "guideline atual de", "analisa esse paper", "avalia esse estudo",
  "explica o diagnóstico ao paciente", "linguagem leiga", "roteiro de aula", "apresentação clínica".
  Calibra incerteza, exige reconciliação medicamentosa em alta e rx, separa significância estatística
  de relevância clínica e must-not-miss de comum. O usuário é médico, sem disclaimers genéricos.
---

# Skill `/med`: suite clínica para resposta médica direcionada

A skill atende dois modos:

1. **Modo direto** (`/med <problema>` ou linguagem natural sem sub-comando): responde de forma compacta e acionável. Hipóteses prováveis, conduta inicial, sinais de alarme do caso, quando referenciar. Encerra oferecendo aprofundar.
2. **Modo sub-comando** (`/med dx`, `/med soap`, etc.): aplica um dos 8 templates clínicos estruturados. A versão pública fica em github.com/fabianofilho/med (até 17/10/2026 o repositório se chamava iamed, nome que agora é só da skill de indexação do app IA.med).

O usuário é médico. Sem disclaimers genéricos do tipo "consulte um médico" ou "isto não substitui avaliação". Ressalvas só quando clinicamente relevantes para o caso.

---

## Roteamento

| Entrada do usuário | Modo | Ação |
|---|---|---|
| `/med` sozinho, `/med help`, `/med list` | help | Mostrar a tabela de comandos (ver seção `/med help`) |
| `/med <texto livre com problema clínico>` | direto | Aplicar o [bloco direto](#modo-direto) |
| `/med dx` + caso | sub-comando | Ler e aplicar `references/dx.md` |
| `/med soap` + info | sub-comando | Ler e aplicar `references/soap.md` |
| `/med rx` + perfil e prescrição | sub-comando | Ler e aplicar `references/rx.md` |
| `/med alta` + info da internação | sub-comando | Ler e aplicar `references/alta.md` |
| `/med prot` + condição | sub-comando | Ler e aplicar `references/prot.md` |
| `/med paper` + texto, link ou PDF | sub-comando | Ler e aplicar `references/paper.md` |
| `/med explica` + diagnóstico e perfil | sub-comando | Ler e aplicar `references/explica.md` |
| `/med aula` + tema, duração, público | sub-comando | Ler e aplicar `references/aula.md` |

### Gatilhos naturais (sem `/med` explícito)

| Frase do usuário | Roteamento |
|---|---|
| "monta o diferencial", "lista hipóteses", "que diagnósticos pensar" | `dx` |
| "nota soap", "evolução", "prontuário da consulta", "registra a consulta" | `soap` |
| "revisa essa prescrição", "checa essa receita", "interações da receita" | `rx` |
| "nota de alta", "sumário de alta", "alta hospitalar" | `alta` |
| "qual o protocolo de", "guideline atual de", "protocolo de [condição]" | `prot` |
| "analisa esse paper", "resumo crítico de artigo", "avalia esse estudo" | `paper` |
| "explica pro paciente", "linguagem leiga", "como conto isso pro paciente" | `explica` |
| "roteiro de aula", "monta uma palestra sobre", "apresentação clínica" | `aula` |
| "que conduta", "como abordar", "que fazer com" + caso clínico | modo direto |

Se o usuário invocar uma intenção claramente coberta por outra skill instalada e mais especializada (ex: pedir para escrever uma anamnese no formato F-S-O-A-P do `/clude`, auditar um prontuário com `/auditar-prontuario`, indexar conteúdo no Notion com `/iamed`), avise e sugira a skill correta. A `/med` não duplica essas funções.

---

## Modo direto

Quando o usuário joga um problema clínico sem sub-comando explícito (`/med` seguido de texto livre, ou linguagem natural como "que conduta com", "como abordar esse caso"), entregue em bloco compacto, em português técnico:

```
**Problema:** [reformular em 1 frase com o que importa clinicamente]

**Hipóteses prováveis** (3 a 4, em ordem de probabilidade):
1. **[Diagnóstico]**: argumentos a favor / pegada-chave no caso
2. **[Diagnóstico]**: ...
3. ...

**Não pode perder** (must-not-miss, baixa probabilidade e alta gravidade):
- **[Diagnóstico]**: quando suspeitar neste caso

**Conduta inicial** (enquanto investiga):
- [Exames de primeira linha, dirigidos ao diferencial acima]
- [Tratamento empírico se aplicável, com droga / dose / via]
- [Estabilização se necessário]

**Sinais de alarme** (motivam reavaliação imediata, específicos do caso):
- [Sinal 1 dirigido às hipóteses acima]
- [Sinal 2]

**Quando referenciar:**
- [Especialidade / nível de cuidado e gatilho clínico]

**Aprofundar:**
- `/med dx` diferencial completo (4 a 6 hipóteses com argumentos pró e contra, exames, RV)
- `/med soap` nota estruturada para prontuário
- `/med prot [condição]` protocolo baseado em evidência
```

**Regras do modo direto:**

- Máximo ~25 linhas no total. Quem precisa de mais profundidade aprofunda via sub-comando.
- Não invente dados. Se faltar info crítica (idade, sexo, duração, comorbidades, sinais vitais), peça antes de gerar, em pergunta específica e única.
- Sinais de alarme são **específicos para as hipóteses do caso**, nunca genéricos do tipo "se piorar procure pronto-socorro".
- Conduta vem com posologia quando relevante (droga, dose, via, frequência, duração).
- Se o problema for sobre prescrição, paper, paciente leigo, alta, aula, protocolo ou nota SOAP, **rote para o sub-comando correto** em vez de tentar entregar bloco direto.
- O bloco "Aprofundar" sugere de 2 a 3 sub-comandos mais relevantes para o caso, não todos.

---

## Sub-comandos: 8 templates clínicos

Quando o usuário invocar um sub-comando, **leia o arquivo correspondente em `references/` e aplique exatamente como descrito ali**. Os templates já incluem instruções de tom, calibração e formato.

| Sub-comando | Arquivo | Função |
|---|---|---|
| `dx` | [references/dx.md](references/dx.md) | Diagnóstico diferencial estruturado, 4 a 6 hipóteses com argumentos pró/contra, exames, conduta inicial |
| `soap` | [references/soap.md](references/soap.md) | Nota SOAP para prontuário, com CID-10, conduta com posologia, sinais de alarme, retorno |
| `rx` | [references/rx.md](references/rx.md) | Revisão crítica de prescrição: interações, dose, contraindicações, duplicidades, STOPP/START |
| `alta` | [references/alta.md](references/alta.md) | Nota de alta hospitalar com reconciliação medicamentosa obrigatória |
| `prot` | [references/prot.md](references/prot.md) | Protocolo baseado em evidências com nível de evidência (I/IIa/IIb/III) e força (A/B/C) |
| `paper` | [references/paper.md](references/paper.md) | Crítica de artigo científico: PICO, viés, efeito absoluto, NNT, aplicabilidade |
| `explica` | [references/explica.md](references/explica.md) | Explicação ao paciente em linguagem acessível, sem jargão, com decisão compartilhada |
| `aula` | [references/aula.md](references/aula.md) | Roteiro de aula ou apresentação clínica com objetivos, caso, encerramento, debate |

Se o sub-comando vier sem o conteúdo necessário (ex: `/med dx` sozinho, sem caso clínico), peça o caso antes de aplicar o template. Não invente.

---

## Comportamento transversal

Estes princípios valem em todos os modos:

1. **Sem disclaimers genéricos.** O usuário é médico, no campo. Não escrever "consulte um médico", "isto não substitui avaliação", "apenas para fins informativos".
2. **Não inventar dados clínicos.** Se faltar informação crítica, perguntar de forma específica e única. Campo secundário ausente vira "não informado" no output.
3. **Calibrar incerteza.** Quando a evidência for fraca, conflitante ou houver extrapolação relevante, sinalizar explicitamente. Não simular confiança.
4. **Distinguir evidência de extrapolação.** Especialmente em `paper`, `prot` e bloco direto: separar o que o estudo/diretriz mostra do que é interpretação clínica.
5. **Distinguir significância estatística de relevância clínica.** Vale para `paper` e `prot`.
6. **Conduta com posologia.** Drogas em `soap`, `rx`, `alta` e no bloco direto devem vir com droga / dose / via / frequência / duração quando aplicável.
7. **Sinais de alarme sempre específicos do caso.** Nunca genéricos.
8. **Reconciliação medicamentosa obrigatória em `alta`.** Medicamentos mantidos, iniciados e suspensos, com motivo.
9. **Português técnico brasileiro.** Sem travessões (usar vírgula, ponto ou dois pontos). Sem hífens decorativos como separador de título. Sem aspas tipográficas, só `"` e `'` retas. Sem reticências Unicode, só `...`.

---

## `/med help` / `/med list` / `/med` sozinho

Quando o usuário invocar a skill sem caso clínico e sem sub-comando, responder com:

```
/med - suite clínica para resposta médica direcionada

Modo direto:
  /med <problema>           resposta compacta + sinais de alarme + opção de aprofundar

Sub-comandos (templates completos):
  /med dx        diagnóstico diferencial estruturado
  /med soap      nota SOAP para prontuário
  /med rx        revisão crítica de prescrição
  /med alta      nota de alta hospitalar
  /med prot      protocolo baseado em evidências
  /med paper     crítica de artigo científico
  /med explica   explicação ao paciente em linguagem leiga
  /med aula      roteiro de aula ou apresentação

Exemplo:
  /med Homem 58a, dor torácica em queimação há 2h, irradiando MSE, sudorese, HAS e DM2
```

---

## Relação com outras skills médicas instaladas

A `/med` não duplica funções de skills mais especializadas:

- **`/clude`** monta anamnese no formato F-S-O-A-P específico de telemedicina do Fabiano. Para nota de prontuário **padrão SOAP genérico**, use `/med soap`. Para a nota no padrão específico de telemedicina, use `/clude`.
- **`/rx`** emite a receita no Whitebook (paciente, dose, assinatura). A `/med rx` só revisa uma prescrição já escrita: interações, dose, contraindicações, STOPP/START, sem emitir nada.
- **`/auditar-prontuario`** audita uma anamnese já escrita usando o sistema local de auditoria. A `/med` não audita, ela ajuda a raciocinar e escrever.
- **`/iamed`** indexa conteúdo (paper, podcast, aula, post) no database Notion do Fabiano. A `/med paper` analisa criticamente, **não indexa**. Se o usuário quiser ambos: aplicar `/med paper` primeiro, depois sugerir `/iamed` para indexar a análise.

Se houver dúvida entre `/med` e uma skill específica, prefira a específica.
