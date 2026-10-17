# `/med soap`: nota SOAP para prontuário

Escreva uma nota SOAP em português técnico, **concisa e objetiva**, sem redundâncias e sem reformulações coloquiais. Use o mínimo necessário para que outro médico entenda o caso em menos de 30 segundos.

## Estrutura obrigatória

### S, Subjetivo
- Queixa principal com duração, evolução, fatores de melhora/piora, sintomas associados.
- Antecedentes pessoais relevantes (HAS, DM, cirurgias prévias, alergias, medicações em uso, hábitos).
- Antecedentes familiares quando relevantes para o caso.

### O, Objetivo
- Sinais vitais (PA, FC, FR, SatO2, T, glicemia capilar se aplicável).
- Ectoscopia em 1 frase.
- Exame físico dirigido aos sistemas relevantes para a queixa.
- Exames complementares com **valores numéricos**, não apenas "normal" ou "alterado".

### A, Avaliação
- Diagnóstico principal com **CID-10**.
- Diagnósticos diferenciais ainda em consideração, em ordem de probabilidade.
- Estratificação de gravidade quando aplicável (NYHA, CURB-65, Wells, GRACE, etc.).

### P, Plano
- **Conduta medicamentosa**: droga / dose / via / posologia / duração.
- **Conduta não-medicamentosa**: medidas, repouso, dieta, atividades, encaminhamentos.
- **Orientações ao paciente** específicas do caso.
- **Sinais de alarme** orientados ao paciente, **específicos do diagnóstico**, não genéricos.
- **Data e motivo do retorno**.

## Regras

- **Não invente dados.** Campo ausente nas informações fornecidas vira "não informado".
- Posologia obrigatória nas drogas prescritas.
- Sinais de alarme sempre específicos do caso (ex: em ITU baixa, orientar dor lombar/febre como gatilho de reavaliação; não escrever "se piorar procure pronto-socorro").
- Sem disclaimers genéricos. Usuário é médico.
- Se a queixa, exame físico ou exames vierem incompletos e isso comprometer a avaliação, peça os campos faltantes antes de escrever a nota.
