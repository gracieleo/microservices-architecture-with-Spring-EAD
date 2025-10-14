
# 🧩 Arquitetura de Microserviços

## 📚 Sumário

* [Concepção da Arquitetura](#-concepção-da-arquitetura)

  * [Granularidade](#-granularidade)
* [Definição de Componentes e Comunicação](#️-definição-de-componentes-e-comunicação)

  * [Comunicação Síncrona via HTTP](#-comunicação-síncrona-via-http)
  * [Comunicação Assíncrona via Mensagens e Eventos](#-comunicação-assíncrona-via-mensagens-e-eventos)
  * [Identificador Distribuído](#-identificador-distribuído)
  * [Preocupações Transversais](#-preocupações-transversais)
  * [Observabilidade](#-observabilidade)
  * [Segurança](#-segurança)
* [Base de Dados: Compartilhada x Por Serviço](#️-base-de-dados-compartilhada-x-por-serviço)

  * [Teorema CAP](#-teorema-cap)
* [Comunicação entre Microserviços](#-comunicação-entre-microserviços)
* [Assincronia por Mensageria ou Evento](#-assincronia-por-mensageria-ou-evento)

---

# 🧩 Concepção da Arquitetura

A **arquitetura de microserviços** envolve conceitos e princípios que devem ser sempre adaptados ao negócio.
Não é um modelo fixo nem um padrão pronto e replicável.

---

## 🎯 Granularidade

Define **como uma aplicação pode ser dividida em microserviços**.
Evite os seguintes extremos:

* **Granularidade alta:** muitas regras em um único serviço.
* **Granularidade fina:** serviços muito pequenos (ex: CRUD simples).

> 💡 Exemplo: um monólito de pagamentos pode ser transformado em vários microserviços.

### Passo 1 – Quebra das regras de negócio nos seguintes serviços:

* `Authuser`
* `Course`
* `Notification`

### Passo 2 – Uso de **base de dados por serviço**

---

# ⚙️ Definição de Componentes e Comunicação

Não existe uma forma “melhor” de comunicação — **depende do negócio**.
No caso de um sistema EAD, serão utilizados **dois tipos de comunicação**.

---

## 🔁 Comunicação Síncrona via HTTP

* **API Restful:** cada microserviço expõe seus recursos para serem consumidos por outros.
* **API Composition Pattern**

---

## 📬 Comunicação Assíncrona via Mensagens e Eventos

Usada para reduzir acoplamento entre os microserviços (não bloqueante).

**Padrões utilizados:**

* **Broken Pattern**
* **Mediator Pattern**
* **Event Notification Pattern**
* **Event Carried State Transfer Pattern**
* **Saga Pattern**

---

## 🆔 Identificador Distribuído

* Deve ser **único**
* **Evita conflitos**
* **Gerado em diferentes bancos de dados**

---

## 🧰 Preocupações Transversais

Implementações que impactam vários microserviços.

**Padrões utilizados:**

* **API Gateway Pattern:** ponto único de entrada para requisições externas; faz o roteamento para cada microserviço.
* **Service Registry & Discovery Pattern:** monitora os serviços registrados, controla endereços e instâncias ativas.
* **Global Config Management Pattern:** gerencia configurações de BD, mensageria e integração com APIs externas.
* **Circuit Breaker Pattern:** fornece resiliência à API em caso de falhas (funciona como um disjuntor).

---

## 📊 Observabilidade

Acompanha métricas, logs e eventos do sistema.

* **Log Aggregation Pattern:** centraliza logs em um único local, permitindo configurar alertas e filtros por microserviço.

---

## 🔒 Segurança

* **Access Token Pattern:** autenticação e autorização interna entre microserviços.

---

# 🗄️ Base de Dados: Compartilhada x Por Serviço

### 🔗 Compartilhada

* (Descrição ou uso conforme o caso)

### 🔐 Por Serviço

* (Isolamento de dados e autonomia por microserviço)

---

## ⚖️ Teorema CAP

Não é possível garantir simultaneamente **Consistência**, **Disponibilidade** e **Particionamento**.
Nos **microserviços (serviços distribuídos)**, o particionamento é obrigatório — portanto, é preciso **priorizar entre consistência e disponibilidade**.

Na maioria dos casos, opta-se por:

* **Disponibilidade**, aceitando **consistência eventual** (os dados se tornam consistentes em atualizações futuras).

---

# 🔗 Comunicação entre Microserviços

* **Síncrona:** espera retorno (bloqueante).
* **Assíncrona:** não bloqueante; pode ou não haver retorno.

> 💡 Não há um estilo melhor — depende das regras de negócio.

---

# 📨 Assincronia por Mensageria ou Evento

Uma **mensagem** é um pedido genérico, com *header* e *body*.
Deve expressar uma **intenção** de comunicação.

Tipos:

* **Comando:** solicitação para “fazer algo”
* **Evento:** notificação de que “algo aconteceu”

**Fluxos comuns:**

* `Producer → Consumer`
* `Publisher → Subscriber`

