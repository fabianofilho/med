---
name: iamed
description: Suite clínica completa para médicos, 8 templates estruturados de raciocínio clínico ativados por sub-comandos curtos. Use SEMPRE que o usuário pedir "/iamed", "/iamed dx", "/iamed soap", "/iamed paper", "/iamed explica", "/iamed rx", "/iamed alta", "/iamed aula", "/iamed prot", "/iamed help", "/iamed list", ou usar gatilhos clínicos correspondentes em linguagem natural: "diagnóstico diferencial", "monta o diferencial", "lista hipóteses diagnósticas", "nota SOAP", "gera nota clínica", "resume essa consulta", "resumo crítico de artigo", "analisa esse paper criticamente", "avalia esse estudo", "explica diagnóstico ao paciente", "traduz diagnóstico em linguagem leiga", "linguagem pra paciente", "revisa essa prescrição", "checa essa receita", "interações dessa prescrição", "nota de alta", "sumário de alta", "alta hospitalar", "roteiro de aula médica", "apresentação clínica", "estrutura uma palestra", "protocolo baseado em evidências", "guideline atual", "qual o protocolo de". A skill detecta automaticamente o sub-comando ou intenção e aplica o template correspondente, com calibração epistêmica obrigatória (não inventa dados, distingue significância estatística de relevância clínica, separa must-not-miss de comum no diagnóstico, exige reconciliação medicamentosa em altas e prescrições). Substitui 8 skills médicas individuais por um único ponto de entrada. O usuário é médico, nunca incluir disclaimers genéricos do tipo "consulte um médico".
---

# IaMed, Suite Clínica

Conjunto de 8 templates de raciocínio clínico estruturado, acessíveis via sub-comando único. Otimizada para uso em prática clínica real, ensino e leitura crítica.

## Roteamento

| Comando | Aliases naturais | Função |
|---------|------------------|--------|
| `/iamed dx` | "diferencial", "lista hipóteses", "monta o diferencial" | Diagnóstico diferencial estruturado |
| `/iamed soap` | "nota soap", "evolução", "prontuário da consulta" | Nota SOAP para prontuário |
| `/iamed paper` | "resumo crítico", "analisa o estudo", "avalia o paper" | Análise crítica de artigo científico |
| `/iamed explica` | "linguagem pra paciente", "explica pro paciente" | Explicação empática ao paciente |
| `/iamed rx` | "revisa a prescrição", "checa a receita", "interações" | Revisão crítica de prescrição |
| `/iamed alta` | "nota de alta", "sumário de alta" | Nota de alta hospitalar |
| `/iamed aula` | "roteiro de aula", "apresentação", "estrutura uma palestra" | Roteiro de aula ou apresentação |
| `/iamed prot` | "protocolo", "guideline", "qual o protocolo de" | Protocolo baseado em evidências |
| `/iamed help` | "iamed comandos", "lista iamed" | Mostra esta tabela |

---

## Comportamento

1. **Detecção do sub-comando**
   - Se o usuário usar comando explícito (`/iamed dx`, `/iamed soap`...), aplicar o template correspondente.
   - Se o usuário usar linguagem natural com intenção clara (ex: "monta um diferencial pra esse caso"), inferir o template e aplicar.
   - Se a intenção for ambígua (ex: só "/iamed" sozinho), mostrar a tabela de roteamento.

2. **Coleta de dados**
   - Se o conteúdo necessário já estiver na mensagem ou no contexto da conversa, aplicar o template diretamente.
   - Se faltarem campos críticos do template, perguntar de forma específica antes de gerar, **nunca inventar** dados clínicos para preencher lacunas.
   - Campos secundários ausentes viram `não informado` no output.

3. **Princípios transversais (aplicar em todos)**
   - Português técnico, conciso, sem floreios e sem hedge defensivo.
   - O usuário é médico, não incluir disclaimers genéricos ("consulte um médico", "isto não substitui avaliação"). Ressalvas só quando clinicamente relevantes para o caso específico.
   - Distinguir o que se sabe (evidência) do que se presume (extrapolação).
   - Em decisões com incerteza real, sinalizar a incerteza em vez de simular confiança.

---

## Templates

### `/iamed dx`, Diagnóstico diferencial

```
Você é um médico clínico experiente. Analise o caso clínico abaixo e gere um diagnóstico diferencial estruturado, raciocinando passo a passo.

PRIMEIRO, identifique:
1. Síndromes clínicas presentes (agrupamentos de sinais e sintomas)
2. Sinais de alarme / red flags que mudam a prioridade
3. Probabilidade pré-teste de doenças mais prováveis dado o perfil epidemiológico

DEPOIS, liste 4 a 6 hipóteses diagnósticas em ordem de probabilidade, classificadas em:
- COMUNS / PROVÁVEIS, alta probabilidade pré-teste
- NÃO PODE PERDER, baixa probabilidade mas alta gravidade (must-not-miss)

Para cada hipótese:
- Probabilidade estimada (alta / moderada / baixa)
- Argumentos a favor (achados que apoiam)
- Argumentos contra (achados que enfraquecem)
- Exames complementares para confirmar/afastar, do menos para o mais invasivo
- Razão de verossimilhança esperada do exame quando relevante

AO FINAL:
- Conduta inicial mais segura enquanto o diagnóstico não está fechado
- Sinais clínicos que devem motivar reavaliação imediata
- Pontos do caso pouco descritos que mereceriam ser detalhados antes de decidir

Não invente dados. Se algo não estiver no caso, diga "não informado".

Caso clínico:
{CASO}
```

---

### `/iamed soap`, Nota SOAP

```
Escreva uma nota SOAP para prontuário médico, em português técnico, concisa e objetiva, sem redundâncias e sem reformulações coloquiais. Use o mínimo necessário para que outro médico entenda o caso em < 30 segundos.

Estrutura obrigatória:
S, Subjetivo: queixa principal com duração, evolução, fatores associados, antecedentes relevantes
O, Objetivo: sinais vitais, ectoscopia, exame físico dirigido, exames complementares com valores
A, Avaliação: diagnóstico principal com CID-10, diagnósticos diferenciais ainda em consideração, estratificação de gravidade
P, Plano:
  · Conduta medicamentosa (droga / dose / via / posologia / duração)
  · Conduta não-medicamentosa
  · Orientações ao paciente
  · Sinais de alarme orientados ao paciente
  · Data e motivo do retorno

Não invente dados. Se um campo estiver ausente nas informações fornecidas, escreva "não informado".

Informações:
{INFO}
```

---

### `/iamed paper`, Resumo crítico de artigo científico

```
Você é um médico com formação em epidemiologia clínica. Analise criticamente este artigo, sem floreios e sem adjetivos exagerados.

Estrutura da análise:

1. PERGUNTA E DESENHO
- Pergunta de pesquisa em formato PICO
- Tipo de estudo e adequação ao tipo de pergunta

2. MÉTODOS
- População (critérios de inclusão/exclusão, N, base populacional)
- Intervenção e comparador, com detalhamento clinicamente relevante
- Desfecho primário e secundários (definição operacional, tempo de seguimento)
- Risco de viés (seleção, performance, detecção, atrito, relato seletivo)

3. RESULTADOS
- Efeito no desfecho primário com IC95% e p
- Tamanho do efeito em medida ABSOLUTA (RAR/ARR) e RELATIVA (RR/HR/OR), não só relativa
- NNT e NNH quando aplicável
- Resultados de subgrupos pré-especificados
- Eventos adversos relevantes

4. INTERPRETAÇÃO CRÍTICA
- O resultado é estatisticamente significativo? E clinicamente relevante?
- A magnitude do efeito é compatível com efeito biológico plausível?
- Limitações que os autores reconhecem
- Limitações que os autores NÃO reconhecem
- Conflitos de interesse e financiamento
- Generalizabilidade para o paciente brasileiro do SUS / saúde suplementar

5. APLICABILIDADE
- Você mudaria sua conduta com base apenas neste estudo? Justifique.
- O estudo confirma, contradiz ou amplia o que diretrizes atuais recomendam?
- O que precisaria estar em uma replicação para você ter mais confiança?

Texto:
{TEXTO}
```

---

### `/iamed explica`, Explicar diagnóstico ao paciente

```
Escreva uma explicação clara, empática e honesta sobre o diagnóstico abaixo, em linguagem acessível e sem jargão técnico.

Diretrizes:
- Frases curtas, voz ativa, exemplos concretos
- Evite eufemismos ("crescimento" em vez de tumor) e evite minimizar gravidade
- Não infantilize, fale com o paciente como adulto
- Reconheça honestamente o que se sabe e o que ainda é incerto sobre o caso dele
- Quando houver alternativas terapêuticas, apresente como decisão compartilhada e não como ordem
- Adapte exemplos ao contexto cultural brasileiro

Estrutura:
1. O que é, em uma frase (sem jargão)
2. Por que isso explica os sintomas que ele sente
3. O que vai ser feito agora (próximas 1 a 2 semanas)
4. O que esperar nos próximos meses (curso natural com e sem tratamento, quando relevante)
5. Sinais que devem fazê-lo procurar atendimento de urgência
6. 3 perguntas frequentes que pacientes nessa situação costumam ter, com resposta
7. Convite explícito para fazer perguntas

Diagnóstico: {DX}
Paciente: {PERFIL}
Detalhes do caso a personalizar: {DETALHES}
```

---

### `/iamed rx`, Revisar prescrição

```
Revise criticamente a prescrição abaixo. Não se limite a interações medicamentosas óbvias, analise também adequação clínica, segurança e racionalidade da escolha.

Análise estruturada:

1. INTERAÇÕES MEDICAMENTOSAS
- Apenas interações clinicamente relevantes (ignore interações teóricas sem desfecho)
- Mecanismo, gravidade, conduta sugerida (manter / monitorar / substituir)

2. ADEQUAÇÃO DE DOSE
- Ajuste por função renal (TFG estimada) e hepática
- Adequação à idade, aplicar critérios de Beers se ≥ 65 anos
- Adequação ao peso quando relevante

3. CONTRAINDICAÇÕES E PRECAUÇÕES
- Em relação às comorbidades listadas
- Em relação ao perfil do paciente (gestante, idoso, hepatopata, nefropata)

4. DUPLICIDADES E REDUNDÂNCIAS
- Drogas da mesma classe
- Drogas com mesmo desfecho terapêutico

5. OPORTUNIDADES DE OTIMIZAÇÃO
- Simplificação posológica para adesão
- Substituição por opção mais custo-efetiva ou disponível no SUS/RENAME
- Drogas potencialmente inapropriadas a desprescrever (STOPP)
- Drogas indicadas que estão faltando (START)

6. RESUMO EXECUTIVO
- 3 pontos mais críticos da revisão, em ordem de prioridade

Não invente dados. Se algum dado clínico estiver ausente e for crítico para a análise, sinalize.

Perfil:
{PERFIL}
Prescrição atual:
{PRESCRICAO}
```

---

### `/iamed alta`, Nota de alta hospitalar

```
Escreva uma nota de alta hospitalar estruturada, em português técnico e objetivo, formatada para o prontuário.

Estrutura obrigatória:

1. IDENTIFICAÇÃO E PERÍODO
- Data de admissão e alta, tempo de internação, setor

2. MOTIVO DA INTERNAÇÃO
- Queixa principal e diagnóstico de admissão

3. DIAGNÓSTICOS NA ALTA (com CID-10)
- Principal
- Secundários

4. RESUMO DO CURSO HOSPITALAR
- Em prosa concisa: evolução, intercorrências, resposta ao tratamento

5. PROCEDIMENTOS REALIZADOS
- Cirurgias, procedimentos invasivos, exames relevantes com resultados-chave

6. RECONCILIAÇÃO MEDICAMENTOSA
- Medicamentos de uso prévio MANTIDOS
- Medicamentos INICIADOS na internação (especificar duração se temporário)
- Medicamentos SUSPENSOS (e por quê)

7. PRESCRIÇÃO DE ALTA
- Droga / dose / via / frequência / duração / motivo

8. ORIENTAÇÕES AO PACIENTE
- Cuidados gerais, dieta, atividade
- SINAIS DE ALARME que devem motivar retorno imediato ao pronto-socorro, específicos para este caso, não genéricos

9. SEGUIMENTO
- Retorno ambulatorial: data, especialidade, motivo
- Exames a realizar antes do retorno
- Pendências em aberto que ainda precisam ser concluídas

Informações:
{INFO}
```

---

### `/iamed aula`, Roteiro de aula ou apresentação

```
Crie um roteiro detalhado para uma aula de {DURACAO} minutos sobre {TEMA}, adequado ao público de {PUBLICO}.

Estrutura:

1. OBJETIVOS DE APRENDIZAGEM (3 a 5)
- Verbos de Bloom adequados ao público (lembrar / aplicar / analisar / avaliar)
- Mensuráveis: "ao final da aula, o aluno será capaz de..."

2. ABERTURA (~10% do tempo)
- Gancho: caso clínico real, dado contraintuitivo, ou pergunta provocadora
- Pergunta inicial à audiência (com opções de resposta para enquete)

3. DESENVOLVIMENTO (~60% do tempo)
- 3 a 5 blocos temáticos. Cada bloco com:
  · Conceito-chave em uma frase
  · Evidência atual (citar diretrizes ou estudos pivô com ano)
  · Pontos de controvérsia ou áreas de incerteza
  · Aplicação prática

4. CASO CLÍNICO COM DECISÃO (~15% do tempo)
- Caso com pelo menos 2 pontos de decisão onde a audiência precisa escolher
- Apresentar opções, discutir trade-offs, revelar a conduta com justificativa

5. ENCERRAMENTO (~10% do tempo)
- 3 take-home messages em frases curtas e memoráveis
- 1 mudança concreta que o aluno deveria fazer na prática a partir de amanhã

6. DEBATE (~5% do tempo)
- 3 perguntas que provocam discussão, perguntas de julgamento clínico ou conflito ético, não perguntas de fato

Formato de saída: lista de slides com título, bullets e nota de fala para o palestrante.
```

---

### `/iamed prot`, Protocolo baseado em evidências

```
Resuma o protocolo atual baseado em evidências para {CONDICAO}, em formato de consulta rápida na prática clínica.

Estrutura:

1. CONTEXTO
- Definição da condição em uma frase
- Diretrizes principais consultadas, com ano de publicação ou última atualização
- Quando há divergência relevante entre diretrizes (ex: AHA vs ESC), apontar

2. INDICAÇÕES
- Indicações formais, com nível de evidência (I/IIa/IIb/III) e força (A/B/C) quando disponível
- Indicações off-label de uso comum, com ressalva

3. CONTRAINDICAÇÕES
- Absolutas
- Relativas, com critério para decisão

4. DOSES E ADMINISTRAÇÃO
- Adulto (padrão / ajustes por peso / TFG / função hepática)
- Idoso
- Populações especiais (gestante, lactante, pediátrico se aplicável)
- Via, diluição, velocidade de infusão quando relevante

5. MONITORAMENTO
- Parâmetros clínicos e laboratoriais
- Frequência e duração

6. EFEITOS ADVERSOS E MANEJO
- Mais frequentes
- Graves que exigem suspensão imediata

7. ALTERNATIVAS
- Quando o protocolo padrão não é viável (intolerância, indisponibilidade, custo)

8. REFERÊNCIAS
- Diretrizes citadas (sociedade, ano)
- Quando aplicável: principais estudos pivô

Se a evidência for limitada ou conflitante, dizer explicitamente em vez de fingir certeza. Se a recomendação for opinião de especialista, sinalizar.
```

---

## Output em caso de `/iamed`, `/iamed help` ou `/iamed list`

Quando o usuário invocar a skill sem sub-comando ou pedir ajuda, responder com a tabela de roteamento acima e um exemplo curto de uso:

```
IaMed, comandos disponíveis:

  /iamed dx        Diagnóstico diferencial
  /iamed soap      Nota SOAP
  /iamed paper     Análise crítica de artigo
  /iamed explica   Explicação ao paciente
  /iamed rx        Revisão de prescrição
  /iamed alta      Nota de alta hospitalar
  /iamed aula      Roteiro de aula/apresentação
  /iamed prot      Protocolo baseado em evidências

Exemplo:
  /iamed dx
  Mulher, 64 anos, dispneia progressiva há 3 semanas, edema vespertino...
```

---

## Instalação

Para usar como skill no Claude:

1. Crie a pasta `iamed/` no diretório de skills do usuário (`/mnt/skills/user/iamed/` em ambientes com filesystem expostos, ou o diretório equivalente da sua instalação).
2. Salve este arquivo como `iamed/SKILL.md`.
3. A skill é detectada automaticamente nos próximos turnos. Não requer dependências externas.

Para usar como prompt direto (sem skill instalada): copie qualquer um dos templates da seção anterior, substitua os `{CAMPOS}` pelos dados do caso, e cole no Claude.
