# Curso Javascript

## Introdução Javascript

### INTERVALOS E TIMEOUUT

#### Intervalo
- "Intervalo" é uma função períodica, que será executada a cada "intervalo"

- Para **referenciar a função**, passar o escopo dela para outro local, deve-se usar o nome da função sem o parênteses. Se passar a função com o parênteses ela irá simplesmente executar e a referência não será passada.

#### Timeout
- É um atraso para executar a função.

```
setInterval(func, periodInMilis); // tempo em ms
setTimeout(func, delayInMilis)
```

#### Desafio

##### Questão5 

- O método "for... in" funciona para objetos; 
- para Arrays usar "Array.prototype.forEach()" ou "for... of"
- "Array.prototype.forEach()" lembra um paradigma mais funcional, declarativo
- "for... of" é um loop normal, imperativo

### Misc

Faz um "publish" no branch: (necessário antes de dar push)
``` 
git push -u origin master
```

## Manipulação da DOM

### Manipulando a DOM

- DOM: árvore de elementos do HTML; Elementos dispostos em tela

- Os atributos onclick, etc, recebem a função com parênteses


### Manipulando a DOM

- Recuperar elementos da DOM

- Armazenar "a referência" do elemento numa variável:
```
document.getElementById()
document.getElementsByTagName() // sempre retorna um vetor (HTMLCollection
document.getElementsByClassName() // sempre retorna um vetor também

// Query Selector - permite percorrer a DOM até encontrar o elemento desejado
document.querySelector("body div.app input#nome2"); // Caminho
    
    // Possível utilizar os atributos também -> usar [] para referenciar atributos
    document.querySelector("input[name=campo2]");

// Retorna apenas um único elemento, para retornar mais de um elemento usar querySelectorAll
    // Retonar uma NodeList

```
> "document" variável global que acessa a DOM da página
> Evita "sujar" a interface com código

### Lidando com elementos

- É posível criar novos elementos com Javascript 
    -> document.createElement()
    -> método appendChild de elementos HTML

- Texto -> precisar criar um TextNode como se fosse um elemento HTML e depois usar o appendChild

- removeChild() -> remove elemento

### Alterando estilos


## App de TODOS

- referenciar elementos na DOM

- complexidade do problema -> como é só texto, um array resolve; a estrutura poderia ser mais complexa.

- innerHTML manipula o conteúdo (HTML) do elemento

- Armazenamento local (local storage) - não relacional (chave-valor)

> Operaçções que modificam a lista -> salvar e deletar todos
> Storage acessado pela variável global "localStorage"
> métodos: localStorage.setItem('list_todos', todos);
> Storage não salva arrays, apenas string -> transferir para JSON (Extrutura de um Objeto do Javascript mas é uma String)
> JSON.stringify
> O conteúdo do storage pode ver visto na aba "application" no menu de dev do navegador

- Valor padrão para variável: "|| valor" - no caso do array: var array = JSON... || [] (valor padrão é um array vazio)
> Esse é um "problema" do JS - a tipagem fraca!
  
## JS assíncrono

### Requisição AJAX

- AJAX -> requisição assíncrona feita a algum backend

- No Javascript não há necessidade de recarregar a página após requisitar algo ao servidor

> classe XMLHttpRequest() -> recuperar informações com o AJAX


```(Javascript)
var xhr = new XMLHttpRequest();

xhr.open('GET', 'https://api.github.com/users/ahnucl');
xhr.send(null);

// Requisição asíncrona, não acontece no mesmo fluxo do script
xhr.onreadystatechange = function() {
    if(xhr.readyState === 4) { // resposta voltou
        console.log(JSON.parse(xhr.responseText));
    }
}
```

### Promisses

- Códigos que não influenciam a linha do tempo de execução do código - devolvem algo apenas após um intervalo de tempo

- Promisse é uma classe do JS; resolve e reject são funções, resolve é retornado em caso de sucessoe reject em caso contrário
```
var minhaPromise = function() {
    return new Promise( function(resolve, reject) {
        
    });
}
```

- Esqueleto de uma Promisse:
```
var minhaPromise = function() {
    return new Promise( function(resolve, reject) {
        var xhr = new XMLHttpRequest();
        xhr.open('GET', 'https://api.git4hub.com/users/ahnucl');
        xhr.send(null);

        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) { // resposta voltou
                if(xhr.status === 200) {
                    resolve(JSON.parse(xhr.responseText));
                } else {
                    reject('Erro na requisição');
                }
            }
        }
    });
}

minhaPromise()
    .then(function(response) {
        console.log(response);
    })
    .catch(function(error) {
        console.warn(error);
    });
``` 

- Promisse: quando chamar a função resolve (1º argumento) cai no "then", quando chamar a função reject (2º argumento) cai no "catch"; os argumentos para as funções no "then" e no "catch" são passados pelas funções da promisse.

### Utilizando Axios

- Usar Axios no lugar da XMLHttpRequest()
> https://github.com/axios/axios

- Importar a lib antes do script para ele já estar disponível

- Axios retorna mais coisas, inclusive a XMLHttpRequest, no mesmo objeto. É possível consultar o readyState, status, etc (campo request, é um XMLHttpRequest)

- Ou seja, o Axios é um Wrapper (encapsulamento) em volta da XMLHttpRequest

- Código equivalente ao do vídeo passado:
```
axios.get('https://api.github.com/users/ahnucl')
    .then(function(response) {
        console.log(response);
    })
    .catch(function(error) {
        console.warn(error);
    });
```

# Curso JavaScript ES6

## Introdução

- ES = EcmaScript

- 6 -> versão (ano + 1, 2015 no caso)

- JS antes do ES6 é bem diferente em questão de novas funcionalidades

- Babel: navegadores lentos para entender novas funcionalidades do ES, o Babel transpila as funcionalidades para códigos que todos os navegadores entendem (ES6+ não tem "Novas" funcionalidades, mas funcionalidades mais abstraídas, modificando a forma de escrever JS)

- Webpack: servidor de desenvolvimento para javacript

- Principais diferenças ES6+:
![](/misc/ES6.png)

- Async/Await é do ES8

## Configurando o Babel

- Instalando o babel e adicionando ele como dependência do projeto pelo yarn:
```
yarn add @babel/cli //instalando a cli do babel
```
- Outra dependência: "yarn add @babel/preset-env"

- "yarn.lock" -> tipo de cache do yarn

- node_nodules mantém todas as dependências do projeto

- após a inicialização de alguma dependência, criar o **.gitignore** para evitar que a pasta node_modules suba pro controle de versão

- arquivo de **configuração do babel** -> *".babelrc"*

- configuração básica do babel:
```
{
    "presets":["@babel/preset-env"]
}
```
> O preset-env é um preset do babel que entende qual ambiente estamos trabalhando (babel pode ser usado em browser, node, react native, etc)

- passar "-w" no comando do babel deixa o babel monitorando o serviço pra automaticamente gerar o novo bundle a cada alteração


## Classes

- Construtores: Propriedades podem ser definidas nos contrutores - o JS permite isso, não é necessário definir uma variável antes de chamá-la no construtor
```
class TodoList extends List {
    constructor() {
        super();

        this.usuario = 'Eu';
    }
```

- Ao adicionar um método de uma clase a um botão, é necessário uma função wrapper!
```
const MinhaLista = new TodoList();

document.getElementById('novotodo').onclick = function() {
    MinhaLista.add('asdasd'); 
}
```

- Métodos estáticos são chamados independente do restante da classe; úteis quando o método deve apenas receber um valor e retonar outro, por exemplo


## Const e Let

- constantes (const); Não é possível reatribuir o valor, mas é possível **mutar** : mudar propriedades dentro de um objeto ou array
```
const usuario = { nome: "leo" };

usuario.nome = 'Leonardo';
``` 
- variáveis de escopo (let) - consultar freeCodeCamp

## Operações em Array

- map, filter e reduce recebem um função como parâmetro

- map

- reduce

- filter

## Arrow Functions
```
const newArr = arr.map(function(item) {
    return item * 2;
});
// Vira:
const new Arr = arr.map(item => item * 2);
```

- Para retornar objetos na sintaxe reduzida é necessário envolvê-lo em parênteses:
```
const retornaObjeto = () => ({ nome: "Leonardo" });
```
