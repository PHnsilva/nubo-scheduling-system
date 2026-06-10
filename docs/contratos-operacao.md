# Nubo - Contratos de Operacao

Os contratos abaixo complementam os Diagramas de Sequencia do Sistema e registram pre-condicoes, pos-condicoes e efeitos das operacoes de dominio modeladas.

## CO01 - solicitarServico(dadosSolicitacao)

| Campo | Descricao |
|---|---|
| Operacao | `solicitarServico(dadosSolicitacao)` |
| Referencias cruzadas | UC04, US04 |
| Ator principal | Cliente |
| Pre-condicoes | Cliente autenticado; categoria ativa; endereco informado; data preferencial futura. |
| Pos-condicoes | Solicitacao criada com status `ABERTA`; endereco associado; historico inicial registrado; notificacao emitida para prestadores compativeis. |

### Efeitos

1. Criar instancia de `SolicitacaoServico`.
2. Associar `Cliente`, `CategoriaServico` e `Endereco`.
3. Definir status inicial como `ABERTA`.
4. Registrar `HistoricoStatus`.
5. Disparar evento de notificacao.

---

## CO02 - enviarProposta(dadosProposta)

| Campo | Descricao |
|---|---|
| Operacao | `enviarProposta(dadosProposta)` |
| Referencias cruzadas | UC07, US07 |
| Ator principal | Prestador |
| Pre-condicoes | Prestador autenticado; prestador ativo; solicitacao em `ABERTA` ou `COM_PROPOSTAS`; categoria da solicitacao vinculada ao prestador. |
| Pos-condicoes | Proposta criada com status `ENVIADA`; solicitacao alterada para `COM_PROPOSTAS`; cliente notificado. |

### Efeitos

1. Validar compatibilidade entre prestador e categoria.
2. Criar instancia de `Proposta`.
3. Associar proposta a solicitacao.
4. Atualizar status da solicitacao para `COM_PROPOSTAS`.
5. Registrar historico.
6. Disparar notificacao ao cliente.

---

## CO03 - selecionarProposta(idSolicitacao, idProposta)

| Campo | Descricao |
|---|---|
| Operacao | `selecionarProposta(idSolicitacao, idProposta)` |
| Referencias cruzadas | UC08, UC09, US08, US09 |
| Ator principal | Cliente |
| Pre-condicoes | Cliente autenticado; solicitacao pertence ao cliente; proposta enviada esta ativa; solicitacao ainda nao esta agendada. |
| Pos-condicoes | Proposta selecionada; demais propostas recusadas; agendamento criado com status `CONFIRMADO`; solicitacao alterada para `AGENDADA`; prestador e cliente notificados. |

### Efeitos

1. Validar propriedade da solicitacao.
2. Validar disponibilidade da proposta.
3. Marcar proposta como `SELECIONADA`.
4. Marcar propostas concorrentes como `RECUSADA`.
5. Criar `Agendamento`.
6. Atualizar `SolicitacaoServico` para `AGENDADA`.
7. Registrar historico.
8. Disparar notificacoes.

---

## CO04 - cancelarAgendamento(idAgendamento, motivo)

| Campo | Descricao |
|---|---|
| Operacao | `cancelarAgendamento(idAgendamento, motivo)` |
| Referencias cruzadas | UC10, US10 |
| Ator principal | Cliente ou Prestador |
| Pre-condicoes | Usuario autenticado; usuario participa do agendamento; agendamento nao esta concluido. |
| Pos-condicoes | Agendamento alterado para `CANCELADO`; motivo registrado; historico atualizado; partes notificadas. |

### Efeitos

1. Validar participacao do usuario no agendamento.
2. Validar que o agendamento ainda pode ser cancelado.
3. Alterar status para `CANCELADO`.
4. Registrar motivo e historico.
5. Atualizar solicitacao relacionada quando necessario.
6. Disparar notificacao.

---

## CO05 - concluirAtendimento(idAgendamento)

| Campo | Descricao |
|---|---|
| Operacao | `concluirAtendimento(idAgendamento)` |
| Referencias cruzadas | UC11, US11 |
| Ator principal | Prestador |
| Pre-condicoes | Prestador autenticado; prestador e responsavel pelo agendamento; agendamento esta `CONFIRMADO` ou `EM_ATENDIMENTO`. |
| Pos-condicoes | Agendamento alterado para `CONCLUIDO`; solicitacao encerrada; cliente liberado para avaliar. |

### Efeitos

1. Validar responsabilidade do prestador.
2. Alterar status para `CONCLUIDO`.
3. Atualizar solicitacao para `ENCERRADA`.
4. Registrar historico.
5. Enviar notificacao ao cliente solicitando avaliacao.
