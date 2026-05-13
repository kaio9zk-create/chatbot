# chatbot
Projeto acadêmico de desenvolvimento de um chatbot inteligente para interação automatizada com usuários.
!pip install langchain langchain-community langchain-groq youtube-transcript-api

import os

from langchain_community.document_loaders import WebBaseLoader 

from langchain_groq import ChatGroq

from langchain_core.prompts import ChatPromptTemplate



# Configuração

os.environ['USER_AGENT'] = 'MeuBot/1.0 (aula-chatbot)'

os.environ['GROQ_API_KEY'] = 'COLOQUE SUA CHAVE API'



# Modelo

chat = ChatGroq(model='llama-3.3-70b-versatile')



# Carregar site 

loader = WebBaseLoader('https://pt.wikipedia.org/wiki/Inteligência_artificial')

lista_documentos = loader.load()



# Preparar texto

documento = ' '.join([doc.page_content for doc in lista_documentos])

documento = documento[:8000]



# Prompt 

template = ChatPromptTemplate.from_messages([

 ("system", "Você é um assistente chamado Minha_IA. Responda APENAS com base no conteúdo fornecido. Se a resposta não estiver no conteúdo, diga: 'Não encontrei essa informação no material fornecido.'"),

 ("user", "Conteúdo: {documentos_informados}\n\nPergunta: {pergunta_do_usuario}")

])



chain = template | chat



# Chatbot (loop)

while True:

  pergunta = input("\nDigite sua pergunta (ou 'x' para sair): ")



  if pergunta.lower() == 'x':

    print("Encerrando...")

    break



  resposta = chain.invoke({

   "documentos_informados": documento,

   "pergunta_do_usuario": pergunta

  })



  print("\nResposta da Minha_IA:")

  print(resposta.content)
#egis Chatbot

Projeto acadêmico desenvolvido com o objetivo de criar um chatbot inteligente capaz de responder perguntas utilizando exclusivamente informações extraídas de um site específico. O sistema foi construído em Python utilizando integração com IA generativa, web scraping e processamento de linguagem natural.

Contexto do Projeto

O projeto foi desenvolvido como trabalho universitário durante a graduação em Engenharia de Software, com foco em aplicar conceitos relacionados a:

inteligência artificial
automação de respostas
integração com APIs
processamento de texto
web scraping
engenharia de prompts

A proposta surgiu da necessidade de criar um chatbot capaz de responder perguntas de forma controlada, utilizando apenas informações previamente obtidas de uma fonte específica da internet.

🎯 Objetivo

O principal objetivo do projeto é demonstrar como um chatbot pode ser limitado a responder apenas com base em conteúdos definidos previamente, evitando o uso de conhecimento externo da IA.

Além disso, o sistema busca proporcionar aprendizado prático sobre:

consumo de APIs de IA
estruturação de prompts
manipulação de HTML
organização de projetos em Python
fluxo conversacional
🔎 Visão Geral

O Aegis Chatbot funciona através da extração automática de conteúdo textual de um site. Essas informações são processadas e enviadas para um modelo de inteligência artificial utilizando a API da Groq.

O chatbot recebe perguntas do usuário pelo terminal e gera respostas baseadas exclusivamente no conteúdo coletado do site informado no código.

Caso a informação solicitada não esteja presente na fonte analisada, o sistema retorna uma mensagem padrão informando que a resposta não foi encontrada.

🏗️ Arquitetura da Solução

O funcionamento do sistema ocorre em etapas:

1. Extração do conteúdo do site

A aplicação realiza uma requisição HTTP utilizando a biblioteca Requests para acessar o site desejado.

2. Processamento do HTML

Com o auxílio do BeautifulSoup, o sistema interpreta o HTML da página e extrai apenas os conteúdos relevantes, como parágrafos e títulos.

3. Limpeza e organização dos dados

Scripts, estilos e conteúdos desnecessários são removidos para melhorar a qualidade das respostas do chatbot.

4. Construção do Prompt

O conteúdo extraído é inserido dentro de um prompt personalizado enviado ao modelo de IA, contendo regras específicas como:

responder apenas utilizando o conteúdo do site
não utilizar conhecimento externo
informar quando não encontrar a resposta
5. Geração das respostas

A API da Groq processa o prompt utilizando o modelo Llama 3.3 70B Versatile e retorna respostas contextualizadas.

6. Interação com o usuário

O chatbot funciona em terminal, permitindo que o usuário envie perguntas continuamente até encerrar a conversa.

🚀 Tecnologias Utilizadas
Python
LangChain
Groq API
BeautifulSoup4
Requests
HTML Parsing
Prompt Engineering
⚙️ Instruções de Uso
1. Instalar as dependências
