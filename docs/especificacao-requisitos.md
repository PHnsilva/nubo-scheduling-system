# Especificação de Requisitos e Visão de Domínio - Nubo

**Sistema:** Nubo - Plataforma de Agendamento e Gestão de Serviços Locais  
**Versão:** 1.0  
**Projeto elaborado por:** Pedro Henrique Silva Vargas, como parte da disciplina Projeto de Software.  
**Data de criação:** 09 de junho de 2026  

## 1. Visão Geral

O **Nubo** é uma plataforma digital para solicitação, proposta, agendamento e acompanhamento de serviços locais. A solução conecta clientes que precisam contratar serviços a prestadores capazes de analisar demandas, enviar propostas e executar atendimentos agendados.

O sistema busca centralizar o fluxo de contratação de serviços, reduzindo comunicação informal, perda de informações e falta de rastreabilidade entre solicitação, proposta, agendamento, conclusão e avaliação.

Este documento é a referência principal para a descrição geral do problema, domínio, requisitos funcionais, requisitos não funcionais e regras de negócio utilizados na documentação de projeto do sistema Nubo.

## 2. Descrição Geral do Problema

Em contratações locais de serviços, é comum que clientes e prestadores negociem por canais informais, sem organização de propostas, histórico de status, confirmação estruturada de agenda ou registro de conclusão. Esse cenário dificulta comparação de propostas, acompanhamento do atendimento, responsabilização das partes e avaliação do serviço prestado.

O Nubo resolve esse problema ao oferecer um fluxo estruturado: o cliente registra uma solicitação, prestadores compatíveis enviam propostas, o cliente seleciona uma proposta, o sistema gera o agendamento, o atendimento é executado, concluído e avaliado. O sistema também mantém registros de notificação e histórico de mudanças de estado.

## 3. Visão de Domínio do Sistema

A visão de domínio do Nubo é organizada em torno da contratação de serviços locais. O conceito central é a `SolicitacaoServico`, criada por um `Cliente` e vinculada a uma `CategoriaServico` e a um `Endereco`. Prestadores compatíveis com a categoria podem responder à solicitação por meio de `Proposta`.

Quando o cliente escolhe uma proposta, o sistema cria um `Agendamento`, vinculando cliente, prestador, solicitação e proposta selecionada. Durante o ciclo de vida da solicitação e do agendamento, o sistema registra mudanças relevantes em `HistoricoStatus` e emite `Notificacao` para os usuários envolvidos. Após a conclusão, o cliente pode registrar uma `Avaliacao` do atendimento.

O domínio é composto pelos seguintes conceitos principais:

| Conceito | Papel no domínio |
|---|---|
| Usuario | Representa a identidade base utilizada para autenticação, perfil e contato. |
| Cliente | Especialização de usuário que cria solicitações e seleciona propostas. |
| Prestador | Especialização de usuário que atende categorias de serviço e envia propostas. |
| Administrador | Especialização de usuário responsável por cadastros e acompanhamento administrativo. |
| CategoriaServico | Classifica os tipos de serviços oferecidos e solicitados. |
| Endereco | Representa o local de atendimento da solicitação. |
| SolicitacaoServico | Núcleo do fluxo de contratação, contendo descrição, categoria, endereço e status. |
| Proposta | Resposta de um prestador a uma solicitação, contendo valor, data, horário e observações. |
| Agendamento | Confirmação operacional derivada da proposta escolhida. |
| Notificacao | Registro de comunicação enviada em eventos relevantes. |
| Avaliacao | Registro da percepção do cliente após a conclusão do serviço. |
| HistoricoStatus | Registro das transições de estado de solicitações e agendamentos. |

## 4. Escopo do Sistema

O escopo do Nubo contempla:

- cadastro e autenticação de usuários;
- diferenciação entre cliente, prestador e administrador;
- cadastro de categorias de serviço;
- criação de solicitações de serviço por clientes;
- consulta de solicitações disponíveis por prestadores;
- envio de propostas por prestadores;
- seleção de proposta pelo cliente;
- geração de agendamento;
- cancelamento e conclusão de atendimentos;
- envio de notificações;
- avaliação do atendimento;
- acompanhamento de histórico de status.

Não faz parte do escopo atual:

- processamento financeiro real;
- integração com meios de pagamento;
- execução operacional do serviço fora da plataforma;
- emissão fiscal;
- chat em tempo real;
- geolocalização avançada ou roteirização.

## 5. Atores

| Ator | Descrição |
|---|---|
| Visitante | Pessoa ainda não autenticada que pode iniciar cadastro ou login. |
| Cliente | Usuário que solicita serviços, analisa propostas, seleciona prestador, acompanha agendamentos e avalia atendimentos. |
| Prestador | Usuário que visualiza solicitações compatíveis, envia propostas, acompanha agendamentos e conclui atendimentos. |
| Administrador | Usuário responsável por acompanhar cadastros, categorias, solicitações, prestadores e registros administrativos. |
| Serviço de Notificação | Sistema externo responsável pelo envio de notificações relacionadas aos principais eventos do fluxo. |

## 6. Regras de Negócio

| ID | Regra |
|---|---|
| RN01 | Uma solicitação de serviço deve estar vinculada a um cliente. |
| RN02 | Uma solicitação deve possuir categoria, descrição e endereço de atendimento. |
| RN03 | Um prestador só deve enviar proposta para solicitações compatíveis com suas categorias de atuação. |
| RN04 | Uma solicitação pode receber múltiplas propostas. |
| RN05 | O cliente pode selecionar apenas uma proposta para gerar o agendamento. |
| RN06 | A seleção de uma proposta altera o estado da solicitação e cria um agendamento associado. |
| RN07 | Solicitações e agendamentos devem manter histórico de mudanças de status. |
| RN08 | O cancelamento deve registrar motivo ou justificativa quando aplicável. |
| RN09 | A avaliação só pode ser registrada após a conclusão do atendimento. |
| RN10 | O sistema deve enviar notificações nos principais eventos do fluxo, como criação de solicitação, envio de proposta, seleção, cancelamento e conclusão. |

## 7. Requisitos Funcionais

| ID | Requisito Funcional |
|---|---|
| RF01 | O sistema deve permitir o cadastro de usuários. |
| RF02 | O sistema deve permitir autenticação de usuários. |
| RF03 | O sistema deve permitir a classificação de usuários como cliente, prestador ou administrador. |
| RF04 | O sistema deve permitir que clientes criem solicitações de serviço. |
| RF05 | O sistema deve permitir que prestadores consultem solicitações compatíveis com suas categorias. |
| RF06 | O sistema deve permitir que prestadores enviem propostas para solicitações abertas. |
| RF07 | O sistema deve permitir que clientes visualizem propostas recebidas. |
| RF08 | O sistema deve permitir que clientes selecionem uma proposta. |
| RF09 | O sistema deve gerar um agendamento a partir da proposta selecionada. |
| RF10 | O sistema deve permitir cancelamento de solicitações ou agendamentos conforme o estado atual. |
| RF11 | O sistema deve permitir que prestadores concluam atendimentos. |
| RF12 | O sistema deve permitir que clientes avaliem atendimentos concluídos. |
| RF13 | O sistema deve registrar histórico de mudanças de status. |
| RF14 | O sistema deve permitir envio de notificações relacionadas aos principais eventos. |
| RF15 | O sistema deve permitir que administradores acompanhem usuários, categorias, solicitações e prestadores. |

## 8. Requisitos Não Funcionais

| ID | Requisito Não Funcional |
|---|---|
| RNF01 | O sistema deve organizar o backend como monólito modular, mantendo separação clara entre módulos de negócio. |
| RNF02 | O backend deve seguir Arquitetura Hexagonal, isolando regras de negócio de detalhes de infraestrutura. |
| RNF03 | A comunicação entre frontend e backend deve ocorrer por API REST. |
| RNF04 | O sistema deve persistir dados em banco relacional PostgreSQL. |
| RNF05 | O sistema deve manter rastreabilidade das principais mudanças de estado. |
| RNF06 | O sistema deve permitir evolução de integrações externas sem alteração direta nas regras de negócio. |
| RNF07 | O sistema deve preservar consistência entre solicitação, proposta selecionada e agendamento gerado. |
| RNF08 | O sistema deve proteger operações administrativas por controle de perfil. |
| RNF09 | O sistema deve utilizar nomenclatura consistente entre documentação, diagramas, entidades e modelo de dados. |
| RNF10 | O sistema deve favorecer testabilidade das regras de negócio por meio do isolamento do domínio. |

## 9. Estados Relevantes do Domínio

| Elemento | Estados principais |
|---|---|
| SolicitacaoServico | ABERTA, COM_PROPOSTAS, AGENDADA, CANCELADA, ENCERRADA |
| Proposta | ENVIADA, SELECIONADA, RECUSADA, EXPIRADA |
| Agendamento | CONFIRMADO, EM_ATENDIMENTO, CONCLUIDO, CANCELADO |
| Notificacao | PENDENTE, ENVIADA, FALHA |
