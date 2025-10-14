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
  * [Teorema CAP](#️-teorema-cap)
* [Comunicação entre Microserviços](#-comunicação-entre-microserviços)
* [Assincronia por Mensageria ou Evento](#-assincronia-por-mensageria-ou-evento)
* [Ecossistema Spring](#-ecossistema-spring)
* [Ferramentas Utilizadas no Projeto](#️-ferramentas-utilizadas-no-projeto)

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

<img width="500" height="150" alt="image" src="https://github.com/user-attachments/assets/850add37-3a01-427d-9a49-c4a7da4fe0c3" />



---

# ⚙️ Definição de Componentes e Comunicação

Não existe uma forma “melhor” de comunicação — **depende do negócio**.
No projeto EAD, serão utilizados **dois tipos de comunicação**.

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

## 🆔 Identificador Distribuído UUID

* São **únicos**
* **Evitam conflitos**
* **Gerado em diferentes bancos de dados**

---

## 🧰 Preocupações Transversais

Implementações que impactam vários microserviços.

**Padrões utilizados:**

* **API Gateway Pattern:** ponto único de entrada para requisições externas; faz o roteamento para cada microserviço.
* **Service Registry Discovery Pattern:** monitora os serviços registrados nele, controla endereços e instâncias ativas.
* **Global Config Management Pattern:** gerencia configurações de BD, mensageria e integração com APIs externas.
* **Circuit Breaker Pattern:** fornece resiliência à API em caso de falhas (funciona como um disjuntor).

---

## 📊 Observabilidade

Acompanha métricas, logs e eventos do sistema.

**Padrões utilizados:**
* **Log Aggregation Pattern:** centraliza logs em um único local, permitindo configurar alertas e filtros por microserviço.

---

## 🔒 Segurança

**Padrões utilizados:**
* **Access Token Pattern:** autenticação e autorização interna entre microserviços.

---

# 🗄️ Base de Dados: Compartilhada x Por Serviço

### 🔗 Compartilhada

* Pode gerar dependências fortes entre serviços.
* Dificulta a escalabilidade e a autonomia de times.
<img alt="image" src="https://github.com/user-attachments/assets/e7259f7c-0b63-487f-b1eb-09e0d164424e" width="500" height="400"/>

### 🔐 Por Serviço

* Cada serviço possui seu próprio banco.
* Favorece independência e escalabilidade.
<img alt="image" src="https://github.com/user-attachments/assets/39097f68-eec0-4fae-84f4-e530b4eae17f" width="500" height="400" />

---

## ⚖️ Teorema CAP

Não é possível garantir simultaneamente **Consistência**, **Disponibilidade** e **Particionamento**.
Nos **microserviços (que são serviços distribuídos)**, o particionamento é obrigatório — portanto, é preciso **priorizar entre consistência e disponibilidade**.

Na maioria dos casos, opta-se por:

* **Disponibilidade**, aceitando **consistência eventual** (os dados se tornam consistentes em atualizações futuras).
<img width="250" height="250" alt="image" src="https://github.com/user-attachments/assets/f891a943-83b4-4292-8713-21ddf8a237a7" />

---

# 🔗 Comunicação entre Microserviços

* **Síncrona:** espera retorno (bloqueante).
* **Assíncrona:** não bloqueante; pode ou não haver retorno.

> 💡 Não há um estilo melhor — depende das regras de negócio.

<img width="500" height="300" alt="image" src="https://github.com/user-attachments/assets/98d9b0c7-d6ab-44fd-bb93-aef0f0e1fa15" />

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
<img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/a9da904d-fa09-4617-ab93-dd1f7c24bb5c" />

---

# 🌱 Ecossistema Spring

O ecossistema **Spring** é composto por vários módulos e projetos prontos para uso.

* **Spring Framework:** base principal do ecossistema.
* **Spring Boot:** Spring Framework + servidor embutido (Tomcat ou Netty) — Configurações.

### Conceitos principais

* **Inversão de Controle (IoC):** padrão de projeto em que um objeto define suas dependências sem criá-las, delegando ao container IoC a tarefa de instanciá-las.
* **Injeção de Dependência :** mecanismo usado pelo Spring Framework para aplicar IoC quando necessário.
* **Beans:** objetos gerenciados pelo container do Spring por meio de IoC e Injeção de Dependência.

  * Reconhecimento via XML (`<bean>`) ou anotações:

    * `@Service`
    * `@Controller`
    * `@Repository`
    * `@Component`
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/1f4acf05-0358-415b-bc02-c4ac10547e4b" />


---

# 🛠️ Ferramentas Utilizadas no Projeto

## 🧩 Projetos Spring utilizados na aplicação EAD

* **Spring AMQP**
* **Spring Web MVC**
* **Spring Data JPA**
* **Spring Security**
* **Spring Log4j2**
* **Spring Boot**
* **Spring Validation**
* **Spring HATEOAS**
* **Spring Cloud Gateway**
* **Spring Cloud Config Server**
* **Spring Cloud Config Client**
* **Spring Cloud Netflix Eureka Server**
* **Spring Cloud Netflix Eureka Client**
* **Spring Cloud Resilience4J**

---

## 🗄️ Banco de Dados

* **PostgreSQL**

---

## 📨 Mensageria

* **RabbitMQ**

---

## 📋 Gerenciamento de Logs

* **Elasticsearch**

---

## ☕ Tecnologias

| Tecnologia           | Versão          |
| -------------------- | --------------- |
| **Java JDK**         | 17 (ou 11)      |
| **Spring Framework** | 6.2.11          |
| **Spring Boot**      | 3.5.6           |

---

