# Documentação de Projeto para o Sistema Nubo

**Sistema:** Nubo - Plataforma de Agendamento e Gestão de Serviços Locais  
**Versão:** 1.0  
**Projeto elaborado por:** Pedro Henrique Silva Vargas, como parte da disciplina Projeto de Software.  
**Data de criação:** 09 de junho de 2026  

## Tabela de Conteúdo

1. Introdução  
2. Modelos de Usuário e Requisitos  
2.1 Descrição de Atores  
2.2 Modelo de Casos de Uso e Histórias de Usuários  
2.3 Diagrama de Sequência do Sistema e Contrato de Operações  
3. Modelos de Projeto  
3.1 Arquitetura  
3.2 Diagrama de Componentes e Implantação  
3.3 Diagrama de Classes  
3.4 Diagramas de Sequência  
3.5 Diagramas de Comunicação  
3.6 Diagramas de Estados  
4. Modelos de Dados  
Histórico de Revisões  

## 1. Introdução

Este documento agrega: 1) a elaboração e revisão de modelos de domínio e 2) modelos de projeto para o sistema **Nubo**. A referência principal para a descrição geral do problema, domínio, requisitos funcionais, requisitos não funcionais e regras de negócio do sistema é o documento de especificação que descreve a visão de domínio do sistema.

A especificação de referência está disponível em:

- [`docs/especificacao-requisitos.md`](./especificacao-requisitos.md)
- [`docs/especificacao-requisitos.pdf`](./especificacao-requisitos.pdf)

O Nubo é uma plataforma web para intermediação, proposta, agendamento e acompanhamento de serviços locais. O sistema conecta clientes que precisam contratar serviços a prestadores capazes de analisar solicitações, enviar propostas e executar atendimentos agendados. A documentação de projeto apresentada neste arquivo organiza os modelos necessários para explicar como o domínio do sistema é representado e como os principais casos de uso são realizados internamente.

## 2. Modelos de Usuário e Requisitos

Esta seção apresenta os atores, histórias de usuário, casos de uso, diagramas de sequência do sistema e contratos de operação utilizados para representar os requisitos do Nubo em nível de usuário e sistema.

### 2.1 Descrição de Atores

| Ator | Descrição |
|---|---|
| Visitante | Pessoa ainda não autenticada que pode iniciar cadastro ou login. |
| Cliente | Usuário que solicita serviços, acompanha solicitações, escolhe propostas, cancela agendamentos e avalia atendimentos. |
| Prestador | Usuário que oferece serviços, consulta solicitações compatíveis, envia propostas, realiza atendimentos e registra conclusão. |
| Administrador | Usuário responsável por gerenciar usuários, prestadores, categorias, solicitações e relatórios. |
| Serviço de Notificação | Sistema externo responsável pelo envio fictício de notificações relacionadas aos principais eventos do fluxo. |

### 2.2 Modelo de Casos de Uso e Histórias de Usuários

O modelo de casos de uso está representado em `codigos/diagrama-de-caso-de-uso.puml` e renderizado em `imagens/diagrama-de-caso-de-uso.png`. Cada caso de uso possui um identificador próprio, utilizado como referência cruzada nos contratos de operação e demais modelos.

#### Histórias de Usuário

| ID | História |
|---|---|
| US01 | Como visitante, quero criar uma conta de cliente para solicitar serviços pela plataforma. |
| US02 | Como visitante, quero criar uma conta de prestador para oferecer serviços locais. |
| US03 | Como usuário cadastrado, quero autenticar-me com segurança para acessar minhas funcionalidades. |
| US04 | Como cliente, quero cadastrar uma solicitação de serviço informando categoria, endereço, descrição e datas preferenciais. |
| US05 | Como cliente, quero acompanhar o status das minhas solicitações para saber se há propostas recebidas. |
| US06 | Como prestador, quero visualizar solicitações compatíveis com minhas categorias e região de atendimento. |
| US07 | Como prestador, quero enviar uma proposta com preço, data, horário e observações. |
| US08 | Como cliente, quero comparar propostas recebidas e escolher uma delas para confirmar o agendamento. |
| US09 | Como prestador, quero aceitar a confirmação do cliente e assumir o atendimento. |
| US10 | Como cliente ou prestador, quero cancelar um agendamento justificando o motivo. |
| US11 | Como prestador, quero concluir um atendimento para registrar o fim do serviço. |
| US12 | Como cliente, quero avaliar o prestador após a conclusão do serviço. |
| US13 | Como administrador, quero gerenciar categorias de serviço para manter o catálogo da plataforma. |
| US14 | Como administrador, quero gerenciar usuários e prestadores para manter a segurança operacional. |
| US15 | Como administrador, quero visualizar relatórios de solicitações, agendamentos e avaliações. |

#### Casos de Uso

| ID | Caso de uso | Ator principal | História relacionada |
|---|---|---|---|
| UC01 | Cadastrar cliente | Visitante | US01 |
| UC02 | Cadastrar prestador | Visitante | US02 |
| UC03 | Autenticar usuário | Visitante / Usuário | US03 |
| UC04 | Solicitar serviço | Cliente | US04 |
| UC05 | Consultar solicitações próprias | Cliente | US05 |
| UC06 | Consultar solicitações disponíveis | Prestador | US06 |
| UC07 | Enviar proposta | Prestador | US07 |
| UC08 | Selecionar proposta | Cliente | US08 |
| UC09 | Confirmar agendamento | Cliente / Prestador | US09 |
| UC10 | Cancelar agendamento | Cliente / Prestador | US10 |
| UC11 | Concluir atendimento | Prestador | US11 |
| UC12 | Avaliar atendimento | Cliente | US12 |
| UC13 | Gerenciar categorias | Administrador | US13 |
| UC14 | Gerenciar usuários | Administrador | US14 |
| UC15 | Consultar relatórios | Administrador | US15 |
| UC16 | Enviar notificação | Serviço de Notificação | US04, US07, US09, US10, US11 |

### 2.3 Diagrama de Sequência do Sistema e Contrato de Operações

Foram modelados três Diagramas de Sequência do Sistema, representando o Nubo como uma caixa-preta e demonstrando a interação externa entre ator e sistema.

| Diagrama | Caso de uso relacionado | Fonte PlantUML | Imagem |
|---|---|---|---|
| Diagrama UML de Sequência - UC04 Solicitar Serviço | UC04 | `codigos/diagrama-de-sequencia-sistema-solicitar-servico.puml` | `imagens/diagrama-de-sequencia-sistema-solicitar-servico.png` |
| Diagrama UML de Sequência - UC07 Enviar Proposta | UC07 | `codigos/diagrama-de-sequencia-sistema-enviar-proposta.puml` | `imagens/diagrama-de-sequencia-sistema-enviar-proposta.png` |
| Diagrama UML de Sequência - UC08 Selecionar Proposta | UC08 | `codigos/diagrama-de-sequencia-sistema-selecionar-proposta.puml` | `imagens/diagrama-de-sequencia-sistema-selecionar-proposta.png` |

Os contratos também são mantidos como arquivo de apoio em `docs/contratos-operacao.md`. O conteúdo consolidado dos contratos é apresentado a seguir para atender à estrutura do documento de projeto.

#### CO01 - solicitarServico(dadosSolicitacao)

| Campo | Descrição |
|---|---|
| Operação | `solicitarServico(dadosSolicitacao)` |
| Referências cruzadas | UC04, US04 |
| Ator principal | Cliente |
| Pré-condições | Cliente autenticado; categoria ativa; endereço informado; data preferencial futura. |
| Pós-condições | Solicitação criada com status `ABERTA`; endereço associado; histórico inicial registrado; notificação emitida para prestadores compatíveis. |

Efeitos: criar `SolicitacaoServico`, associar `Cliente`, `CategoriaServico` e `Endereco`, definir status `ABERTA`, registrar `HistoricoStatus` e disparar notificação.

#### CO02 - enviarProposta(dadosProposta)

| Campo | Descrição |
|---|---|
| Operação | `enviarProposta(dadosProposta)` |
| Referências cruzadas | UC07, US07 |
| Ator principal | Prestador |
| Pré-condições | Prestador autenticado; prestador ativo; solicitação em `ABERTA` ou `COM_PROPOSTAS`; categoria da solicitação vinculada ao prestador. |
| Pós-condições | Proposta criada com status `ENVIADA`; solicitação alterada para `COM_PROPOSTAS`; cliente notificado. |

Efeitos: validar compatibilidade entre prestador e categoria, criar `Proposta`, associar proposta à solicitação, atualizar status da solicitação, registrar histórico e notificar o cliente.

#### CO03 - selecionarProposta(idSolicitacao, idProposta)

| Campo | Descrição |
|---|---|
| Operação | `selecionarProposta(idSolicitacao, idProposta)` |
| Referências cruzadas | UC08, UC09, US08, US09 |
| Ator principal | Cliente |
| Pré-condições | Cliente autenticado; solicitação pertence ao cliente; proposta enviada está ativa; solicitação ainda não está agendada. |
| Pós-condições | Proposta selecionada; demais propostas recusadas; agendamento criado com status `CONFIRMADO`; solicitação alterada para `AGENDADA`; prestador e cliente notificados. |

Efeitos: validar propriedade da solicitação, validar disponibilidade da proposta, marcar proposta como `SELECIONADA`, recusar propostas concorrentes, criar `Agendamento`, atualizar `SolicitacaoServico` para `AGENDADA`, registrar histórico e disparar notificações.

#### CO04 - cancelarAgendamento(idAgendamento, motivo)

| Campo | Descrição |
|---|---|
| Operação | `cancelarAgendamento(idAgendamento, motivo)` |
| Referências cruzadas | UC10, US10 |
| Ator principal | Cliente ou Prestador |
| Pré-condições | Usuário autenticado; usuário participa do agendamento; agendamento não está concluído. |
| Pós-condições | Agendamento alterado para `CANCELADO`; motivo registrado; histórico atualizado; partes notificadas. |

Efeitos: validar participação do usuário no agendamento, validar cancelamento permitido, alterar status, registrar motivo e histórico, atualizar solicitação relacionada quando necessário e disparar notificação.

#### CO05 - concluirAtendimento(idAgendamento)

| Campo | Descrição |
|---|---|
| Operação | `concluirAtendimento(idAgendamento)` |
| Referências cruzadas | UC11, US11 |
| Ator principal | Prestador |
| Pré-condições | Prestador autenticado; prestador é responsável pelo agendamento; agendamento está `CONFIRMADO` ou `EM_ATENDIMENTO`. |
| Pós-condições | Agendamento alterado para `CONCLUIDO`; solicitação encerrada; cliente liberado para avaliar. |

Efeitos: validar responsabilidade do prestador, alterar status para `CONCLUIDO`, atualizar solicitação para `ENCERRADA`, registrar histórico e enviar notificação ao cliente solicitando avaliação.

## 3. Modelos de Projeto

Esta seção apresenta os modelos de projeto do Nubo. Os modelos detalham a arquitetura, os componentes, as classes, as interações internas, a comunicação entre objetos e os estados relevantes do sistema.

### 3.1 Arquitetura

O Nubo adota uma arquitetura cliente-servidor com comunicação via API REST. O backend é modelado como um monólito modular, isto é, uma única aplicação implantável organizada em módulos de negócio, como autenticação, solicitações, propostas, agendamentos, notificações e administração.

Internamente, o backend segue a Arquitetura Hexagonal, separando responsabilidades em interface, aplicação, domínio e infraestrutura. Essa organização mantém as regras de negócio protegidas de detalhes técnicos, como banco de dados, autenticação, documentação da API e integração com serviços externos de notificação.

A arquitetura foi escolhida considerando a evolução esperada do produto. Os fluxos de solicitação de serviços, envio de propostas, seleção de prestadores, agendamento, cancelamento, conclusão e avaliação tendem a crescer em regras, integrações e variações operacionais. Com o domínio isolado, o sistema pode trocar mecanismos de persistência, adicionar novos canais de entrada, substituir integrações externas e ampliar módulos internos sem comprometer o núcleo das regras de negócio.

O monólito modular mantém simplicidade operacional em uma primeira versão realista do produto, evitando a complexidade inicial de múltiplos serviços independentes. Ao mesmo tempo, a organização modular e hexagonal preserva fronteiras claras entre funcionalidades, facilitando manutenção, testes e possível decomposição futura caso o sistema aumente em escala.

A arquitetura do Nubo é representada por um Diagrama UML de Componentes, utilizado para demonstrar a organização macro dos módulos do sistema, suas responsabilidades e dependências. Esse diagrama complementa o Diagrama UML de Implantação, que apresenta a distribuição física dos componentes em ambiente de execução.

Arquivos:

- `codigos/diagrama-de-arquitetura.puml`
- `imagens/diagrama-de-arquitetura.png`
- `codigos/diagrama-de-pacotes.puml`
- `imagens/diagrama-de-pacotes.png`

### 3.2 Diagrama de Componentes e Implantação

O Diagrama UML de Componentes apresenta a arquitetura geral do backend Nubo, incluindo controladores REST, DTOs, serviços de aplicação, entidades de domínio, interfaces de repositório, implementações de repositório, integração de notificação externa e banco de dados.

O Diagrama UML de Implantação apresenta dispositivo do usuário, navegador, servidor da aplicação, servidor de banco de dados e serviço externo de notificação. Sua responsabilidade é demonstrar onde os componentes estarão alocados para execução.

Arquivos:

- `codigos/diagrama-de-componentes.puml`
- `imagens/diagrama-de-componentes.png`
- `codigos/diagrama-de-implantacao.puml`
- `imagens/diagrama-de-implantacao.png`

### 3.3 Diagrama de Classes

O diagrama de classes modela as principais entidades e relacionamentos do domínio: `Usuario`, `Cliente`, `Prestador`, `Administrador`, `CategoriaServico`, `Endereco`, `SolicitacaoServico`, `Proposta`, `Agendamento`, `Notificacao`, `Avaliacao` e `HistoricoStatus`.

Arquivos:

- `codigos/diagrama-de-classes.puml`
- `imagens/diagrama-de-classes.png`

### 3.4 Diagramas de Sequência

Os diagramas de sequência de projeto detalham a realização interna dos fluxos principais. Diferentemente dos Diagramas de Sequência do Sistema, estes mostram elementos internos como interface web, controladores, serviços de aplicação, repositórios, domínio, banco de dados e serviço externo de notificação.

Arquivos:

- `codigos/diagrama-de-sequencia-projeto-solicitar-servico.puml`
- `imagens/diagrama-de-sequencia-projeto-solicitar-servico.png`
- `codigos/diagrama-de-sequencia-projeto-selecionar-proposta.puml`
- `imagens/diagrama-de-sequencia-projeto-selecionar-proposta.png`

### 3.5 Diagramas de Comunicação

Os diagramas de comunicação representam as colaborações entre objetos e componentes para realização dos casos de uso. Eles complementam os diagramas de sequência ao destacar vínculos e mensagens entre os participantes.

Arquivos:

- `codigos/diagrama-de-comunicacao-solicitar-servico.puml`
- `imagens/diagrama-de-comunicacao-solicitar-servico.png`
- `codigos/diagrama-de-comunicacao-selecionar-proposta.puml`
- `imagens/diagrama-de-comunicacao-selecionar-proposta.png`

### 3.6 Diagramas de Estados

Os diagramas de estados representam o ciclo de vida de solicitações e agendamentos. Eles demonstram transições como abertura, recebimento de propostas, agendamento, cancelamento, conclusão e encerramento.

Arquivos:

- `codigos/diagrama-de-estados-solicitacao.puml`
- `imagens/diagrama-de-estados-solicitacao.png`
- `codigos/diagrama-de-estados-agendamento.puml`
- `imagens/diagrama-de-estados-agendamento.png`

## 4. Modelos de Dados

O modelo de dados do Nubo é representado por um esquema relacional compatível com PostgreSQL. O diagrama correspondente apresenta as tabelas, chaves primárias, chaves estrangeiras e relacionamentos usados para persistir os elementos centrais do domínio.

Arquivos:

- `codigos/diagrama-de-modelo-dados.puml`
- `imagens/diagrama-de-modelo-dados.png`

As entidades do domínio são mapeadas para tabelas relacionais, preservando identificadores, relacionamentos, estados e vínculos entre os principais fluxos do sistema. A entidade `Usuario` é persistida na tabela `usuarios`. Os perfis especializados `Cliente`, `Prestador` e `Administrador` são representados por tabelas próprias, relacionadas à tabela `usuarios` por chave primária e chave estrangeira compartilhada. Essa estratégia permite manter dados comuns de autenticação e identificação em `usuarios`, enquanto os dados específicos de cada perfil permanecem separados.

As relações um-para-muitos, como `Cliente` para `SolicitacaoServico`, `Prestador` para `Proposta` e `Usuario` para `Notificacao`, são representadas por chaves estrangeiras nas tabelas dependentes. A relação muitos-para-muitos entre `Prestador` e `CategoriaServico` é resolvida pela tabela associativa `prestador_categorias`.

Os estados de solicitação, proposta e agendamento são representados no modelo de objetos por enumerações e persistidos no banco como campos textuais controlados. Valores monetários, médias e avaliações utilizam tipos decimais; datas e horários utilizam campos temporais; descrições e justificativas utilizam campos textuais.

A tabela `historico_status` registra as mudanças de estado de solicitações e agendamentos, permitindo rastreabilidade das principais transições do sistema. Dessa forma, o modelo relacional preserva os principais conceitos do domínio orientado a objetos sem exigir que o banco de dados conheça diretamente classes, herança ou métodos.

## Histórico de Revisões

| Nome | Data | Razões para Mudança | Versão |
|---|---|---|---|
| Pedro Henrique Silva Vargas | 09 de junho de 2026 | Criação da documentação de projeto | 1.0 |
