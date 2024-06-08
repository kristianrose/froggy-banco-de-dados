

#### O que há de novo? 
- Agora é possível listar keys que têm um prefixo!

#### Funcionalidades em que estou a trabalhar
- uma url de autenticação
- uma biblioteca baseada no Froggy
- sistema de ratelimit



### Começo
Qualquer tipo de linguagem é suportada. Mas eu vou usar o Node.js como exemplo. Como um cliente http, estou usando [phin](https://github.com/ethanent/phin).

##### Código inicial
- Importar a biblioteca phin
```js
const phin = require('phin');
```
Utilizar o url fornecido no site.  `froggy_url` vai ser a referência do URL do servidor web nos próximos trechos de código. Substitua-o pelo seu. 
- Criar uma função async
```js
async function Database () {}
```
Vamos escrever algum código dentro desta função. Nos trechos de código abaixo, 
- Definir uma key para um valor
```js
const response = await phin({
  method: 'POST',
  url: 'froggy_url/set',
  parse: 'json',
  data: {
    key: 'servidor_83726472',
    value: "Teste do Servidor"
  }
})
```
O código acima atribui um valor a uma keyt. A key pode estar na forma de `string`. Se falarmos de valor, pode colocar *strings*, *booleans* também como *objects* ! Definimos a nossa key com êxito. Vamos buscar o seu valor agora!  (A resposta principal está em `response.body`)
- Obter o valor de uma key
```js
const response = await phin({
  method: 'GET',
  url: 'froggy_url/get',
  parse: 'json',
  data: {
    key: 'servidor_83726472'
  }
}); console.log(response.body.value) // -> Teste do Servidor
```
O código acima obtém o valor de uma key. Se fornecer uma key inválida (ou seja, se não tiver atribuído um valor à key), o corpo da resposta devolverá o valor como`null` !

- Deletar a key
```js
const response = await phin({
  method: 'DELETE',
  parse: 'json',
  url: 'froggy_url/delete',
  data: {
    key: 'servidor_83726472'
  }
}); console.log(response.body.deleted) // -> true
```
É possível verificar se uma key foi ou não eliminada através da propriedade deleted do response.body. Se ela for `true`, então a key foi deletada!

- Lista todas as keys com um prefixo **(NOVO!)**
```js
phin({
  method: 'GET',
  parse: 'json',
  url: 'froggy_url/list',
    data: {
      prefix: `servidor_`,
      showKeys: true
    }
  }).then(res => {
    const arrayOfKeys = res.body.result;
})
```
O código acima é um exemplo de como pode listar keys que têm um prefixo em comum. O `showKeys` não é necessária. Ela já é verdadeira por padrão. Se definir `showKeys` para `false`, obterá apenas a array de valores.

- Código até agora
```js
const phin = require('phin');

async function Database () {

  /* Definir uma key para um valor */
  const setkey = await phin({
    method: 'POST',
    url: 'froggy_url/set',
    parse: 'json',
    data: {
      key: 'servidor_83726472',
      value: "Teste do Servidor"
    }
  });

  /* Obter o valor de uma key */
  const getkey = await phin({
    method: 'GET',
    url: 'froggy_url/get',
    parse: 'json',
    data: {
      key: 'servidor_83726472'
    }
  }); 
  const valor= getkey.body.value;
  console.log(valor); // -> Teste do Servidor

  /* Deletar a key */
  const delkey = await phin({
    method: 'DELETE',
    parse: 'json',
    url: 'froggy_url/delete',
    data: {
      key: 'servidor_83726472'
    }
  }); 
  const deletado = delkey.body.deleted;
  if (deletedo) console.log('Key deletada com sucesso!');

  phin({
  method: 'GET',
  parse: 'json',
  url: 'froggy_url/list',
    data: {
      prefix: `servidor_`,
      showKeys: true
    }
  }).then(res => {
    const arrayOfKeys = res.body.result;
  });
}

// Chamar a função 
Database();
```
