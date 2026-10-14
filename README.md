# 📩 MailFlow AI — Automação Inteligente de E-mails

**Automação de atendimento por e-mail com Inteligência Artificial, utilizando Make, Google Gemini, Gmail e Google Drive.**

O **MailFlow AI** é um projeto de automação desenvolvido para otimizar o processamento de solicitações recebidas por e-mail. A solução combina inteligência artificial generativa, integração entre serviços e regras de roteamento para classificar mensagens, consultar uma base de conhecimento e gerar respostas contextualizadas automaticamente.

O projeto utiliza como cenário de aplicação o atendimento a dúvidas relacionadas ao **Oracle Fusion Cloud Applications**, com foco nos módulos HCM, Financials, SCM e Sales.

---

## 🎯 Objetivo

Automatizar etapas repetitivas do atendimento por e-mail, reduzindo a necessidade de classificação manual e agilizando a elaboração de respostas para dúvidas técnicas e funcionais.

A solução foi desenvolvida para:

- Monitorar automaticamente mensagens recebidas.
- Identificar solicitações relacionadas ao Oracle Cloud Applications.
- Classificar e-mails utilizando inteligência artificial.
- Direcionar mensagens por meio de regras condicionais.
- Consultar uma base de conhecimento armazenada no Google Drive.
- Gerar respostas contextualizadas com IA generativa.
- Enviar respostas automaticamente pelo Gmail.
- Organizar mensagens utilizando marcadores de atendimento.

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Aplicação no projeto |
|---|---|
| **Make** | Orquestração e automação dos processos |
| **Google Gemini AI** | Classificação de mensagens e geração de respostas |
| **Gmail** | Monitoramento, envio e organização de e-mails |
| **Google Drive** | Armazenamento e recuperação da documentação |
| **JSON** | Representação e exportação do fluxo de automação |
| **Markdown** | Documentação dos prompts e do projeto |

## 🏗️ Arquitetura da Solução

O MailFlow AI utiliza uma arquitetura de automação baseada em módulos conectados no Make.

O processamento começa com o monitoramento da caixa de entrada, seguido pela classificação inteligente das mensagens. Um roteador condicional determina o caminho de cada solicitação.

As mensagens identificadas como dúvidas Oracle seguem para consulta documental e geração de respostas, enquanto as mensagens fora do escopo são direcionadas para uma rota alternativa.

### Fluxo completo da automação

![Arquitetura do MailFlow AI](images/fluxo-completo.png)

## ⚙️ Funcionamento

### 1. Monitoramento de E-mails

**Módulo:** Gmail — Watch Emails

O processo é iniciado pelo monitoramento da caixa de entrada do Gmail, conforme o agendamento estabelecido no Make.

Na configuração utilizada, o módulo recupera até cinco mensagens por execução, respeitando os critérios e o ponto inicial de monitoramento.

As informações coletadas são encaminhadas para a etapa de classificação.

### 2. Classificação com Inteligência Artificial

**Módulo:** Google Gemini AI

O Gemini recebe o assunto e um trecho do conteúdo da mensagem e identifica se a solicitação está relacionada ao Oracle Cloud Applications.

A classificação utiliza duas possíveis respostas:

- **SIM:** mensagem relacionada aos assuntos Oracle definidos no prompt.
- **NÃO:** mensagem fora do escopo de atendimento automatizado.

Essa etapa utiliza instruções específicas para limitar a resposta do modelo às classificações esperadas.

📄 [Consultar prompt de classificação](prompts/classificacao-emails.md)

### 3. Roteamento Condicional

**Módulo:** Router — Make

O Router interpreta o resultado da classificação e direciona a mensagem para a rota correspondente.

**Rota SIM — Dúvida Oracle**

A mensagem segue para a recuperação da base de conhecimento, geração de resposta e envio automático.

**Rota NÃO — Atendimento Alternativo**

A mensagem segue para a rota destinada à organização de solicitações fora do escopo automatizado, permitindo sua identificação para acompanhamento humano.

### 4. Consulta à Base de Conhecimento

**Módulo:** Google Drive

O cenário recupera a documentação técnica utilizada como referência para o atendimento.

A base documental do projeto contempla materiais sobre:

| Módulo Oracle | Área de aplicação |
|---|---|
| Oracle Fusion Cloud HCM | Gestão de capital humano |
| Oracle Fusion Cloud Financials | Gestão financeira e contábil |
| Oracle Fusion Cloud SCM | Cadeia de suprimentos e manufatura |
| Oracle Fusion Cloud Sales | Gestão comercial e relacionamento com clientes |

**Implementação inicial:** a recuperação documental foi configurada utilizando um File ID específico do Google Drive, associado ao manual Oracle HCM.

A consulta dinâmica aos demais documentos é uma possibilidade de evolução da solução.

### 5. Geração Inteligente de Respostas

**Módulo:** Google Gemini AI

Após a recuperação da documentação, um segundo módulo Gemini recebe a solicitação original e o contexto disponibilizado pelo Google Drive.

O modelo é instruído a:

- Responder em português do Brasil.
- Utilizar linguagem profissional e objetiva.
- Limitar a resposta a quatro parágrafos.
- Utilizar exclusivamente o contexto fornecido.
- Indicar a origem das informações.
- Produzir conteúdo em HTML para melhorar a apresentação no e-mail.

Essa abordagem busca reduzir respostas sem fundamentação documental e padronizar a comunicação.

📄 [Consultar prompt de geração de respostas](prompts/geracao-respostas.md)

### 6. Envio Automático

**Módulo:** Gmail — Send an Email

A resposta produzida pelo Gemini é encaminhada automaticamente ao remetente da solicitação original.

O módulo utiliza:

- Endereço do remetente como destinatário.
- Assunto original com o prefixo `Re:`.
- Resposta gerada pelo Gemini como conteúdo HTML.

### 7. Organização dos Atendimentos

**Módulo:** Gmail — Move an Email

Após o processamento, o cenário utiliza o Gmail para organizar as mensagens em marcadores específicos.

Os marcadores definidos para o projeto são:

- **MailFlow AI:** organização das solicitações atendidas automaticamente.
- **Help Desk Human:** identificação de mensagens destinadas ao atendimento humano.

Essa organização facilita a separação dos atendimentos e o acompanhamento das solicitações.

---

## 🧠 Inteligência Artificial Aplicada

O projeto utiliza IA generativa em duas etapas distintas.

**Classificação de conteúdo**

O primeiro modelo interpreta o assunto e o conteúdo disponibilizado do e-mail para determinar sua relevância ao atendimento Oracle.

**Geração contextualizada**

O segundo modelo utiliza informações fornecidas pela base documental para elaborar respostas alinhadas ao contexto da solicitação.

A solução apresenta uma abordagem de geração fundamentada em documentos, semelhante ao princípio de *Retrieval-Augmented Generation (RAG)*.

A implementação de uma arquitetura RAG mais completa, com recuperação semântica de trechos e mecanismos de avaliação de relevância, permanece como possibilidade de evolução.

## 📁 Estrutura do Repositório

```text
mailflow-ai/
│
├── blueprints/
│   └── mailflow-ai.json
│
├── prompts/
│   ├── classificacao-emails.md
│   └── geracao-respostas.md
│
├── images/
│   └── fluxo-completo.png
│
└── README.md
```

**Descrição dos diretórios**

- `blueprints/`: estrutura exportada do cenário Make, com informações sensíveis removidas.
- `prompts/`: instruções utilizadas nos modelos de inteligência artificial.
- `images/`: registros visuais da arquitetura da automação.
- `README.md`: apresentação e documentação principal do projeto.

## 🚀 Como Reproduzir o Projeto

### Pré-requisitos

- Conta na plataforma Make.
- Conta Google com acesso ao Gmail e Google Drive.
- Conexão configurada com o Google Gemini AI.
- Documentação de referência armazenada no Google Drive.
- Permissões necessárias para leitura, envio e organização das mensagens.

### Configuração

1. Crie ou acesse uma conta no Make.
2. Importe o arquivo disponível em `blueprints/mailflow-ai.json`.
3. Configure suas próprias conexões com Gmail, Google Drive e Gemini.
4. Atualize os identificadores dos documentos utilizados.
5. Verifique os campos mapeados e os filtros das rotas.
6. Configure o agendamento do cenário.
7. Realize testes com mensagens fictícias antes de habilitar o envio automático.

**Observação:** o Blueprint representa a estrutura da automação, mas não substitui a configuração das conexões e permissões necessárias em cada ambiente.

## 🔒 Segurança e Boas Práticas

O projeto considera a importância da proteção das informações processadas durante o atendimento.

As principais recomendações incluem:

- Não armazenar credenciais ou chaves de API no repositório.
- Utilizar conexões autenticadas no Make.
- Restringir o acesso aos documentos de referência.
- Evitar a exposição de informações pessoais de remetentes.
- Revisar o conteúdo do Blueprint antes de publicá-lo.
- Validar respostas geradas por IA em cenários críticos.
- Manter mecanismos de tratamento de erros e rastreabilidade.

## 📈 Melhorias Futuras

O MailFlow AI pode evoluir com novas funcionalidades:

- **Consulta dinâmica:** identificação e recuperação automática dos documentos mais relevantes.
- **Recuperação semântica:** implementação de mecanismos de busca por trechos relevantes da documentação.
- **Validação de respostas:** identificação de informações insuficientes antes do envio automático.
- **Tratamento de erros:** mecanismos de repetição para falhas temporárias de APIs.
- **Monitoramento:** registro de volume de mensagens, classificações e falhas.
- **Indicadores de desempenho:** análise de tempo de resposta e eficiência da automação.
- **Atendimento híbrido:** encaminhamento estruturado para revisão humana.
- **Prevenção de duplicidade:** controle de mensagens já processadas.

## 📌 Considerações Finais

O MailFlow AI demonstra a aplicação prática de inteligência artificial generativa na automação de processos empresariais.

Por meio da integração entre Make, Gmail, Google Drive e Gemini, o projeto reúne classificação inteligente, roteamento condicional, consulta documental e geração automatizada de respostas em um único fluxo.

Além de explorar ferramentas *low-code*, a solução aborda conceitos de integração de sistemas, processamento de informações, automação de tarefas e utilização de dados como base para decisões automatizadas.

O projeto representa uma implementação inicial que pode ser expandida para cenários mais complexos de atendimento, suporte técnico e gestão de solicitações.

---

**Projeto desenvolvido para fins de aprendizado e demonstração prática de automação com inteligência artificial.**
