# Easy Support AI

Sistema inteligente de atendimento ao cliente baseado em Inteligência Artificial, utilizando técnicas de Processamento de Linguagem Natural (NLP), Recuperação de Informação com Geração Aumentada (Retrieval-Augmented Generation - RAG) e Modelos de Linguagem de Grande Porte (LLM) para fornecer respostas automatizadas e precisas às consultas dos clientes.

## 📋 Sumário

- [📋 Sumário](#-sumário)
- [📋 Visão Geral](#-visão-geral)
- [✨ Funcionalidades](#-funcionalidades)
- [🏗️ Arquitetura do Projeto](#-arquitetura-do-projeto)
- [🛠️ Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [📦 Instalação](#-instalação)
- [🚀 Como Usar](#-como-usar)
- [🧪 Testes](#-testes)
- [📁 Estrutura do Projeto](#-estrutura-do-projeto)
- [📚 Base de Conhecimento](#-base-de-conhecimento)
- [🔧 Contribuindo](#-contribuindo)
- [📄 Licença](#-licença)

## 📋 Visão Geral

O Easy Support AI é um sistema inteligente projetado para automatizar o atendimento ao cliente, fornecendo respostas precisas e contextualizadas às consultas dos usuários. O sistema combina técnicas avançadas de NLP, RAG e LLMs para:

1. **Entender a intenção** por trás das consultas dos usuários
2. **Recuperar informações relevantes** de uma base de conhecimento estruturada  
3. **Gerar respostas contextuais** e personalizadas usando LLMs
4. **Propor ações** quando apropriado (como abertura de chamados)
5. **Garantir conformidade** através do Modelo de Contexto (MCP)

O sistema é implementado como uma aplicação Streamlit que orquestra múltiplos componentes especializados através de um workflow baseado em LangGraph.

## ✨ Funcionalidades

- **Processamento de Linguagem Natural (NLP)**: Entendimento avançado de consultas em língua portuguesa
- **Classificação de Urgência**: Identifica automaticamente o nível de urgência (baixa, média, alta) das solicitações
- **Retrieval-Augmented Generation (RAG)**: Combina recuperação de informações com geração de texto
- **Base de Conhecimento**: Base estruturada de documentos para consulta rápida e precisa
- **Modelo de Contexto (MCP)**: Implementa o Model Context Protocol para melhorar coerência e segurança nas respostas
- **Geração de Respostas**: Cria respostas naturais, precisas e baseadas em evidências
- **Ações Governadas**: Pode propor ações como abertura de chamados, sujeitas à confirmação do usuário
- **Interface Streamlit**: Interface web intuitiva para interação com o sistema
- **API MCP**: Servidor para execução segura de ações externas
- **Sistema de Avaliação**: Conjunto de testes para validar a qualidade das respostas
- **Testes Automatizados**: Suite de testes para garantir qualidade e confiabilidade

## 🏗️ Arquitetura do Projeto

O projeto segue uma arquitetura modular com os seguintes componentes principais:

```
easy_support_ai/
├── app.py                    # Aplicação Streamlit principal
├── config.py                 # Configurações do projeto
├── requirements.txt          # Dependências do projeto
├── .gitignore                # Arquivos e diretórios ignorados pelo Git
├── classifier/               # Módulo de classificação de urgência
│   ├── train.py              # Script de treinamento do classificador
│   └── service.py            # Serviço de classificação
├── rag/                      # Módulo de Retrieval-Augmented Generation
│   ├── index_documents.py    # Script para indexar documentos
│   ├── retriever.py          # Módulo de recuperação de documentos
│   └── context_budget.py     # Gerenciamento de contexto para LLMs
├── generation/               # Módulo de geração de respostas
│   └── answer_service.py     # Serviço de geração de respostas
├── knowledge_base/           # Base de conhecimento
│   ├── exportacao_relatorios.md  # Documento de exemplo
│   └── politica_senhas.md        # Política de senhas
├── mcp_server/               # Servidor Modelo de Contexto (MCP)
│   ├── server.py             # Servidor MCP
│   └── client.py             # Cliente MCP
├── evaluation/               # Módulo de avaliação
│   └── rag_cases.json        # Casos de teste para avaliação RAG
├── workflow/                 # Definição do workflow
│   └── graph.py              # Grafo de workflow (LangGraph)
├── schemas.py                # Definições de esquemas de dados (Pydantic)
└── tests/                    # Testes automatizados
    └── test_workflow.py      # Testes do workflow
```

### Componentes Principais

1. **Classifier** (`classifier/service.py`)
   - Responsável por classificar a urgência das consultas (baixa, média, alta)
   - Utiliza um modelo de regressão logística com TF-IDF treinado em dados históricos
   - Retorna urgência, confiança e probabilidades para cada classe

2. **RAG (Retrieval-Augmented Generation)** (`rag/`)
   - `retriever.py`: Recupera documentos relevantes da base de conhecimento usando embeddings
   - `context_budget.py`: Gerencia o tamanho do contexto enviado aos LLMs para evitar overflow de tokens
   - `index_documents.py`: Processa e indexa documentos da base de conhecimento em um vector store (ChromaDB)

3. **Generation** (`generation/answer_service.py`)
   - Gera respostas naturais usando LLMs (OpenAI GPT-5.4-nano)
   - Baseia-se exclusivamente no contexto recuperado para evitar hallucinações
   - Cita fontes específicas das respostas
   - Declara necessidade de revisão humana quando a evidência é insuficiente

4. **Workflow** (`workflow/graph.py`)
   - Orquestra o fluxo completo de processamento usando LangGraph
   - Etapas: validação → classificação → recuperação → decisão → geração
   - Implementa lógica para determinar quando ações são necessárias (ex: abertura de chamados)

5. **MCP Server** (`mcp_server/`)
   - Implementa o Modelo de Contexto (Model Context Protocol)
   - Fornece ferramentas seguras para execução de ações externas
   - Atualmente implementa: criação e consulta de chamados (tickets)
   - Inclui políticas de escalonamento como recursos

6. **API** (`app.py`)
   - Interface Streamlit para interação com o sistema
   - Exibe histórico de conversas
   - Mostra fontes utilizadas, confiança da classificação e urgência sugerida
   - Solicita confirmação do usuário antes de executar ações propostas

## 🛠️ Tecnologias Utilizadas

- **Python 3.8+**: Linguagem principal
- **Streamlit**: Framework para aplicação web interativa
- **OpenAI API**: Para acesso ao modelo GPT-5.4-nano e embeddings
- **Scikit-learn**: Para classificação de urgência (Regressão Logística com TF-IDF)
- **Sentence Transformers**: Para geração de embeddings semânticos
- **ChromaDB**: Vector store para busca eficiente de documentos similares
- **LangChain/LangGraph**: Para orquestração de workflows e RAG
- **MCP (Model Context Protocol)**: Para integração segura com ferramentas externas
- **Pydantic**: Para validação e serialização de dados
- **Python-dotenv**: Para gerenciamento de variáveis de ambiente
- **Pytest**: Framework de testes
- **Joblib**: Para serialização de modelos de machine learning

## 📦 Instalação

### Pré-requisitos

- Python 3.8 ou superior
- pip (gerenciador de pacotes Python)
- Git (para clonar o repositório)
- Conta OpenAI com acesso ao modelo GPT-5.4-nano e embeddings

### Passo a Passo

1. **Clone o repositório**:
   ```bash
   git clone https://github.com/seu-usuario/easy_support_ai.git
   cd easy_support_ai
   ```

2. **Crie um ambiente virtual** (recomendado):
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   .\venv\Scripts\activate   # Windows
   ```

3. **Instale as dependências**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure as variáveis de ambiente**:
   ```bash
   cp .env.example .env
   ```
   Edite o arquivo `.env` com suas configurações:
   ```
   OPENAI_API_KEY=sua_chave_de_api_aqui
   OPENAI_MODEL=gpt-5.4-nano
   OPENAI_EMBEDDING_MODEL=text-embedding-3-small
   RAG_TOP_K=5
   RAG_MIN_SCORE=0.45
   MAX_CONTEXT_TOKENS=3500
   CLASSIFIER_CONFIDENCE_MIN=0.65
   ```

5. **Prepare a base de conhecimento**:
   ```bash
   python rag/index_documents.py
   ```

6. **Treine o classificador** (se necessário):
   ```bash
   python classifier/train.py
   ```
   Observação: O classificador já vem pré-treinado com os dados em `data/tickets_treino.csv`, mas você pode re-treinar se atualizar os dados.

## 🚀 Como Usar

### Iniciando a Aplicação

```bash
streamlit run app.py
```

A aplicação estará disponível em `http://localhost:8501`

### Interface do Usuário

1. Digite sua pergunta ou descrição do problema no campo de chat
2. O sistema processará sua solicitação através do workflow:
   - Validação da entrada
   - Classificação de urgência
   - Recuperação de conhecimento relevante
   - Decisão sobre necessidade de ação
   - Geração de resposta baseada em evidências
3. A resposta será exibida com:
   - Resposta em português brasileiro
   - Fontes utilizadas (quando disponíveis)
   - Urgência sugerida (baixa, média, alta)
   - Confiança da classificação (percentual)
   - Indicação se revisão humana é necessária
   - Ação proposta (quando aplicável)

### Exemplo de Uso

```bash
# Exemplo de interação:
Usuário: Como redefinir a senha?
Sistema: 
Resposta: Para redefinir sua senha, acesse o portal de autoatendimento e selecione "Esqueci minha senha"...
Fontes utilizadas: politica_senhas.md
Urgência sugerida: baixa
Confiança: 95.0%
Revisão humana necessária: Não
```

### Ações Governadas

Quando o sistema determinar que uma ação é necessária (ex: abertura de chamado para urgência alta), ele:

1. Exibe um aviso amarelo solicitando confirmação
2. Mostra os detalhes da ação proposta
3. Aguarda sua confirmação explícita antes de executar
4. Após confirmação, executa a ação através do servidor MCP seguro
5. Mostra o resultado da ação executada

## 🧪 Testes

Para executar os testes automatizados:

```bash
# Executar todos os testes
pytest tests/

# Executar com cobertura de código
pytest --cov=./ tests/

# Executar testes específicos
pytest tests/test_workflow.py -v
```

## 📁 Estrutura do Projeto Detalhada

### `app.py`
Ponto de entrada da aplicação. Configura e inicia a interface Streamlit com todos os componentes necessários.

### `config.py`
Contém todas as configurações do projeto, carregadas de variáveis de ambiente através do python-dotenv:
- `OPENAI_API_KEY`: Chave da API OpenAI
- `OPENAI_MODEL`: Modelo de linguagem a ser usado (padrão: gpt-5.4-nano)
- `OPENAI_EMBEDDING_MODEL`: Modelo de embeddings (padrão: text-embedding-3-small)
- `RAG_TOP_K`: Número de documentos a recuperar (padrão: 5)
- `RAG_MIN_SCORE`: Score mínimo de similaridade para recuperação (padrão: 0.45)
- `MAX_CONTEXT_TOKENS`: Limite máximo de tokens no contexto (padrão: 3500)
- `CLASSIFIER_CONFIDENCE_MIN`: Confiança mínima para classificação automática (padrão: 0.65)

### `classifier/`
Módulo responsável por entender a urgência por trás das consultas dos usuários.

#### `classifier/train.py`
Script para treinar o modelo de classificação usando:
- Dataset em `data/tickets_treino.csv` com colunas "texto" e "urgencia"
- Pipeline com TF-IDF + Regressão Logística
- Avaliação com métricas de F1-score e relatório de classificação
- Salva o modelo treinado em `classifier/urgency_model.joblib`

#### `classifier/service.py`
Serviço que carrega o modelo treinado e faz predições em tempo real:
- Retorna urgência prevista, confiança e probabilidades para cada classe
- Levanta exceção se o modelo não estiver treinado

### `rag/`
Implementa o pipeline de Retrieval-Augmented Generation:

#### `rag/retriever.py`
Responsável por recuperar os documentos mais relevantes para uma dada consulta:
- Usa embeddings semânticos para buscar no vector store ChromaDB
- Retorna os top-k documentos mais similares

#### `rag/context_budget.py`
Gerencia o tamanho do contexto enviado aos LLMs:
- Concatena documentos recuperados respeitando limites de tokens
- Prioriza documentos mais relevantes quando há necessidade de truncamento
- Retorna contexto formatado, documentos selecionados e contagem de tokens

#### `rag/index_documents.py`
Script para processar e indexar documentos da base de conhecimento:
- Lê todos os arquivos `.md` e `.txt` do diretório `knowledge_base/`
- Divide documentos em chunks usando RecursiveCharacterTextSplitter
- Gera embeddings e armazena no ChromaDB para busca eficiente

### `generation/`
Responsável por gerar respostas naturais e contextuais:

#### `generation/answer_service.py`
Serviço que combina o contexto recuperado com LLMs para gerar respostas finais:
- Usa OpenAI GPT-5.4-nano para geração de texto
- Segue instruções rigorosas para evitar hallucinações
- Cita somente fontes presentes no contexto fornecido
- Declara necessidade de revisão humana quando evidência é insuficiente
- Retorna resposta estruturada com metadata (urgência, confiança, etc.)

### `knowledge_base/`
Contém a base de conhecimento utilizada pelo sistema RAG:
- Documentos em formato Markdown (.md) ou texto puro (.txt)
- Cada documento deve incluir metadados YAML no início (opcional)
- Exemplos: `exportacao_relatorios.md`, `politica_senhas.md`

### `mcp_server/`
Implementação do Modelo de Contexto (Model Context Protocol) para melhorar a coerência e relevância das respostas:

#### `mcp_server/server.py`
Servidor MCP que fornece ferramentas seguras:
- `create_ticket`: Cria um chamado após aprovação explícita
- `get_ticket`: Consulta um chamado existente pelo ID
- Recurso `support://policy/escalation`: Fornece política de escalonamento
- Banco de dados SQLite para persistência de chamados

#### `mcp_server/client.py`
Cliente síncrono para chamar as ferramentas do MCP Server:
- Used by the workflow to execute approved actions
- Handles communication with the MCP server via stdio

### `evaluation/`
Contém conjuntos de dados e métricas para avaliar o desempenho do sistema RAG:

#### `evaluation/rag_cases.json`
Casos de teste para validação do sistema:
- Perguntas de exemplo com fontes esperadas
- Indicação se o sistema deve abstainer de responder (quando não há base de conhecimento suficiente)
- Exemplo:
  ```json
  [
    {
      "question": "Como redefinir a senha?",
      "expected_source": "politica_senhas.md",
      "must_abstain": false
    },
    {
      "question": "Qual é a cotação do dólar agora?",
      "expected_source": null,
      "must_abstain": true
    }
  ]
  ```

### `workflow/`
Orquestra o fluxo completo de processamento usando LangGraph:

#### `workflow/graph.py`
Define o grafo de workflow que conecta todos os componentes:
1. `validate`: Verifica se a entrada é válida
2. `classify`: Classifica a urgência da consulta
3. `retrieve`: Recupera documentos relevantes da base de conhecimento
4. `decide`: Determina se ação é necessária (baseado em confiança, urgência e documentos)
5. `generate`: Gera resposta final usando LLM
6. `failure`: Lida com erros durante o processamento

### `schemas.py`
Define os modelos de dados (Pydantic models) usados throughout the application:
- `SourceReference`: Referência a uma fonte utilizada na resposta
- `AssistantAnswer`: Estrutura da resposta final do assistente

### `tests/`
Contém os testes automatizados para garantir qualidade do código:
- `test_workflow.py`: Testes para o workflow completo

## 📚 Base de Conhecimento

A base de conhecimento está localizada no diretório `knowledge_base/`. Para adicionar novos conhecimentos:

1. Crie um novo arquivo Markdown (`.md`) ou texto (`.txt`) no diretório `knowledge_base/`
2. Escreva o conteúdo usando linguagem clara e estruturada
3. Opcionalmente, adicione metadados YAML no início do arquivo:
   ```yaml
   ---
   titulo: Título do Documento
   categoria: categoria
   versao: 1.0
   vigente_desde: 2026-01-01
   publico: interno
   ---
   ```
4. Re-indexe a base de conhecimento:
   ```bash
   python rag/index_documents.py
   ```

## 🔧 Contribuindo

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Faça commit das suas alterações (`git commit -m 'Add some AmazingFeature'`)
4. Faça push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

### Diretrizes de Contribuição

- Siga o estilo de código existente
- Escreva testes para novas funcionalidades
- Atualize a documentação conforme necessário
- Mantenha os commits atômicos e descritivos
- Todos os Pull Requests devem passar nos testes existentes

## 📄 Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 👥 Autores

- **Equipe Easy Support AI** - *Trabalho inicial* 

## 🙏 Agradecimentos

- Equipe de suporte que forneceu os dados de treinamento
- Comunidade open source pelas bibliotecas utilizadas
- Usuários beta que forneceram feedback valioso

---

