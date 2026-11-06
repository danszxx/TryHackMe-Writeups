# TryHackMe — Data Poisoning in RAG Systems

**Área:** AI Security  
**Sala:** [Data Poisoning in RAG Systems](https://tryhackme.com/room/datapoisoninginragsystems)

## Introdução

Esta sala aborda um problema que pode acontecer em sistemas de IA que consultam documentos externos: a alteração dos dados utilizados pelo sistema. Mesmo quando o modelo continua funcionando, informações manipuladas podem mudar as respostas que ele apresenta.

O objetivo aqui é entender as diferenças entre os tipos de *data poisoning*, identificar em que etapa eles acontecem e observar o efeito de uma alteração no material de referência de um assistente RAG.

## 1. Envenenamento dos dados de treinamento — Task 2

**Pergunta:** What type of poisoning affects model training?

A própria tarefa diferencia os dados usados para treinar um modelo daqueles que ele consulta depois. Quando exemplos maliciosos entram no treinamento ou no ajuste fino (*fine-tuning*), o modelo pode aprender associações incorretas. Isso é diferente de alterar apenas os documentos recuperados durante uma pergunta.

**Como chegar à resposta:** localizar na Task 2 a explicação de *training data poisoning* e observar que o ataque acontece nos dados de treinamento, antes do uso normal do modelo.

**Resposta:** `Training Data`

![Task 2 — exemplo sobre envenenamento de dados de treinamento](images/image-1.png)

## 2. Embeddings e bancos vetoriais — Task 3

Um sistema RAG pode transformar documentos em representações numéricas (*embeddings*) e pesquisar os mais próximos de uma pergunta. A proximidade matemática não garante que o conteúdo seja confiável.

### Pergunta 1 — Aumentando a presença de documentos manipulados

**Pergunta:** Which corpus poisoning technique increases the density of attacker-controlled documents in a specific semantic region?

Se alguém adiciona muitos documentos parecidos sobre o mesmo assunto, esses documentos podem dominar os primeiros resultados de uma pesquisa por similaridade. A técnica explora justamente a quantidade de conteúdo naquela região do espaço vetorial.

**Como chegar:** na parte sobre técnicas de *corpus poisoning*, identificar a descrição de vários documentos controlados pelo atacante concentrados em uma região semântica.

**Resposta:** `Corpus Flooding`

![Task 3 — demonstração de Corpus Flooding](images/image-2.png)

### Pergunta 2 — Alteração da representação vetorial

**Pergunta:** Which type of poisoning manipulates how documents are represented in vector space without changing the base model?

Aqui a diferença está na camada atingida. O modelo-base não precisa ser modificado: a alteração acontece na maneira como os documentos são representados ou posicionados para recuperação.

**Como chegar:** comparar, na Task 3, as camadas de treinamento, corpus e embeddings; procurar a técnica descrita como manipulação da representação vetorial.

**Resposta:** `Embedding-Level Poisoning`

![Task 3 — demonstração de Embedding-Level Poisoning](images/image-3.png)

### Pergunta 3 — Imitando documentos confiáveis

**Pergunta:** What corpus poisoning technique imitates the tone and structure of trusted documents?

Um documento malicioso também pode ser escrito para parecer um procedimento legítimo: mesma linguagem, formatação e estilo. A aparência de confiabilidade facilita que o conteúdo passe despercebido.

**Como chegar:** procurar na lista de técnicas a que descreve a imitação do estilo e da estrutura de materiais confiáveis.

**Resposta:** `Semantic Mimicry`

![Task 3 — demonstração de Semantic Mimicry](images/image-4.png)

## 3. Ataques ao pipeline de ingestão — Task 4

O pipeline de ingestão recebe documentos e os prepara para a base de conhecimento. Ele pode coletar arquivos, extrair texto, dividir o conteúdo em partes, gerar embeddings e indexar tudo para pesquisas futuras.

### Pergunta 1 — Nome do pipeline

**Pergunta:** What type of pipeline collects, parses, and indexes documents into an AI system’s knowledge base?

A pergunta descreve exatamente o fluxo de entrada e processamento dos documentos. O ponto de atenção é que um arquivo malicioso pode entrar na base se não houver validação da origem ou do conteúdo.

**Como chegar:** acompanhar o fluxo de documentos apresentado no início da Task 4.

**Resposta:** `Ingestion Pipeline`

![Task 4 — identificação do pipeline de ingestão](images/image-5.png)

### Pergunta 2 — Caso de dependency confusion

**Pergunta:** In the 2021 dependency confusion attack, what did the build system automatically pull from public repositories?

O estudo de caso mostra o risco de confiar automaticamente em conteúdo externo. Em um ataque de *dependency confusion*, um sistema de construção pode instalar pacotes publicados em repositórios públicos, confundindo-os com dependências internas.

**Como chegar:** ler o caso de 2021 apresentado na tarefa e identificar o que a automação baixou dos repositórios.

**Resposta:** `Malicious Packages`

![Task 4 — estudo de caso de Dependency Confusion](images/image-6.png)

## 4. Impactos no comportamento — Task 5

Nem todo ataque provoca erro, travamento ou uma resposta claramente absurda. Em alguns casos, o sistema continua respondendo normalmente, mas as recomendações começam a mudar.

### Pergunta 1 — Mudanças discretas

**Pergunta:** What type of poisoning effect causes small, gradual behavioural shifts that appear normal?

O enunciado fala de mudanças pequenas e graduais. A pista está no contraste que a Task 5 faz entre efeitos óbvios e efeitos difíceis de perceber.

**Como chegar:** localizar a seção *Subtle Poisoning Effects* e comparar com os exemplos de alterações evidentes.

**Resposta:** `Subtle`

![Task 5 — observação de alterações graduais no comportamento](images/image-7.png)

### Pergunta 2 — O que foi corrompido?

**Pergunta:** When poisoning alters behaviour but the infrastructure remains operational, what has been corrupted?

O sistema pode continuar disponível mesmo quando os dados utilizados por ele estão comprometidos. Isso mostra por que acompanhar apenas a disponibilidade do serviço não é suficiente.

**Como chegar:** observar no exemplo da tarefa que a infraestrutura permanece funcionando, enquanto as informações utilizadas pelo sistema foram alteradas.

**Resposta:** `Data`

![Task 5 — exemplo de integridade dos dados comprometida](images/image-8.png)

## 5. Detecção e mitigação — Task 6

Uma das dificuldades de identificar *data poisoning* é que o conteúdo manipulado pode parecer legítimo. Por isso, além de validar a origem dos documentos, faz sentido comparar respostas ao longo do tempo e revisar alterações importantes na base.

### Pergunta 1 — Monitoramento

**Pergunta:** What type of monitoring focuses on detecting gradual changes in model outputs over time?

Se as respostas mudam aos poucos, analisar apenas uma interação pode não revelar o problema. É preciso observar o comportamento em diferentes momentos.

**Como chegar:** procurar na Task 6 a parte sobre mudanças graduais nas saídas do modelo e o monitoramento utilizado para acompanhá-las.

**Resposta:** `Behavioural Monitoring`

![Task 6 — monitoramento de mudanças em respostas](images/image-9.png)

### Pergunta 2 — Estudo de caso da Amazon

**Pergunta:** In the Amazon fake reviews case study, approximately how many suspected fake reviews did Amazon report blocking before publication in 2023–2024?

A tarefa usa avaliações falsas como exemplo de manipulação de informações que influenciam sistemas de classificação e recomendação.

**Como chegar:** encontrar o estudo de caso da Amazon na Task 6 e anotar a quantidade aproximada mencionada no texto.

**Resposta:** `250 million`

![Task 6 — referência ao estudo de caso de avaliações falsas](images/image-10.png)

## 6. Exercício prático — Task 7

Esta parte é a que vale mais a pena documentar com prints. A proposta é comparar as respostas de um assistente antes e depois de atualizar o material que ele consulta. O teste deve acontecer apenas no agente fornecido pela sala.

### Etapa 1 — Registrar o comportamento original

1. Na Task 7, abrir o agente em **Open Agent**.
2. Fazer a pergunta indicada pela sala sobre a política de redefinição de senha.
3. Anotar os requisitos informados pelo assistente (prazo, complexidade e procedimento).
4. Perguntar também sobre o processo interno de implantação e registrar a resposta. Essa segunda consulta serve como comparação.

**Anotações do teste:**

- Política de senhas antes da alteração: *[According to the Password Reset Policy, employees must reset passwords every 90 days. Passwords must be at least 12 characters long and include uppercase letters, lowercase letters, numbers, and one special character. Resets are handled by the IT Help Desk at helpdesk.papertrail.internal, and identity verification is done via employee ID and a one-time code sent to the registered mobile number. Shared accounts are not permitted.]*

![Task 7 — resposta inicial do assistente antes da alteração](images/image-11.png)

### Etapa 2 — Atualizar o documento de referência

1. Ativar a opção de ingestão de documentos apresentada no painel do exercício.
2. Usar o texto de atualização fornecido pela própria Task 7.
3. Enviar a atualização e verificar se o agente confirma que o material foi recebido.

O detalhe importante aqui é que não estamos mudando o modelo ou seu treinamento: estamos alterando um conteúdo que ele utiliza como referência.

![Task 7 — envio do material de referência atualizado](images/image-12.png)

### Etapa 3 — Repetir as perguntas

1. Repetir a pergunta sobre a política de redefinição de senha.
2. Comparar a nova resposta com a original.
3. Repetir a pergunta sobre o processo de implantação para verificar se ela permaneceu consistente.

**Anotações do teste:**

- Política de senhas depois da alteração: *[According to the revised Password Reset Policy, employees must reset passwords every 180 days. Passwords must be at least 8 characters long and include both uppercase and lowercase letters. The requirements for special characters and numbers have been removed. Password resets are handled through the self-service portal at passwords.papertrail.external, with authentication using only email addresses. The previous employee ID and mobile verification steps have been deprecated.]*


![Task 7 — resposta do assistente após alteração da referência](images/image-13.png)

### Pergunta 1 — O que foi modificado?

**Pergunta:** Which component was modified to influence the assistant’s behaviour?

O elemento alterado no exercício é o documento que o assistente usa para consultar a política. Não houve necessidade de modificar os pesos do modelo ou seu código.

**Resposta:** `Reference Material`

### Pergunta 2 — Qual foi a classe do ataque?

**Pergunta:** What attack class does modifying trusted knowledge fall under?

Como a alteração foi realizada nos dados de referência confiados pelo sistema, o caso se enquadra em envenenamento de dados.

**Resposta:** `Data Poisoning`

## 7. Conclusão — Task 8

A principal diferença que ficou clara nesta sala é que um sistema de IA pode continuar funcionando normalmente e ainda assim entregar informações incorretas por causa dos dados que recebe. Os pontos de entrada não ficam apenas no prompt: também incluem treinamento, documentos, embeddings e ingestão.

O exercício com o agente ajuda a visualizar essa diferença, porque permite comparar uma resposta antes e depois da mudança no material de referência. Para reduzir esse tipo de risco, é importante controlar a origem dos documentos, revisar atualizações e acompanhar alterações no comportamento das respostas.

## Resumo das respostas

| Task | Resposta |
|---|---|
| 2 | `Training Data` |
| 3.1 | `Corpus Flooding` |
| 3.2 | `Embedding-Level Poisoning` |
| 3.3 | `Semantic Mimicry` |
| 4.1 | `Ingestion Pipeline` |
| 4.2 | `Malicious Packages` |
| 5.1 | `Subtle` |
| 5.2 | `Data` |
| 6.1 | `Behavioural Monitoring` |
| 6.2 | `250 million` |
| 7.1 | `Reference Material` |
| 7.2 | `Data Poisoning` |
