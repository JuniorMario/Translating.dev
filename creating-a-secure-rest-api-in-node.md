Considere todo software que você já usou na sua vida, Não importa se você é
desenvolvedor ou só um usuário comum que casualmente vasculha a internet e 
e checa as redes sociais. Quase todo software que você pode achar utiliza
algum tipo de API.

API(Application programming interfaces / Interface de programação de aplicativos)
habilita aplicações de software a se comunicar com outros pedaços de software
de forma consistente. Seja internamente para conectar com algum componente
da aplicação ou externamente para conectar a um serviço.

Usar componetes e serviços baseados em API no desenvolvimento é também uma ótima 
maneira de manter escalabilidade e produtividade assim lhe capacitando de desenolver
múltiplas aplicações baseadas em componentes modulares e reutilizaveis,
permitindo escalabilidade e facilitando a manutenção.

Nós deveremos levar em consideração que vários serviços online tem front-facing APIs
(APIs com interface gráfica) e você pode utiliza-las para integrar fácilmente coisas
como logins/cadastros por redes sociais, pagamentos com cartão de crédito e muitas
outras funcionalidades.

Implementar tal variedade de serviços via API pode muito facilitado se usado 
uma plataforma em comum para comunicação, um protocolo padronizado, compartilhado
e com suporte de todos. Nós usaremos REST para isso.

REST significa REpresentation State Transfer(Transferência de Estado representacional)
e é utilizado para acessar e manipular dados usando várias operação sem declaração.
Essas operações são integradar ao protocolo HTTP e representa uma funcionalidade 
de essencial de CRUD(Create, Read, Update, Delete).

As operações HTTP disponíveis são:
	 - POST (criar um recurso ou fornece informações)
	 - GET (recee um index de recursos ou um recurso individual)
	 - PUT (Cria ou substitui um recurso)
	 - PATCH (Atualiza/Modifica um recurso)
	 - DELETE (Remove um recurso)

Usando as operações listadas acima e um recurso como endereço, nós podemos construir
uma API REST simplesmente criando um "endpoint" para cada operação. E para
implementação de padrãp, nós teremos uma base estável e compreensíveel
nos habilitando a desenvolver o código rápidamente e o manter depois.
Como mencionado anteriormente, a mesma base ser[a utilizada para integrar
"third-pary features", a maioria também usa REST API, fazendo sua integração
ser muito mais rápida.

Enquanto uma quantidade grande de plataformas e linguagens de programação pode
ser utilizado para contruir APIs, nesse artigo, vamos forca em Node.js.

Node.js é um ambiente runtime(aplicação que possibilita o processamento, a renderização e a execução de elementos escritos em linguagem não suportada nativamente pelo sistema)
que roda no server-side. Dentro desse ambiente, nós podemos usar JavaScript
para construir nosso software, nossas APIs REST, e invocar serviçoes externos
por meio de suas APIs. Esse fato é especialmente conveniente para desenvolvedores
que estão vindo do desensolvimento Front-end, pois dévem estar familiarizados
com JavaScript, fazendo a transição ser mais natural. Isso também tem o bonus
de unificar todas a base do códigoem uma única linguagem de programação.

Com uma infraestrutura, Node.js é projetado para criar aplicações de rede escaláveis.
também é relativamente simples de configurar numa máquina local, e você pode ter
seu servidor rodando com poucas linhas de código. Até alguns sérvicoes de Nuvem
como o AWS(Amazon Web Services) rodam Node.js, permiting que você rode aplicação sem servidor.

Agora, é claro, nada está muito claro, no meio das comunidades de programalção
há  sempre essa discussão sobre qual linguagem de programação é a melhor e qual
ambiente é o mais adequado para algum propósito específico. mas eu vou deixar
isso para você.

Por agora, vamos começar a criar nossa API REST com Node.js!

Nesse tutorial, nós vamos criar uma API bastante comum para um recurso chamado `users`.

Nosso recurso vai ser a seguinte estrutura básica:
	-id (Um UUID gerado automaticamente)
	-primeiroNome
	-sobrenome
	-email
	-senha
	-nivelDePermissao

E nós vamos criar as seguintes operações para esse recurso:
	- [POST] endpoint/usuarios
	- [GET] endpoint/usuarios (lista os usuários)
	- [GET] endpoint/usarios/:idDoUsuario (retorna um usuário especificado)
	- [PATCH] endpoint/usuarios/:idDoUsuario (atualiza as informações de um usuário especificado)
	- [DELETE] endpoint/usuarios/:idDoUsuario (remove um usuário especificado) 

Nós também usaremos o JWT(JSON Web Token) para acesso com tokens, e para esse fim, nós vamos
criar um outro recurso chamado `auth` que vai esperar o email e senha do usuário
e vai gerar e retornar um token utilizado para autenticação de algumas operações. 

##Configuração

Antes de tudo, garanta que você tem a última versão do Node.js instalada.
Para esse artigo, estarei usando a versão 10.15.2(8.11+ deve funcionar). 

Então, deve-se garantir que você tem o MongoDB instalado, ou instale daqui www.mongodb.com.

Crie uma pasta que usaremos para o nosso projeto e nomeie de `API-rest-simples`.

Abre um terminal nessa pasta e rode `npm init` para criar o arquivo package.json
para nosso projeto.

Nós estaremos também usando Express nesse projeto. É um framework decente de Node.js
com algumas features nativas que vão acelerar nosso desenvolvimento.

Para manter esse tutorial focando e no tópico, aqui está o package.json com as devidas dependencias:
`{
  "name": "api-rest-simples",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "body-parser": "1.7.0",
    "express": "^4.8.7",
    "jsonwebtoken": "^7.3.0",
    "moment": "^2.17.1",
    "moment-timezone": "^0.5.13",
    "mongoose": "^5.1.1",
    "node-uuid": "^1.4.8",
    "swagger-ui-express": "^2.0.13",
    "sync-request": "^4.0.2"
  }
}
`

Cole o código no arquivo package.json e depois disso rode `npm install`.
Parabéns, agora você tem todas as dependência e configurações necessárias para rodar
nossa API REST simples.


