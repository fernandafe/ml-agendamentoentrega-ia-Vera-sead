# Vera: Guia SEAD-GO RAG (Retrieval-Augmented Generation)

##  Visão Geral

**Vera** é uma assistente de Inteligência Artificial especializada em fornecer informações precisas e objetivas sobre os procedimentos internos da Secretaria de Estado da Administração de Goiás (SEAD-GO).

Orquestrada através do **n8n** e utilizando a técnica de **RAG (Retrieval-Augmented Generation)**, Vera consulta uma base de conhecimento exclusiva de 30 manuais e documentos oficiais, garantindo que todas as respostas sejam estritamente baseadas em fontes internas e atualizadas.

### Principais Funcionalidades

*   **RAG com 30 Documentos:** Base de conhecimento robusta e restrita a 30 manuais de procedimentos da SEAD-GO.
*   **Orquestração n8n:** Fluxo de trabalho completo e automatizado, desde a entrada da mensagem até a resposta final.
*   **Interface Telegram:** Comunicação direta e amigável com o usuário final via Telegram.
*   **Multimodalidade:** Capacidade de processar e responder a mensagens de **texto**, **áudio** (transcrição via IA) e **imagem** (análise via IA).
*   **Memória Conversacional:** Utiliza **Redis** para manter o contexto da conversa, permitindo interações fluidas e contínuas.
*   **Text-to-Speech (TTS):** Respostas de texto podem ser convertidas em áudio via ElevenLabs e enviadas ao usuário.

## Arquitetura e Componentes

A arquitetura de Vera é modular e baseada em ferramentas de código aberto e serviços de IA, orquestrada pelo n8n.

| Componente | Função | Tecnologia/Serviço |
| :--- | :--- | :--- |
| **Orquestração** | Gerenciamento do fluxo de trabalho e lógica de negócios. | n8n |
| **Interface** | Ponto de contato com o usuário final. | Telegram |
| **Modelo de Linguagem (LLM)** | Geração de respostas e análise multimodal (áudio/imagem). | Google Gemini (via n8n LangChain Node) |
| **Base de Conhecimento (RAG)** | Fonte de dados exclusiva para as respostas. | 30 Manuais da SEAD-GO (integrados ao fluxo) |
| **Memória** | Armazenamento temporário do histórico de conversas. | Redis |
| **Voz (TTS)** | Conversão de texto em fala para respostas em áudio. | ElevenLabs |

## Base de Conhecimento (30 Manuais)

A assistente Vera é treinada exclusivamente nos seguintes 30 manuais de procedimentos da SEAD-GO. Seu escopo de conhecimento é estritamente limitado a estas fontes.

| Categoria | Documento |
| :--- | :--- |
| **Aposentadoria e Tempo** | Manual de Aposentadoria |
| | Averbação e Desaverbação de tempo de contribuição Modelo Rede Padrão DIVULGADO |
| | Vacância para assumir cargo inacumulável |
| **Férias e Licenças** | Manual de Férias |
| | Indenização de férias cargos da estrutura |
| | Licença-paternidade - Manual da Rede Padrão |
| | Licença capacitação - 3ª edição - Manual da Rede Padrão |
| | Licença por motivo de afastamento do cônjuge - Manual da Rede Padrão |
| | Licença para desempenho de mandato classista- Manual da Rede Padrão DIVULGADO |
| | Licença para tratar de interesses particulares - 2ª edição - Manual da Rede Padrão |
| | Licença Prêmio - 2ª Edição - Manual da Rede Padrão |
| | Licença Maternidade - Manual da Rede Padrão |
| | Redução de jornada para amamentação - DIVULGADO - Modelo da Rede Padrão |
| | Desincompatibilização e Licença por atividade política |
| **Frequência e Lotação** | Frequência DIVULGADO - Manual da Rede Padrão |
| | Inclusão e Encerramento nos Sistemas de Frequência - Manual da Rede Padrão |
| | Gestão dos Sistemas de Frequência - Manual da Rede Padrão |
| | Lotação de servidor - Manual da Rede Padrão |
| **Saúde e Benefícios** | Assistência Pré-escolar - Manual da Rede Padrão |
| | Auxilio funeral - Manual da Rede Padrão |
| | Exame Médico Periódico DIVULGADO - Manual da Rede Padrão |
| | Tratamento contínuo - Modelo da Rede Padrão |
| **Outros Procedimentos** | Manuel de GRG-PES e FCRG-PES |
| | Estágio - Manual da Rede Padrão |
| | Integração Salis e RHnet - Manual da Rede Padrão |
| | Redução de jornada de trabalho com redutor de remuneração |
| | Vale-transporte Celetista DIVULGADO - Manual da Rede Padrão |
| | Exoneração e rescisão a pedido - Manual da Rede Padrão |
| | Contrato Temporário - Modelo da Rede Padrão |
| | Missão Oficial - Modelo da Rede Padrão |
| | Acertos Financeiros - Manual da Rede Padrão |

## Persona e Regras de Negócio

A assistente Vera opera sob um conjunto rigoroso de regras de negócio e uma persona bem definida, garantindo a qualidade e a conformidade das informações.

| Aspecto | Descrição |
| :--- | :--- |
| **Nome/Cargo** | Vera (Guia SEAD-GO), Assistente Especialista. |
| **Tom de Voz** | Profissional, formal, objetivo, gentil e proativo. Comunicação concisa e amigável. |
| **Missão** | Dominar o conteúdo dos manuais para facilitar o acesso à informação e esclarecer dúvidas de forma precisa. |
| **Restrição de Escopo** | As respostas são baseadas **APENAS** no conteúdo dos 30 manuais listados. |
| **Obrigatoriedade** | **Sempre** deve citar a fonte (Manual e Seção) da informação. |
| **Exceções** | Possui tratamento para perguntas fora do escopo e para informações não encontradas nos manuais. |

## Configuração e Instalação

Para replicar o fluxo de trabalho de Vera, siga os passos abaixo:


### 1. Pré-requisitos

Você precisará dos seguintes serviços e ferramentas:

*   **n8n** (Instância própria ou Cloud)
*   **Redis** (Servidor de cache para memória conversacional)
*   **Conta Telegram** (Para criar o bot e obter o token)
*   **Credenciais de LLM** (Google Gemini, configurado no n8n)
*   **Credenciais de TTS** (ElevenLabs, configurado no n8n)

### 2. Importação do Workflow

1.  Baixe o arquivo `vera_n8n_workflow.json` (anexo a esta documentação).
2.  Na sua instância do n8n, clique em **"New"** e depois em **"Import from JSON"**.
3.  Selecione o arquivo JSON baixado.

### 3. Configuração de Credenciais

Após a importação, você precisará configurar as credenciais nos seguintes nós:

*   **Telegram Trigger / Telegram1/2/3/4:** Configure a credencial `Telegram account` com o token do seu bot.
*   **Redis1/2/3/4/5/6/7/8/9/10/Wait/Wait1/Wait2:** Configure a credencial `Redis account` com os dados de conexão do seu servidor Redis.
*   **Analyze voice message / Analyze image:** Configure a credencial `Google Gemini(PaLM) Api account`.
*   **Convert text to speech:** Configure a credencial `ElevenLabs account`.

### 4. Integração RAG (Documentos)

O nó `Manuais` no fluxo de trabalho contém a lista dos 30 documentos. **É crucial que você integre o conteúdo real desses documentos** ao seu sistema de RAG.

No fluxo original, o nó `Vera` (o Agent) está configurado para receber o conteúdo dos manuais. Você deve garantir que:

1.  Os 30 documentos listados na seção **Base de Conhecimento** sejam processados (chunking, embedding) e armazenados em um **Vector Store** acessível pelo n8n (ex: ChromaDB, Pinecone, Weaviate, ou até mesmo um nó de Vector Store do n8n).
2.  O nó `Vera` (Agent) utilize o **Vector Store Tool** para realizar a busca (Retrieval) antes de gerar a resposta (Generation).

O `systemMessage` do nó `Vera` já contém a lista dos manuais e as regras de negócio, mas a **integração do Vector Store** é o passo que garante o RAG.

## Arquivos

*   [`vera_n8n_workflow.json`](./vera_n8n_workflow.json): O arquivo JSON completo do fluxo de trabalho do n8n.

---
*Documentação gerada por Manus AI.*
