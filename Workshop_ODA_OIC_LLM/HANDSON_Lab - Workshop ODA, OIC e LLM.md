# Lab 0X - Workshop ODA, OIC e LLM

Criando um Oracle Digital Assistant que consome dados de uma chamada REST no Oracle Integration Cloud e responde mensagens através da integração com o LLM Maritaca AI.

## Description

Neste laboratório vamos criar: 
- Um Oracle Digital Assistant para interface de conversação e chamadas do OIC e LLM

- Montar uma integracão para chamar uma API do IBGE e retornar seu resultado.

- Criar uma chave de API para acesso ao LLM Maritaca AI

## Vamos nessa!

### 1. Provisionar nosso ambiente do Oracle Digital Assistant

* Abra sua console do OCI
![Alt text](./1.png "a title")

* No menu lateral esquerdo, utilize a barra de pesquisa e selecione a opcão de Digital Assistant
![Alt text](./2.png "a title")

* Selecione o compartment de sua escolha e siga os passos abaixo para criar um novo ambiente
![Alt text](./3.png "a title")

* Passe um Nome, uma Descrição e em SHAPE selecione Development (desenvolvimento), clique em CREATE
![Alt text](./4.png "a title")

* Após provisionado, abra o ambiente e clique em "Service Console" para acessar seu ODA
![Alt text](./5.png "a title")

### 2. Configurar API Services para as integrações com OIC e LLM

* Está é a tela inicial da console do Oracle Digital Assistant"
![Alt text](./6.png "a title")

* Clique no menu lateral, SETTINGS e API SERVICES
![Alt text](./7.png "a title")

* Clique no botão Add REST Service para criar uma nova conexão
![Alt text](./8.png "a title")

* Em Create REST Service adicione um nome, passe o Endpoint da sua integração OIC e em Method, selecione POST
![Alt text](./9.png "a title")

* Após criado, desça a pagina até Authentication Type e selecione Bearer Token. Adicione seu Token e salve 
![Alt text](./10.png "a title")

* Em Content Type, mais abaixo na pagina, selecione application/json. Cole o texto abaixo no Body: 

{
    "Nome": "Oracle"
}

* Teste sua requisição, clicando em Test Request
![Alt text](./11.png "a title")

* Clique no botão Add REST Service para criar uma nova conexão com LLM
![Alt text](./13.png "a title")

* Em Create REST Service adicione um nome, passe o Endpoint do Maritaca AI e em Method, selecione POST
![Alt text](./14.png "a title")

* Desça até HEADERS e em KEY escreve "authorization" e em VALUE passe key e sua chave API como mostra na imagem:
![Alt text](./15.png "a title")

* Em Request, Content Type selecione application/json e em Body, adicione o texto abaixo: 

{
    "messages": [
        {
            "role": "user",
            "content": "Você pode me falar quanto é 25 + 27?"
        }
    ],
    "do_sample": true,
    "max_tokens": 200,
    "temperature": 0.7,
    "top_p": 0.95
}

* Logo após, clique em Test Request
![Alt text](./16.png "a title")
![Alt text](./17.png "a title")

### 3. Criando Skill e fluxo de conversação no ODA

* Volte ao menu lateral, selecione Development e clique em Skills
![Alt text](./18.png "a title")

* Clique em New Skill
![Alt text](./19.png "a title")

* Passe um nome, uma descrição, selecione a Primary Language para portugues e clique em CREATE
![Alt text](./20.png "a title")

* Passe um nome, uma descrição, selecione a Primary Language para portugues e clique em CREATE
![Alt text](./20.png "a title")

* Com a Skill criada você verá a tela de Intenções para iniciar seu fluxo de conversação. Clique em + intent
![Alt text](./21.png "a title")

* Edite o nome para "coletar distritos"
![Alt text](./22.png "a title")

* Descça a pagina até Uterrances to ADD e clique em "advanced input mode"
![Alt text](./23.png "a title")

* Cole o texto abaixo e clique em CREATE: 

api
coletar
distritos
ibge
integracao

![Alt text](./24.png "a title")

* Repita os passos anteriores para criar uma Intent para fechar a conversação
![Alt text](./25.png "a title")
![Alt text](./26.png "a title")

* No topo da pagina, clique em Train e depois Submit, aguarde até o treino ser concluido
![Alt text](./27.png "a title")

* Após treinado, clique em Flows no menu lateral
![Alt text](./28.png "a title")

* Clique em Add Flow, passe as informações como mostra na imagem e clique em Create
![Alt text](./29.png "a title")

![Alt text](./30.png "a title")

* Após criado você verá seu flow de conversação, clique em ... em Start e add Start State
![Alt text](./31.png "a title")

* Em Add State, clique em Ask Question, passe um nome e clique em Insert
![Alt text](./32.png "a title")

* Edite a pergunta como mostra na imagem, em Variable clique em Create e Flow Scope
![Alt text](./33.png "a title")

* Em Name, coloque "nome" e em Type String, clique em Apply
![Alt text](./34.png "a title")

* Novamente vá em ... e clique em add State para seguir com o Fluxo
![Alt text](./35.png "a title")

* Selecione Send Message e clique em Insert
![Alt text](./35.png "a title")

*  Adicione o texto abaixo em Messages:

"Certo, ${nome.value}! estou coletando dados da integracão..."
![Alt text](./37.png "a title")

* Novamente vá em ... e clique em add State para seguir com o Fluxo
![Alt text](./38.png "a title")

* Selecione Call REST e clique em Insert
![Alt text](./39.png "a title")

* No bloco de REST Service, selecione sua conexão com OIC, Method POST e em Request Body cole o texto abaixo: 

{
    "Nome": "${nome.value}"
}

![Alt text](./40.png "a title")

* Em Result Variable, clique em Create e em Flow Scope Variable
![Alt text](./41.png "a title")

* em Name coloque "respostaIntegracao" e clique em Apply
![Alt text](./42.png "a title")

* Adicione um novo State no fluxo de conversação
![Alt text](./43.png "a title")

* Selecione Send Message e clique em Insert
![Alt text](./44.png "a title")

* Em Messages adicione o texto abaixo:
${respostaIntegracao.value.responsePayload.nome}

![Alt text](./45.png "a title")


* Clique em Preview e teste seu ODA, veja a saida do OIC como primeiro parametro da chamada

![Alt text](./46.png "a title")
![Alt text](./47.png "a title")

* Volte ao Fluxo e adicione um novo Flow
![Alt text](./48.png "a title")

* Selecione a Intent "fechar chat" e clique em Create
![Alt text](./49.png "a title")

* Em Start clique em ... e add start state
![Alt text](./50.png "a title")

* Selecione Send Message e clique em Insert
![Alt text](./51.png "a title")

* Altere o Messages como a imagem abaixo
![Alt text](./52.png "a title")

* Clique em Preview e teste seu ODA
![Alt text](./53.png "a title")

* Volte ao Fluxo e adicione um Flow em UnresolvedIntent
![Alt text](./54.png "a title")


* Em UnresolvedIntent, clique no primeiro State e clique em "add precending state"
![Alt text](./55.png "a title")

* Selecione Call Rest para fazer a chamada ao LLM
![Alt text](./56.png "a title")

* No Bloco Call Rest selecione sua integração com o Maritaca LLM, Method POST e cole o texto abaixo em Request Body: 

{
    "messages": [
        {
            "role": "user",
            "content": "${system.message.messagePayload.text}"
        }
    ],
    "do_sample": true,
    "max_tokens": 200,
    "temperature": 0.7,
    "top_p": 0.95
}

![Alt text](./57.png "a title")

* Em Result Variable, clique em Create e em Flow Scope Variable
![Alt text](./58.png "a title")

* em Name coloque "saida_LLM" e clique em Apply
![Alt text](./59.png "a title")

* Volte e em Headers no Bloco REST escreva "authorization" em KEY e cole sua chave API no Value como mostra na imagem abaixo: 
![Alt text](./60.png "a title")

* Volte para o fluxo e em send Message mude o conteudo para o texto abaixo: 

${saida_LLM.value.responsePayload.answer}

![Alt text](./61.png "a title")
![Alt text](./62.png "a title")

* Volte para o fluxo e em Call_LLM clique nos ... e em Make Start State como mostra na imagem
![Alt text](./63.png "a title")


* Clique em Preview, faça uma pergunta e veja sua integração com o LLM responder.
![Alt text](./64.png "a title")
![Alt text](./65.png "a title")

### Parabéns você finalizou seu Laboratório!!! 

## Autor

Gustavo Sant'ana

## Documentacões adicionais

* [ODA Documentation](https://docs.oracle.com/en/cloud/paas/digital-assistant/)

* [Maritaca AI Documentation](https://chat.maritaca.ai/)
