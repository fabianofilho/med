# IaMed

Suite clínica para Claude. Um único ponto de entrada (`/iamed`) com 8 sub-comandos que cobrem o raciocínio clínico estruturado do dia a dia médico: diagnóstico diferencial, nota SOAP, leitura crítica de paper, explicação ao paciente, revisão de prescrição, nota de alta, roteiro de aula e protocolo baseado em evidências.

Substitui 8 skills médicas individuais por uma só, com calibração epistêmica obrigatória e linguagem técnica direta. Feita para médicos, sem disclaimers genéricos do tipo "consulte um médico".

## Comandos

| Comando | Função |
|---------|--------|
| `/iamed dx` | Diagnóstico diferencial estruturado (comum vs. must-not-miss, exames por verossimilhança) |
| `/iamed soap` | Nota SOAP para prontuário, concisa, pronta pra colar |
| `/iamed paper` | Análise crítica de artigo (PICO, viés, efeito absoluto + relativo, NNT/NNH, aplicabilidade SUS) |
| `/iamed explica` | Explicação ao paciente, empática, sem jargão, sem infantilizar |
| `/iamed rx` | Revisão de prescrição (interação clinicamente relevante, dose, Beers, STOPP/START) |
| `/iamed alta` | Nota de alta hospitalar com reconciliação medicamentosa e sinais de alarme específicos |
| `/iamed aula` | Roteiro de aula/apresentação por tempo, público e objetivos de Bloom |
| `/iamed prot` | Protocolo baseado em evidências (níveis I/IIa/IIb/III, divergências entre diretrizes) |
| `/iamed help` | Mostra a tabela de comandos |

A skill também aciona pelos gatilhos em linguagem natural correspondentes ("monta o diferencial", "nota de alta", "revisa essa prescrição", etc).

## Princípios

1. **Não inventa dados.** Campo ausente vira `não informado`. Dado clínico crítico ausente é sinalizado e perguntado, nunca preenchido por extrapolação.
2. **Calibração epistêmica.** Distingue evidência de presunção. Distingue significância estatística de relevância clínica. Em incerteza real, sinaliza.
3. **Sem hedge defensivo.** Português técnico, conciso. Ressalvas só quando clinicamente relevantes para o caso.
4. **Decisão compartilhada quando cabível.** Em alternativas terapêuticas, apresenta opções e trade-offs em vez de ordens.
5. **Reconciliação medicamentosa obrigatória.** Em alta e revisão de prescrição, separa explicitamente mantidos / iniciados / suspensos.

## Instalação como Claude Skill

Clone o repositório dentro do diretório de skills do usuário (ou copie a pasta `iamed/`):

```bash
# Local típico em filesystem exposto (Claude Code)
cd ~/.claude/skills
git clone https://github.com/fabianofilho/iamed.git
```

Estrutura final esperada:

```
~/.claude/skills/iamed/
└── iamed/
    └── SKILL.md
```

A skill é detectada automaticamente nos próximos turnos. Não tem dependência externa, só o arquivo `SKILL.md`.

## Uso como prompt direto

Sem instalar a skill, basta abrir [iamed/SKILL.md](iamed/SKILL.md), copiar o template do sub-comando que interessa, substituir os campos `{CASO}`, `{INFO}`, `{TEXTO}`, `{PERFIL}`, `{PRESCRICAO}`, `{DX}`, `{TEMA}`, `{DURACAO}`, `{PUBLICO}`, `{CONDICAO}` etc., e colar no Claude (ou em qualquer LLM).

## Exemplos rápidos

Diagnóstico diferencial:

```
/iamed dx
Mulher, 64 anos, dispneia progressiva há 3 semanas, ortopneia, edema vespertino +/4+,
HAS e DM2 em uso de losartana e metformina. Exame: B3, estertores em bases,
turgência jugular a 45 graus.
```

Revisão de prescrição:

```
/iamed rx
Perfil: 78 anos, TFG 38 mL/min, fibrilação atrial, ICFEr 30%, DM2.
Prescrição: enalapril 20 mg 12/12h, espironolactona 50 mg/dia, AAS 100 mg/dia,
varfarina 5 mg/dia, glibenclamida 5 mg 12/12h, ibuprofeno 600 mg SOS.
```

Resumo crítico de paper:

```
/iamed paper
[colar abstract ou texto integral]
```

## Estrutura do repositório

```
iamed/
├── README.md
└── iamed/
    └── SKILL.md
```

A pasta interna `iamed/` é proposital: é o nome da skill e o que o Claude espera no diretório de skills.

## Licença

Uso pessoal. Sem garantias clínicas. O conteúdo gerado pela skill auxilia o raciocínio médico mas não substitui a responsabilidade técnica de quem prescreve, audita ou decide.
