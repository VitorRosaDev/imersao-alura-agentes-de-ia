# Agente de Service Desk com RAG e LangGraph (Imersão IA Alura + Google)

![Alura + Google](https://img.shields.io/badge/Imersão%20IA-Alura%20%2B%20Google-blue.svg)
![LangChain](https://img.shields.io/badge/LangChain-0.1-green.svg)
![Google Gemini](https://img.shields.io/badge/Google-Gemini-orange.svg)
![LangGraph](https://img.shields.io/badge/LangGraph-Stateful%20Agents-purple.svg)

## 📖 Sobre o Projeto

Este repositório é o resultado do projeto desenvolvido durante a **Imersão IA da Alura em parceria com o Google**. O objetivo foi construir um agente de IA para automação de um Service Desk de uma empresa fictícia ("Carraro Desenvolvimento"), capaz de entender, classificar e responder a solicitações de funcionários com base nas políticas internas da empresa.

O agente utiliza o poder do modelo **Google Gemini** através do ecossistema **LangChain** para criar um fluxo de trabalho inteligente e autônomo, orquestrado com **LangGraph**.

## ✨ Funcionalidades Principais

O agente implementa um fluxo completo de atendimento, dividido em três etapas principais:

1.  **Triagem Inteligente**: Utiliza um LLM para analisar a mensagem do usuário e classificá-la em uma de três categorias:
    * `AUTO_RESOLVER`: A dúvida pode ser respondida com base nos documentos de políticas.
    * `PEDIR_INFO`: A mensagem é vaga e precisa de mais detalhes.
    * `ABRIR_CHAMADO`: A solicitação é um pedido de exceção ou aprovação que requer a abertura de um ticket.

2.  **Base de Conhecimento com RAG (Retrieval-Augmented Generation)**:
    * Documentos PDF com as políticas da empresa são carregados e processados.
    * Um banco de dados vetorial (FAISS) é criado para permitir buscas semânticas rápidas.
    * Quando uma pergunta é classificada como `AUTO_RESOLVER`, o sistema busca os trechos mais relevantes nos documentos e os utiliza como contexto para que o LLM formule uma resposta precisa e fundamentada.

3.  **Orquestração com LangGraph**:
    * Um grafo de estados (state machine) é construído para gerenciar todo o fluxo de decisão do agente.
    * O agente navega entre os nós (triagem, auto-resolver, pedir info, abrir chamado) de forma condicional, com base nos resultados de cada etapa, criando um sistema robusto e com múltiplos caminhos de resolução.

## 🛠️ Tecnologias Utilizadas

* **Google Gemini**: Modelo de linguagem generativa utilizado para as tarefas de classificação e geração de respostas.
* **LangChain**: Framework principal para desenvolvimento de aplicações com LLMs, utilizado para criar as cadeias (chains) e integrar os componentes.
* **LangGraph**: Biblioteca para construir agentes robustos e cíclicos, orquestrando a lógica de estados do Service Desk.
* **Pydantic**: Para garantir que a saída do LLM na etapa de triagem seja um JSON estruturado e válido.
* **FAISS (Facebook AI Similarity Search)**: Para criar o banco de dados vetorial em memória.
* **PyMuPDF**: Para carregar e extrair o texto dos documentos de políticas em formato PDF.
* **Google Colab**: Ambiente de desenvolvimento utilizado para executar o notebook.

##  flowchart

O fluxo de decisão do agente pode ser visualizado no seguinte diagrama, gerado pelo LangGraph:

![Diagrama do Grafo do Agente](https://i.imgur.com/u7r3F7k.png) 
*Observação: Substitua a URL acima pela imagem do grafo gerada no seu notebook para uma representação exata.*

## 🚀 Como Executar

1.  **Pré-requisitos**:
    * Uma conta Google para acessar o Google Colab.
    * Uma chave de API do Google AI Studio (antigo MakerSuite) para usar o modelo Gemini.

2.  **Configuração**:
    * Abra o notebook no Google Colab.
    * No menu lateral esquerdo, clique no ícone de chave (🔑) para abrir a aba "Secrets".
    * Crie um novo secret com o nome `GEMINI_API_KEY` e cole sua chave de API no campo de valor.

3.  **Preparação dos Documentos**:
    * Este projeto utiliza 3 arquivos PDF de políticas internas como base de conhecimento. Faça o upload desses arquivos para a raiz do ambiente do Colab.
        * `Política de Uso de E-mail e Segurança da Informação.pdf`
        * `Política de Reembolsos (Viagens e Despesas).pdf`
        * `Políticas de Home Office.pdf`

4.  **Instalação das Dependências**:
    * Execute a primeira célula de cada "Aula" para instalar as bibliotecas necessárias:
      ```python
      # Aula 1 e 2
      !pip install -q --upgrade langchain langchain-google-genai google-generativeai langchain_community faiss-cpu langchain-text-splitters pymupdf
      
      # Aula 3
      !pip install -q --upgrade langgraph
      ```

5.  **Execução**:
    * Execute todas as células do notebook em ordem. Os resultados dos testes para cada etapa serão exibidos no final.

## 📂 Estrutura do Código

O notebook está dividido em três seções principais, refletindo a evolução do projeto:

* **Aula 1**: Focada na criação do `triagem_chain`, que usa o LLM com `with_structured_output` para classificar as mensagens dos usuários de forma confiável.
* **Aula 2**: Implementa o pipeline de RAG, desde o carregamento e divisão dos documentos até a criação do `retriever` com FAISS e a cadeia `document_chain` para responder perguntas com base no contexto.
* **Aula 3**: Une os componentes das aulas anteriores em um único agente coeso usando `LangGraph`, definindo os nós de ação e as arestas condicionais que ditam o fluxo de trabalho.
