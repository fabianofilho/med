# `/med alta`: nota de alta hospitalar

Escreva uma nota de alta hospitalar estruturada, em português técnico e objetivo, formatada para prontuário. **Reconciliação medicamentosa é obrigatória**.

## Estrutura obrigatória

### 1. Identificação e período
- Data de admissão e data de alta.
- Tempo de internação (em dias).
- Setor (enfermaria, UTI, etc.).

### 2. Motivo da internação
- Queixa principal e diagnóstico de admissão.

### 3. Diagnósticos na alta (com CID-10)
- **Principal**.
- **Secundários** relevantes (comorbidades ativas, intercorrências da internação).

### 4. Resumo do curso hospitalar
- Prosa concisa: evolução clínica, intercorrências, resposta ao tratamento, estabilização.

### 5. Procedimentos realizados
- Cirurgias, procedimentos invasivos, exames de imagem com **resultados-chave**.

### 6. Reconciliação medicamentosa (obrigatória)
Liste explicitamente em 3 grupos:

- **MANTIDOS**: medicamentos de uso prévio mantidos na alta.
- **INICIADOS**: medicamentos iniciados durante a internação que continuarão na alta. Para cada um, especificar **duração total** se for temporário (ex: antibiótico por 7 dias).
- **SUSPENSOS**: medicamentos de uso prévio suspensos, **com o motivo da suspensão**.

### 7. Prescrição de alta
- Cada droga com: nome / dose / via / frequência / duração / motivo.

### 8. Orientações ao paciente
- Cuidados gerais, dieta, atividade física, retorno ao trabalho.
- **SINAIS DE ALARME** que motivam retorno imediato ao pronto-socorro, **específicos para este caso** (não genéricos).

### 9. Seguimento
- **Retorno ambulatorial**: data, especialidade, motivo.
- **Exames a realizar antes do retorno**.
- **Pendências em aberto** que ainda precisam ser concluídas (biópsia pendente, consulta a especialista, etc.).

## Regras

- **Não invente dados.** Campo ausente vira "não informado".
- Sinais de alarme **sempre específicos do caso** (ex: pós-IAM, orientar dor torácica recorrente, dispneia, sudorese; não "se sentir mal, retorne").
- Posologia completa em todas as drogas da prescrição.
- Reconciliação medicamentosa não pode ser omitida, mesmo que o paciente não use medicações prévias (registrar explicitamente "sem medicações prévias").
- Sem disclaimers genéricos. Usuário é médico.
