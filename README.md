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
