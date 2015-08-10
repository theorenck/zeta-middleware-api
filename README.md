# Zeta Middleware API

Browser and node module for making API requests against [Zeta Middleware API](http://{host}/api/{version}).

**Please note: This module uses [Popsicle](https://github.com/blakeembrey/popsicle) to make API requests. Promises must be supported or polyfilled on all target environments.**

## Installation

```
npm install zeta-middleware-api --save
bower install zeta-middleware-api --save
```

## Usage

### Node

```javascript
var ZetaMiddlewareApi = require('zeta-middleware-api');

var client = new ZetaMiddlewareApi();
```

### Browsers

```html
<script src="zeta-middleware-api/index.js"></script>

<script>
  var client = new ZetaMiddlewareApi();
</script>
```

### Options

You can set options when you initialize a client or at any time with the `options` property. You may also override options for a single request by passing an object as the second argument of any request method. For example:

```javascript
var client = new ZetaMiddlewareApi({ ... });

client.options = { ... };

client.resource('/').get(null, {
  baseUri: 'http://example.com',
  headers: {
    'Content-Type': 'application/json'
  }
});
```

#### Base URI

You can override the base URI by setting the `baseUri` property, or initializing a client with a base URI. For example:

```javascript
new ZetaMiddlewareApi({
  baseUri: 'https://example.com'
});
```

#### Base URI Parameters

If the base URI has parameters inline, you can set them by updating the `baseUriParameters` property. For example:

```javascript
client.options.baseUriParameters.version = 'v1';
```

### Resources

All methods return a HTTP request instance of [Popsicle](https://github.com/blakeembrey/popsicle), which allows the use of promises (and streaming in node).

#### resources.entities.schema

```js
var resource = client.resources.entities.schema;
```

##### GET

Este *endpoint* disponibiliza uma lista completa dos *recursos* disponíveis nesta instância específica do *Middleware*, com informações sobre a estrutura de dos recursos, campos, tipos de dados, índices de pesquisa e identificadores.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **extended** _boolean_

Este recurso possui metadados adicionais que pode ser solicitados sob demanda. O parâmetro **extended** define o modo com que os metadados contidos no recurso em questão serão apresentados. O valor padrão de **extended** é *false*, apresentado os metadados em sua representação simples. Para uma representação detalhada o parâmetro deve ser ajustado para *true*.

#### resources.entities.schema.name(name)

* **name** _string_

Nome do **recurso** a ser descrito.

```js
var resource = client.resources.entities.schema.name(name);
```

##### GET

Apresenta a estrutura, campos, tipos de dados, índices de pesquisa e identificadores de um único *recurso* da instância.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **extended** _boolean_

Este recurso possui metadados adicionais que pode ser solicitados sob demanda. O parâmetro **extended** define o modo com que os metadados contidos no recurso em questão serão apresentados. O valor padrão de **extended** é *false*, apresentado os metadados em sua representação simples. Para uma representação detalhada o parâmetro deve ser ajustado para *true*.

#### resources.entities.name(name)

* **name** _string_

Nome do **tipo de recurso** sobre o qual a operação será executada. Apenas tipos listados no **_endpoint_** */resources/schema* serão aceitos.

```js
var resource = client.resources.entities.name(name);
```

##### GET

Retorna todos os itens de um determinado tipo de **recurso**. Suporta seleção customizada dos campos das representações padrão dos recursos, ordenação e paginação.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **limit** _integer, default: 100_

Este recurso suporta paginação. O parâmetro **limit** define o numero máximo de itens retornados por página. Embora não seja obrigatório, pode trabalhar em conjunto com o parâmtro **offset**. (ex *limit=10* retorna uma página de itens da posição *0* até *10*. O valor padrão de limit é *100*, quando ajustado para *0* desabilita a paginação.

* **offset** _integer_

O parâmetro **offset**, ou deslocamento, indica o número de itens saltados antes do inicio da contagem definida por **limit** (ex: *?limit=10&offset=10* retorna uma página de itens da posição *11* até *20*). O valor padão de **offset** é *0*.

* **sort** _string_

Este recurso suporta ordenação. O parâmetro **sort** define o modo como a lista de itens será ordenada.

* **fields** _string_

É possíve retornar apenas um subconunto de campos da prepresentação padrão do recurso quando necessário. Nem sempre o cliente precisa de todas as informações contidas na representação padrão do recurso, permitir que ele receba somente quais informações que realmente precisa permite utilizar melhor recursos importantes como rede e hardware, otimizando a performance e eficiência do cunsumoa do serviço.

##### POST

Cria um novo recurso.

```js
resource.post().then(function (res) { ... });
```

##### Body

**application/json**

#### resources.entities.name(name).id(id)

* **id** _string_

**Identificador único** do **recurso** sobre o qual a operação será executada. Apenas recursos listados no **_endpoint_** */resources/schema* serão aceitos.

```js
var resource = client.resources.entities.name(name).id(id);
```

##### GET

Retorna um recurso.

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **fields** _string_

É possíve retornar apenas um subconunto de campos da prepresentação padrão do recurso quando necessário. Nem sempre o cliente precisa de todas as informações contidas na representação padrão do recurso, permitir que ele receba somente quais informações que realmente precisa permite utilizar melhor recursos importantes como rede e hardware, otimizando a performance e eficiência do cunsumoa do serviço.

##### DELETE

Destrói um recurso.

```js
resource.delete().then(function (res) { ... });
```

##### PATCH

Atualiza parcialmente um recurso.

```js
resource.patch().then(function (res) { ... });
```

##### Body

**application/json**

##### PUT

Atualiza um recurso.

```js
resource.put().then(function (res) { ... });
```

##### Body

**application/json**

#### resources.entities.search

```js
var resource = client.resources.entities.search;
```

##### GET

Endpoint que expõe uma interface de consulta nativa, baseada em SQL (ODBC 3.0)

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **limit** _integer, default: 100_

Este recurso suporta paginação. O parâmetro **limit** define o numero máximo de itens retornados por página. Embora não seja obrigatório, pode trabalhar em conjunto com o parâmtro **offset**. (ex *limit=10* retorna uma página de itens da posição *0* até *10*. O valor padrão de limit é *100*, quando ajustado para *0* desabilita a paginação.

* **offset** _integer_

O parâmetro **offset**, ou deslocamento, indica o número de itens saltados antes do inicio da contagem definida por **limit** (ex: *?limit=10&offset=10* retorna uma página de itens da posição *11* até *20*). O valor padão de **offset** é *0*.

* **extended** _boolean_

Este recurso possui metadados adicionais que pode ser solicitados sob demanda. O parâmetro **extended** define o modo com que os metadados contidos no recurso em questão serão apresentados. O valor padrão de **extended** é *false*, apresentado os metadados em sua representação simples. Para uma representação detalhada o parâmetro deve ser ajustado para *true*.

* **q** _string_

Uma declaração de cláusula SELECT da SQL (ODBC 3.0)

#### resources.health

```js
var resource = client.resources.health;
```

##### GET

```js
resource.get().then(function (res) { ... });
```

#### resources.artifacts

```js
var resource = client.resources.artifacts;
```

##### GET

```js
resource.get().then(function (res) { ... });
```

##### Query Parameters

```javascript
resource.get({ ... });
```

* **sort** _string_

Este recurso suporta ordenação. O parâmetro **sort** define o modo como a lista de itens será ordenada.

#### resources.artifacts.name(name)

* **name** _string_

```js
var resource = client.resources.artifacts.name(name);
```

##### GET

```js
resource.get().then(function (res) { ... });
```



### Custom Resources

You can make requests to a custom path in the API using the `#resource(path)` method.

```javascript
client.resource('/example/path').get();
```

## License

Apache 2.0
