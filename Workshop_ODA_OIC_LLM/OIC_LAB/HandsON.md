# Lab 01 - Workshop OIC

Criando uma integracão simples utilizando adaptadores REST.

## Description

Neste laboratório vamos montar uma integracão para chamar uma API do IBGE e retornar seu resultado.

## Vamos nessa !

### 1. Provisionar nosso ambiente OIC

* Abra sua console do OCI
![Alt text](./1.PNG "a title")

* No menu lateral esquerdo, selecione a opcão de integration
![Alt text](./2.PNG "a title")

* Selecione o compartment de sua escolha e siga os passos abaixo para criar um novo ambiente
![Alt text](./3.PNG "a title")
![Alt text](./4.PNG "a title")

* Após provisionado, abra o ambiente e clique em "Service Console" para acessar seu OIC
![Alt text](./5.PNG "a title")

### 2. Configurar adaptadores para a integracão

* Depois de aberto seu OIC, clique na aba "integracões" e logo após em "conexões"
![Alt text](./6.PNG "a title")
![Alt text](./7.PNG "a title")

* Selecione a opcão "Criar" que esta na direita da tela
![Alt text](./8.PNG "a title")

* Pesquise por "Rest" e selecione o adaptador
![Alt text](./9.PNG "a title")

* Configure o adaptador da seguinte maneira:
``` 
Tipo de conexão: "URL Base da API Rest"
URL de Conexão: https://servicodados.ibge.gov.br/api/v1/localidades
Auth: "No security Policy"
```
![Alt text](./10.PNG "a title")

* Teste e Salve seu conector, após isso pode fechar essa tela
![Alt text](./11.PNG "a title")

### 3. Criar nossa primeira integracão

* Selecione a aba "Integracões" acima da aba de "Conexões"
![Alt text](./12.PNG "a title")

* Clique em "Criar" e selecione a opcão "Orquestrada por App"
![Alt text](./13.PNG "a title")
![Alt text](./14.PNG "a title")

* Preencha os campos necessário e clique em "Criar"
![Alt text](./15.PNG "a title")

* Como primeiro conector vamos utilizar o "Sample Rest Endpoint", pode ser encontrado pesquisando ou então no menu lateral direito, indo em Rest Adapters e o encontrando na lista que aparecer.
![Alt text](./16.jpg "a title")

* Configure o conector "acionador" da nossa integracão
![Alt text](./17.PNG "a title")
![Alt text](./18.PNG "a title")
![Alt text](./19.PNG "a title")
![Alt text](./20.PNG "a title")
![Alt text](./21.PNG "a title")
* Adicione o seguinte template JSON como Resposta:
```
[ {
  "id" : 520005005,
  "nome" : "Abadia de Goiás",
  "municipio" : {
    "id" : 5200050,
    "nome" : "Abadia de Goiás",
    "microrregiao" : {
      "id" : 52010,
      "nome" : "Goiânia",
      "mesorregiao" : {
        "id" : 5203,
        "nome" : "Centro Goiano",
        "UF" : {
          "id" : 52,
          "sigla" : "GO",
          "nome" : "Goiás",
          "regiao" : {
            "id" : 5,
            "sigla" : "CO",
            "nome" : "Centro-Oeste"
          }
        }
      }
    }
  }
} ]
```
![Alt text](./22.PNG "a title")
![Alt text](./23.PNG "a title")
![Alt text](./24.PNG "a title")
![Alt text](./25.PNG "a title")

* Arraste agora o conector que criamos no primeiro passo, no qual configuramos com a API do IBGE
![Alt text](./26.PNG "a title")

* Configure o conector com a "api ibge" da nossa integracão
![Alt text](./27.PNG "a title")
* Adicione o seguinte template JSON como Resposta:
```
{
  "id" : 0,
  "nome" : "distrito"
}
```
![Alt text](./28.PNG "a title")
![Alt text](./Screenshot_0.png "a title")
![Alt text](./30.PNG "a title")

* Exclua um dos componentes de mapeamento para não ficar duplicado
![Alt text](./31.PNG "a title")

* Adicione um componente de mapeamento agora após o ultimo conector Rest que colocamos
![Alt text](./32.PNG "a title")

* Aqui é onde vamos fazer o de/para das informacões recebidas da API do IBGE para resultado final da integracão (Arraste os campos da esquerda pra direita igual a imagem abaixo) e clique em "Validar" antes de fechar.
![Alt text](./Screenshot_1.png "a title")
![Alt text](./Screenshot_2.png "a title")
![Alt text](./Screenshot_3.png "a title")

* Agora selecione o menu sanduiche na direita da tela, e clique em "Rastreamento"
![Alt text](./35.PNG "a title")

* Arraste o campo "Nome" para a primeira coluna disponível e clique em "Salvar"
![Alt text](./36.PNG "a title")

* Podemos agora salvar nossa integracão e fechar logo em seguida
![Alt text](./37.PNG "a title")

### 4. Testar nossa primeira integracão

* Passe o mouse em cima da sua integracão e clique no botão de "Power", após isso clique me "Ativar"
![Alt text](./38.PNG "a title")
![Alt text](./39.PNG "a title")

* Passe o mouse em cima da sua integracão novamente e clique no botão de "Play", após isso selecione "Teste"
![Alt text](./40.PNG "a title")
![Alt text](./41.PNG "a title")

* Agora para fazer o teste final, clique novamente em "TESTE" e aguardamos para receber o retorno da API !
![Alt text](./42.PNG "a title")
![Alt text](./Screenshot_4.png "a title")

## Autor

Pedro Carrijo
LinkedIn: [@pedrocarrijo](https://twitter.com/dompizzie)

## Documentacões adicionais

* [OIC-GETTING-STARTED](https://docs.oracle.com/en/cloud/paas/integration-cloud/index.html)
* [OIC-REST-ADAPTER](https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-adapter/index.html)
