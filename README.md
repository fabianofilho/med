# med

Suíte clínica para Claude. Um único ponto de entrada (`/med`) que responde a um problema clínico de forma direta e curta, e 8 sub-comandos com templates completos para o raciocínio clínico do dia a dia: diagnóstico diferencial, nota SOAP, revisão de prescrição, nota de alta, protocolo baseado em evidências, leitura crítica de paper, explicação ao paciente e roteiro de aula.

Feita para médicos: calibração epistêmica obrigatória, linguagem técnica direta e nenhum disclaimer genérico do tipo "consulte um médico".

> Até 17/10/2026 este repositório se chamava `iamed`. O nome mudou para não confundir com o app IA.med (app.iamed.cc). O link antigo redireciona para cá.

## Comandos

| Comando | Função |
|---------|--------|
| `/med [caso]` | Modo direto: hipóteses prováveis, o que não pode perder, conduta inicial, sinais de alarme do caso e quando referenciar, em até ~25 linhas |
| `/med dx` | Diagnóstico diferencial estruturado (4 a 6 hipóteses com argumentos a favor e contra, exames, conduta inicial) |
| `/med soap` | Nota SOAP para prontuário, com CID-10, conduta com posologia, sinais de alarme e retorno |
| `/med rx` | Revisão crítica de prescrição (interações clinicamente relevantes, dose, contraindicações, duplicidades, STOPP/START) |
| `/med alta` | Nota de alta hospitalar com reconciliação medicamentosa obrigatória |
| `/med prot` | Protocolo baseado em evidências, com nível de evidência (I, IIa, IIb, III) e força (A, B, C) |
| `/med paper` | Crítica de artigo científico (PICO, viés, efeito absoluto e relativo, NNT, aplicabilidade no SUS e na saúde suplementar) |
| `/med explica` | Explicação ao paciente, sem jargão e sem infantilizar, com decisão compartilhada |
| `/med aula` | Roteiro de aula ou apresentação clínica por tempo, público e objetivos |
| `/med help` | Mostra a tabela de comandos |

A skill também aciona pelos gatilhos em linguagem natural correspondentes ("que conduta", "monta o diferencial", "nota de alta", "revisa essa prescrição", "qual o protocolo de" e outros).

## Princípios

1. **Não inventa dados.** Campo ausente vira `não informado`. Dado clínico crítico ausente é perguntado, nunca preenchido por extrapolação.
2. **Calibração epistêmica.** Distingue evidência de extrapolação e significância estatística de relevância clínica. Em incerteza real, sinaliza.
3. **Sem hedge defensivo.** Português técnico e conciso. Ressalvas só quando clinicamente relevantes para o caso.
4. **Sinais de alarme específicos do caso**, nunca genéricos do tipo "se piorar, procure o pronto-socorro".
5. **Reconciliação medicamentosa obrigatória.** Em alta e revisão de prescrição, separa mantidos, iniciados e suspensos, com o motivo.

## Instalação

### Claude Code

O Claude Code procura cada skill em `~/.claude/skills/<nome>/SKILL.md`. Clone o repositório fora dessa pasta e ligue só a pasta `med/`:

```bash
git clone https://github.com/fabianofilho/med.git ~/src/med
ln -s ~/src/med/med ~/.claude/skills/med
```

A skill aparece na próxima sessão. Para atualizar, `git -C ~/src/med pull`.

### claude.ai

Compacte a pasta `med/` (com o `SKILL.md` e a pasta `references/`) num `.zip` e envie em Personalizar > Habilidades.

## Uso como prompt direto

Sem instalar a skill, abra o template do sub-comando em [med/references/](med/references/), substitua os campos do caso e cole no Claude (ou em qualquer LLM).

## Exemplos rápidos

Modo direto:

```
/med Homem 58a, dor torácica em queimação há 2h, irradiando para o membro superior esquerdo, sudorese, HAS e DM2
```

Diagnóstico diferencial:

```
/med dx
Mulher, 64 anos, dispneia progressiva há 3 semanas, ortopneia, edema vespertino +/4+,
HAS e DM2 em uso de losartana e metformina. Exame: B3, estertores em bases,
turgência jugular a 45 graus.
```

Revisão de prescrição:

```
/med rx
Perfil: 78 anos, TFG 38 mL/min, fibrilação atrial, ICFEr 30%, DM2.
Prescrição: enalapril 20 mg 12/12h, espironolactona 50 mg/dia, AAS 100 mg/dia,
varfarina 5 mg/dia, glibenclamida 5 mg 12/12h, ibuprofeno 600 mg SOS.
```

Crítica de paper:

```
/med paper
[colar o abstract, o texto integral ou o link]
```

## Estrutura do repositório

```
med/
├── README.md
└── med/
    ├── SKILL.md
    └── references/
        ├── alta.md
        ├── aula.md
        ├── dx.md
        ├── explica.md
        ├── paper.md
        ├── prot.md
        ├── rx.md
        └── soap.md
```

A pasta interna `med/` é a skill: é ela que vai para `~/.claude/skills/` ou para o zip do claude.ai.

## Licença

Uso pessoal. Sem garantias clínicas. O conteúdo gerado pela skill auxilia o raciocínio médico, mas não substitui a responsabilidade técnica de quem prescreve, audita ou decide.
