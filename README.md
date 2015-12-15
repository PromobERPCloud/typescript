# Promob TypeScript Style Guide

*Style Guide baseado no [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)*

## Conteúdos

  1. [Tipos](#tipos)
  1. [Referências](#referências)
  1. [Objetos](#objetos)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Funções](#funções)
  1. [Arrow Functions (Lambdas)](#arrow-functions)
  1. [Construtores](#construtores)
  1. [Módulos](#módulos)
  1. [Iteradores e Geradores](#iteradores-e-geradores)
  1. [Propriedades](#propriedades)
  1. [Variáveis](#variáveis)
  1. [Hoisting](#hoisting)
  1. [Operadores](#operadores)
  1. [Blocos](#blocos)
  1. [Comentários](#comentários)
  1. [Whitespace](#whitespace)
  1. [Vírgulas](#vírgulas)
  1. [Ponto e vírgula](#ponto-e-vírgula)
  1. [Casting](#casting)
  1. [Nomes](#nomes)
  1. [Acessores](#acessores)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [Anotações de Tipo](#anotações-de-tipo)
  1. [Interfaces](#interfaces)
  1. [Organização](#organização)
  1. [Compatibilidade com ECMAScript 5](#compatibilidade-com-ecmascript-5)
  1. [ECMAScript 6](#ecmascript-6)
  1. [Typescript 1.5](#typescript-15)
  1. [License](#license)

## Tipos

  - [1.1](#1.1) <a name='1.1'></a> **Primitivo**: A manipulação de um tipo primitivo é feita diretamente no seu valor.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **Complexo**: A manipulação de um tipo complexo é feito por uma referência para o seu valor.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ voltar ao topo](#conteúdos)**

## Referências

  - [2.1](#2.1) <a name='2.1'></a> Use `const` para todas as suas referências; evite usar `var`.

  > Isso garante que você não vá reatribuir suas referências (mutação), o que pode levar a bugs e código de difícil compreensão.

    ```javascript
    // ruim
    var a = 1;
    var b = 2;

    // bom
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> Se as referências necessitam ser alteradas, use `let` ao invés `var`.

  > `let` possui escopo de bloco (block-scoped) ao invés do escopo de função (function-scoped) que `var` possui.

    ```javascript
    // ruim
    var count = 1;
    if (true) {

      count += 1;

    }

    // bom, usando let
    let count = 1;
    if (true) {

      count += 1;

    }
    ```

  - [2.3](#2.3) <a name='2.3'></a>  `let` e `const` possuem escopo de block (block-scoped).

    ```javascript
    // const e let existem apenas nos blocos em que foram definidos
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ voltar ao topo](#conteúdos)**

## Objetos

  - [3.1](#3.1) <a name='3.1'></a> Utilize a sintaxe literal para criar um objeto

    ```javascript
    // ruim
    const item = new Object();

    // bom
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> Não utilize [palavras reservadas](http://es5.github.io/#x7.6.1) como nome de atributos. Garante a compatibilidade com versões antigas do IE.

    ```javascript
    // ruim
    const superman = {
      default: { clark: 'kent' },
      private: true,
    };

    // bom
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true,
    };
    ```

  - [3.3](#3.3) <a name='3.3'></a> Utilize sinônimos legíveis no lugar de palavras reservadas.

    ```javascript
    // ruim
    const superman = {
      class: 'alien',
    };

    // ruim
    const superman = {
      klass: 'alien',
    };

    // bom
    const superman = {
      type: 'alien',
    };
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> Utilize propriedades computadas quando criar objetos com nome dinâmico de propriedade.

  > Permite que você defina todas as propridades de um objeto em apenas um lugar.

    ```javascript

    const getKey = function(k) {

      return `um atributo chamado ${k}`;

    }

    // ruim
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // bom
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> Utilize lambdas (arrow functions) para objetos com métodos ao invés de propriedades ou funções anônimas.

    ```javascript
    // ruim
    const atom = {
      value: 1,
      addValue: function (value) {
        return atom.value + value;
      },
    };

    // ruim
    const atom = {
      value: 1,
      addValue(value) {
        return atom.value + value;
      },
    };

    // bom
    const atom = {
      value: 1,
      addValue: (value) => atom.value + value
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name='3.6'></a> Utilize a forma reduzida de atribuição de valor.

  > Maneira mais sucinta e descritiva.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  - [3.7](#3.7) <a name='3.7'></a> Agrupe as atribuições de forma reduzida no começo da declaração do objeto.

  >  Facilita o entendimento das propriedades atribuídas de forma reduzida.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // ruim
    const obj = {
      episodeOne: 1,
      twoJedisWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // bom
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJedisWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

**[⬆ voltar ao topo](#conteúdos)**

## Arrays

  - [4.1](#4.1) <a name='4.1'></a> Utilize a sintaxe literal para criar arrays.

    ```javascript
    // ruim
    const items = new Array();

    // bom
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> Utilize Array#push ao invés de atribuição direta para adicionar itens ao array.

    ```javascript
    const someStack = [];


    // ruim
    someStack[someStack.length] = 'abracadabra';

    // bom
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> Utilize array spreads `...` para copiar arrays.

    ```javascript
    // ruim
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // bom
    const itemsCopy = [...items];
    ```
  - [4.4](#4.4) <a name='4.4'></a> Para converter um objeto array-like para um array, utilize Array#from.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

**[⬆ voltar ao topo](#conteúdos)**

## Destructuring

  - [5.1](#5.1) <a name='5.1'></a> Utilize object destructuring quando é necessário acessar e utilizar múltiplas propriedades de um objeto.

  > Com destructuring é desnecessário criar referências temporárias para utilizar as propriedades do objeto.

    ```javascript
    // ruim
    const getFullName = function(user) {

      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;

    }

    // bom
    const getFullName = function(obj) {

      const { firstName, lastName } = obj;
      return `${firstName} ${lastName}`;

    }

    // muito bom
    const getFullName = function({ firstName, lastName }) {

      return `${firstName} ${lastName}`;

    }
    ```

  - [5.2](#5.2) <a name='5.2'></a> Utilize array destructuring.

    ```javascript
    const arr = [1, 2, 3, 4];

    // ruim
    const first = arr[0];
    const second = arr[1];

    // bom
    const [first, second] = arr;
    ```

  - [5.3](#5.3) <a name='5.3'></a> Utilize object destructuring para múltiplos valores de retorno, não array destructuring.

  > Você pode adicionar novas propriedades sem quebrar as funções que utilizam o método.

    ```javascript
    // ruim
    const processInput = function(input) {
      return [left, right, top, bottom];

    }

    // o caller precisa pensar sobre a ordem dos valores dentro do array
    const [left, __, top] = processInput(input);

    // bom
    const processInput = function(input) {
      return { left, right, top, bottom };

    }

    // o caller seleciona apenas os atributos necessários
    const { left, right } = processInput(input);
    ```


**[⬆ voltar ao topo](#conteúdos)**

## Strings

  - [6.1](#6.1) <a name='6.1'></a> Aspas simples `''` para strings.

    ```javascript
    // ruim
    const name = "Capt. Janeway";

    // bom
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> Strings mais longas que 120 caracteres devem ser escritas em mútiplas linhas.
  - [6.3](#6.3) <a name='6.3'></a> Nota: Concatenação de strings longas pode causar problemas de performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // ruim
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // ruim
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bom
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name='6.4'></a> Quando construindo strings programaticamente, utilize templates.

  > Templates são mais legíveis, possuem sintaxe concisa e  interpolação.

    ```javascript
    // ruim
    const sayHi = function(name) {

      return 'Olá ' + name + '.';

    }

    // ruim
    const sayHi = function(name) {

      return ['Olá ', name, '.'].join();

    }

    // bom
    const sayHi = function(name) {

      return `Olá ${name}.`;

    }
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Funções

  - [7.1](#7.1) <a name='7.1'></a> Declare funções ao invés de criar expressões

  > Declarações são nomeadas e são mais fáceis de identificar no call stack.

    ```javascript
    
    // ruim
    const foo = function() {
    };

    // ruim
    const foo = () => {
    };
    
    // bom
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name='7.2'></a> Expressões:

    ```javascript
    // immediately-invoked function expression (IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name='7.3'></a> Nunca declare a função em um bloco que não seja de função (if, while, etc). Ao invés, atribua a função a uma variável. Browsers interpretam diferentemente esse comportamento.
  - [7.4](#7.4) <a name='7.4'></a> **Nota:** ECMA-262 define um `block` como uma lista de comandos. Uma declaração de função não é um comando. [Nota ECMA-262](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // ruim
    if (currentUser) {

      const test = function() {

        console.log('Nope.');

      }

    }

    // bom
    let test;
    if (currentUser) {

      test = () => {

        console.log('Yup.');

      };

    }
    ```

  - [7.5](#7.5) <a name='7.5'></a> Nunca nomeie um parametro `arguments`. Isso terá precedência em cima do `arguments` que é dado para cada escopo de função.

    ```javascript
    // ruim
    const nope = function(name, options, arguments) {
      // comandos
    }

    // bom
    const yup = function(name, options, args) {
      // comandos
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name='7.6'></a> Nunca utilize `arguments`, escolha a sintaxe rest  `...`.

  > `...` é explicito quanto quais argumentos você deseja utilizar. Argumentos rest estão em um Array, e não em um array-like object como o `arguments`.

    ```javascript
    // ruim
    const concatenateAll = function() {

      const args = Array.prototype.slice.call(arguments);
      return args.join('');

    }

    // bom
    const concatenateAll = function(...args) {

      return args.join('');

    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> Utilize a sintaxe de parâmetro default ao invés de alterar argumentos da função.

    ```javascript
    // muito ruim
    const handleThings = function(opts) {
      // não devemos alterar argumentos de função.
      // se opts é falso será setado para um objeto que talvez introduza bugs sutis
      opts = opts || {};
      // ...
    }

    // ruim
    const handleThings = function(opts) {

      if (opts === void 0) {

        opts = {};

      }
      // ...
    }

    // bom
    const handleThings = function(opts = {}) {
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> Use parâmetros default com cuidado.

  ```javascript
  var b = 1;
  const count = function(a = b++) {

    console.log(a);

  }
  count();  // 1
  count();  // 2
  count(3); // 3
  count();  // 3
  ```


**[⬆ voltar ao topo](#conteúdos)**

## Arrow Functions

  - [8.1](#8.1) <a name='8.1'></a> Quando function expressions são necessárias, como por exemplo passando uma função anônima como parametro, utilize a sintaxe de arrow.

  > Se a função for razoávelmente complexa, talvez seja melhor mover essa lógica para uma declaração.

    ```javascript
    // ruim
    [1, 2, 3].map(function (x) {

      return x * x;

    });

    // bom
    [1, 2, 3].map((x) => {

      return x * x;

    });

    // bom
    [1, 2, 3].map((x) => x * x;);
    ```

  - [8.2](#8.2) <a name='8.2'></a> Se o corpo da função tiver o tamanho de uma linha e apenas um argumento, é dispensável o uso de parênteses e chaves, e pode ser utilizado o return implícito. Se não, utilize as chaves, parentes e `return`

  > Syntactic sugar. Muito legível.

  > Não utilize se a função retorna um objeto complexo.

    ```javascript
    // bom
    [1, 2, 3].map(x => x * x);

    // bom
    [1, 2, 3].reduce((total, n) => {
      return total + n;
    }, 0);
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Construtores

  - [9.1](#9.1) <a name='9.1'></a> Sempre utilize `class`. Evite manipular o `prototype` diretamente.

  > `class` é mais concisa e é mais familiar para C# devs.

    ```javascript
    // ruim
    function Queue(contents = []) {

      this._queue = [...contents];

    }
    Queue.prototype.pop = function() {

      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;

    }


    // bom
    class Queue {

      constructor(contents = []) {

        this._queue = [...contents];

      }

      pop() {

        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;

      }

    }
    ```

  - [9.2](#9.2) <a name='9.2'></a> Utilize `extends` para herança.

  > É uma maneira simples de herdar a funcionalidade do prototype sem quebrar `instanceof`.

    ```javascript
    // ruim
    const inherits = require('inherits');
    function PeekableQueue(contents) {

      Queue.apply(this, contents);

    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {

      return this._queue[0];

    }

    // bom
    class PeekableQueue extends Queue {

      peek() {

        return this._queue[0];

      }

    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> Métodos podem retornar `this` para implementar method chaining.

    ```javascript
    // ruim
    Jedi.prototype.jump = function() {

      this.jumping = true;
      return true;

    };

    Jedi.prototype.setHeight = function(height) {

      this.height = height;

    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // bom
    class Jedi {

      jump() {

        this.jumping = true;
        return this;

      }

      setHeight(height) {

        this.height = height;
        return this;

      }

    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - [9.4](#9.4) <a name='9.4'></a> É aconselhado reescrever o método toString() se necessário

    ```javascript
    class Jedi {

      contructor(options = {}) {

        this.name = options.name || 'no name';

      }

      getName() {

        return this.name;

      }

      toString() {

        return `Jedi - ${this.getName()}`;

      }

    }
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Módulos

  - [10.1](#10.1) <a name='10.1'></a> Utilize (`import`/`export`) como padrão.

    ```javascript
    // ruim
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // bom
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // muito bom - angularjs2 syntax
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a>Não `export` diretamente de um `import`. 

  > Ter uma maneira de importar módulos e uma maneira para exportar deixa o código mais consistente.

    ```javascript
    // ruim
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // bom
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.3](#10.3) <a name='10.3'></a>Não utilize wildcards
  
    ```javascript
    // ruim
    import * as PromobERPCommon from './common';
    
    //bom
    import PromobERPCommon from './common;'
    ```

**[⬆ voltar ao topo](#conteúdos)**

## Iteradores e Geradores

  - [11.1](#11.1) <a name='11.1'></a> Evite iteradores. Prefira utilizar funções como `map()`, `reduce()`, `forEach()` ao invés de `for-of`.

  > Isso fortalece a regra de referências imutáveis.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // ruim
    let sum = 0;
    for (let num of numbers) {

      sum += num;

    }

    sum === 15;

    // bom
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // muito bom
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - [11.2](#11.2) <a name='11.2'></a> Não utilize geradores por enquanto.

**[⬆ voltar ao topo](#conteúdos)**


## Propriedades

  - [12.1](#12.1) <a name='12.1'></a> Use notação de ponto para acessar propriedades.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // ruim
    const isJedi = luke['jedi'];

    // bom
    const isJedi = luke.jedi;
    ```

  - [12.2](#12.2) <a name='12.2'></a> Utilize a sintaxe de indexação `[]` em casos raros. Prefira criar uma interface e utilizar a notação de ponto quando possível.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    const getProp = function(prop) {

      return luke[prop];

    }

    const isJedi = getProp('jedi');
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Variáveis

  - [13.1](#13.1) <a name='13.1'></a> Sempre utilize `const`/`let` para declarar variáveis, dependendendo da necessidade. Prefira `const`.

    ```javascript
    // ruim
    superPower = new SuperPower();

    // bom
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> Utilize uma declaração `const`/`let` por variável.

    > É mais fácil de ler e adicionar variáveis.

    ```javascript
    // ruim
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // ruim
    // (encontre o erro :) )
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // bom
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> Agrupe todos seus `const`s e todos seus `let`s.

  > Ajuda a organizar o código.

    ```javascript
    // ruim
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // ruim
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // bom
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> Atribua os valores quando necessário, mas declare as variáveis em lugares sensatos.

  > Lembre-se: `let` e `const` possuem escopo de bloco (block-scoped) e não escopo de função (function-scoped).

    ```javascript
    // bom
    function() {

      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {

        return false;

      }

      return name;

    }

    // ruim - chamada de função desnecessária
    function(hasName) {

      const name = getName();

      if (!hasName) {

        return false;

      }

      this.setFirstName(name);

      return true;

    }

    // bom
    function(hasName) {

      if (!hasName) {

        return false;

      }

      const name = getName();
      this.setFirstName(name);

      return true;

    }
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Hoisting

  - [14.1](#14.1) <a name='14.1'></a> Hoisting é o comportamento padrão do javascript de mover declarações para o topo. Declarações `var` sofrem com isso, mas a atribuição de um valor para uma variável `var` não sofre. Declarações `const` e `let` possuem um conceito chamado [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let). É importante saber que [typeof não é seguro](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // isso não funcionaria se notDefined não tivesse sido declarada
    function example() {

      console.log(notDefined); // => throws a ReferenceError

    }

    // isso funcionaria devido ao hoisting, mas o valor da váriavel não estaria setado
    function example() {

      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;

    }

    // o interpretador está aplicando o hoisting na variável, o exemplo acima poderia ser reescrito dessa maneira:
    function example() {

      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;

    }

    // utilizando const e let
    function example() {

      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;

    }
    ```

  - [14.2](#14.2) <a name='14.2'></a> Funções anônimas sofrem "hoist" da própria variável, mas não a atribuição do corpo da função.

    ```javascript
    function example() {

      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {

        console.log('anonymous function expression');

      };

    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> Funções nomeadas que são atribuídas a variáveis sofrem "hoist" do nome da variável, mas não do nome da função nem da implementação da função.

    ```javascript
    function example() {

      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {

        console.log('Flying');

      };

    }

    // o mesmo vale se o nome da função é igual ao nome da variável
    function example() {

      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {

        console.log('named');

      }

    }
    ```

  - [14.4](#14.4) <a name='14.4'></a> Declarações de funções sofrem "hoist" no nome da função e na sua implementação.

    ```javascript
    function example() {

      superPower(); // => Flying

      function superPower() {

        console.log('Flying');

      }

    }
    ```

  - Mais Informações: [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ voltar ao topo](#conteúdos)**


## Operadores

  - [15.1](#15.1) <a name='15.1'></a> SEMPRE utilize `===` e `!==` ao invés de `==` e `!=`.
  - [15.2](#15.2) <a name='15.2'></a> Comandos condicionais como o `if` avaliam a expressão utilizando coercion com o método abstrato `ToBoolean` e seguem as seguintes regras:

    + **Objects** são **true**
    + **Undefined** é **false**
    + **Null** é **false**
    + **Booleans** recebem **o valor do booleano**
    + **Numbers** são **false** se **+0, -0, ou NaN**, senão são **true**
    + **Strings** são **false** se é uma string vazia `''`, senão são  **true**

    ```javascript
    if ([0]) {
      // true
      // Array é um objeto, objeto é true
    }
    ```

  - [15.3](#15.3) <a name='15.3'></a> Utilize atalhos.

    ```javascript
    // ruim
    if (name !== '') {
      // ...stuff...
    }

    // bom
    if (name) {
      // ...stuff...
    }

    // ruim
    if (collection.length > 0) {
      // ...stuff...
    }

    // bom
    if (collection.length) {
      // ...stuff...
    }
    ```

  - [15.4](#15.4) <a name='15.4'></a> Para mais informações: [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ voltar ao topo](#conteúdos)**


## Blocos

  - [16.1](#16.1) <a name='16.1'></a> Utilize chaves para blocos multi-linhas. Pode-se omitir as chaves em blocos de uma linha.

    ```javascript
    // ruim
    if (test) return false;

    // bom
    if (test)
      return false;

    // muito bom
    if (test) {
      return false;
    }

    // ruim
    function() { return false; }

    // bom
    function() {
      return false;
    }
    ```

  - [16.2](#16.2) <a name='16.2'></a> Se você está utilizando blocos multi-linhas com `if` e `else`, coloque `else` na mesma linha do fechamento do bloco do `if`.

    ```javascript
    // ruim
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // bom
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

    - [16.3](#16.3) <a name='16.3'></a> Não omita as chaves em blocos multi-linha.

    > Omitir as chaves nesse caso facilita bugs.

    ```javascript
    // ruim
    if (test)
      thing1();
      thing2();
    else
      thing3();

    // bom
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ voltar ao topo](#conteúdos)**


## Comentários

  - [17.1](#17.1) <a name='17.1'></a> Utilize `/** ... */` para comentários multi-linha. Inclua uma descrição descrição e tente não ser redundante. Se o código está claro o suficiente para outra pessoa, reavalie se comentários são realmente necessários.
    ```javascript
    // ruim
    // make() retorna um novo elemento
    // baseado na tag
    //
    const make = function(tag) {

      // ...stuff...

      return element;

    }

    // bom
    /**
     * make() retorna um novo elemento
     * baseado na tag
     */
    const make = function(tag) {

      // ...stuff...

      return element;

    }
    ```

  - [17.2](#17.2) <a name='17.2'></a> Utilize `//` para comentários de uma linha. Comente na linha imediatamente acima a linha do código, e deixe uma linha branco entre o comando anterior e o comentário.

    ```javascript
    // ruim
    const active = true;  // is current tab

    // bom
    // is current tab
    const active = true;

    // ruim
    const getType = function() {

      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;

    }

    // bom
    const getType = function() {

      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;

    }
    ```

  - [17.3](#17.3) <a name='17.3'></a> Utilize prefixos `FIXME` ou `TODO` quando necessário.

  - [17.4](#17.4) <a name='17.4'></a> Utilize `//FIXME:` para descrever problemas.

    ```javascript
    class Calculator {

      constructor() {
        // FIXME: shouldn't use a global here
        total = 0;
      }

    }
    ```

  - [17.5](#17.5) <a name='17.5'></a> Utilize `//TODO:` para descrever soluções.

    ```javascript
    class Calculator {

      constructor() {
        // TODO: total should be configurable by an options param
        this.total = 0;
      }

    }
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Whitespace

  - [18.1](#18.1) <a name='18.1'></a> Utilize tabs como 4 espaços - padrão VS.

    ```javascript
    // ruim
    function() {

    ∙∙const name;

    }

    // ruim
    function() {

    ∙const name;

    }

    // bom
    function() {

    ∙∙∙∙const name;

    }
    ```

  - [18.2](#18.2) <a name='18.2'></a> Coloque um espaço antes da chave inicial.

    ```javascript
    // ruim
    const test = function(){

      console.log('test');

    }

    // bom
    const test = function() {

      console.log('test');

    }

    // ruim
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // bom
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  - [18.3](#18.3) <a name='18.3'></a> Coloque um espaço antes de abrir parênteses em comandos de controle (`if`, `while` etc.). Não coloque espaços antes da lista de argumentos de uma função.

    ```javascript
    // ruim
    if(isJedi) {

      fight ();

    }

    // bom
    if (isJedi) {

      fight();

    }

    // ruim
    const fight = function () {

      console.log ('Swooosh!');

    }

    // bom
    const fight = function() {

      console.log('Swooosh!');

    }
    ```

  - [18.4](#18.4) <a name='18.4'></a> Utilize espaço para separar operadores.

    ```javascript
    // ruim
    const x=y+5;

    // bom
    const x = y + 5;
    ```

  - [18.5](#18.5) <a name='18.5'></a> Acabe arquivos com um caracter de nova linha.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - [18.5](#18.5) <a name='18.5'></a> Idente quando estiver fazendo method chaining de vários métodos. Utilize o ponto no começo da linha para demonstrar que é um método e não um comando novo.
  
    ```javascript
    // ruim
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // ruim
    $('#items').
      find('.selected').
      highlight().
      end().
      find('.open').
      updateCount();

    // bom
    $('#items')
      .find('.selected')
      .highlight()
      .end()
      .find('.open')
      .updateCount();

    // ruim
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // bom
    const leds = stage.selectAll('.led')
        .data(data)
        .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
        .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - [18.6](#18.6) <a name='18.6'></a> Insira uma linha em branco após abrir o bloco e antes de fechar ele.

  ```javascript
  // ruim
  if (foo) {
    return bar;
  }

  // obm
  if (foo) {

    return bar;

  }

  // ruim
  const baz = function(foo) {
    return bar;
  }

  // bom
  const baz = function(foo) {

    return bar;

  }
  ```

  - [18.7](#18.7) <a name='18.7'></a> Insira uma linha em branco após o final do bloco

    ```javascript
    // ruim
    if (foo) {

      return bar;

    }
    return baz;

    // bom
    if (foo) {

      return bar;

    }

    return baz;

    // ruim
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // bom
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;
    ```


**[⬆ voltar ao topo](#conteúdos)**

## Vírgulas

  - [19.1](#19.1) <a name='19.1'></a> Vírgulas no ínicio: **Nope.**

    ```javascript
    // ruim
    const story = [
        once
      , upon
      , aTime
    ];

    // bom
    const story = [
      once,
      upon,
      aTime,
    ];

    // ruim
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // bom
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [19.2](#19.2) <a name='19.2'></a> DISCUSS - Vírgula sobrando: **Nope.**

  > Typescript não elimina a vírgula que sobra.

    ```javascript
    // ruim
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    
    // bom
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    ```

**[⬆ voltar ao topo](#conteúdos)**


## Ponto e vírgula

  - [20.1](#20.1) <a name='20.1'></a> **Sempre.**

    ```javascript
    // ruim
    (function() {

      const name = 'Skywalker'
      return name

    })()

    // bom
    (() => {

      const name = 'Skywalker';
      return name;

    })();

    // bom - closures não se tornam argumentos no caso de concatenação
    ;(() => {

      const name = 'Skywalker';
      return name;

    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ voltar ao topo](#conteúdos)**


## Casting

  - [21.1](#21.1) <a name='21.1'></a> Faça o cast no início  do comando.
  - [21.2](#21.2) <a name='21.2'></a> Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - [21.3](#21.3) <a name='21.3'></a> Utilize `parseInt` para Numbers e sempre utilize um radix para casting.

    ```javascript
    const inputValue = '4';

    // ruim
    const val = new Number(inputValue);

    // ruim
    const val = +inputValue;

    // ruim
    const val = inputValue >> 0;

    // ruim
    const val = parseInt(inputValue);

    // bom
    const val = Number(inputValue);

    // bom
    const val = parseInt(inputValue, 10);
    ```

  - [21.4](#21.4) <a name='21.4'></a> `parseInt` pode virar um bottleneck. Utilize Bitshift caso tenha se tornado um bottleneck por [razões de performance](http://jsperf.com/coercion-vs-casting/3), comente porque e o que você está fazendo.

    ```javascript
    // good
    /**
     * parseInt estava deixando o código lento
     * Bitshifting a string para transformar-lá num Number deixou muito mais rápido
     */
    const val = inputValue >> 0;
    ```

  - [21.5](#21.5) <a name='21.5'></a> **Note:** Cuidado ao utilizar bitshift. Numeros são representados por [64 bits](http://es5.github.io/#x4.3.19), porém operações bitshifts sempre [retornam 32 bits](http://es5.github.io/#x11.7). O Maior inteiro de 32 bits é 2.147.483.647.
  
    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - [21.6](#21.6) <a name='21.6'></a> Booleans:

    ```javascript
    const age = 0;

    // ruim
    const hasAge = new Boolean(age);

    // bom
    const hasAge = Boolean(age);

    // bom
    const hasAge = !!age;
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Nomes

  - [22.1](#22.1) <a name='22.1'></a> Seja sempre descritivo com o nome das suas funções.

    ```javascript
    // ruim
    function q() {
      // ...stuff...
    }

    // bom
    function query() {
      // ..stuff..
    }
    
    
    // muito bom
    function queryStuffOnDataBase() {
      // ..stuff..
    }
    ```

  - [22.2](#22.2) <a name='22.2'></a> Utilize camelCase para nomear objetos, métodos, funções, campos privados e instâncias.

    ```javascript
    // ruim
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    const c = function() {}

    // bom
    const thisIsMyObject = {};
    const thisIsMyFunction = function() {}
    ```

  - [22.3](#22.3) <a name='22.3'></a> Utilize PascalCase para nomear classes, campos públicos, modulos ou interfaces - essas sempre começando por I.

    ```javascript
    // ruim
    function user(options) {

      this.name = options.name;

    }

    const bad = new user({
      name: 'nope',
    });

    // bom
    module AperatureScience {

      class User {

        constructor(options) {

          this.name = options.name;

        }

      }

    }

    const good = new AperatureScience.User({
      name: 'yup',
    });
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Acessores 

  - [23.1](#23.1) <a name='23.1'></a> DISCUSS Utilize a sintaxe de `get()` e `set()` do Typescript 

    ```javascript
    class Foo{
       private bar : number;
       
       get Bar() : number {
         return this.bar;
       }
       
       set Bar(value : number) {
         this.bar = value;
       }
    }
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Events

  - [24.1](#24.1) <a name='24.1'></a> Placeholder de AngularJS events

  **[⬆ voltar ao topo](#conteúdos)**


## jQuery

  - [25.1](#25.1) <a name='25.1'></a> Utilize o prefixo `$`.

    ```javascript
    // ruim
    const sidebar = $('.sidebar');

    // bom
    const $sidebar = $('.sidebar');
    ```

  - [25.2](#25.2) <a name='25.2'></a> Faça cache de buscas do jQuery.

    ```javascript
    // ruim
    function setSidebar() {

      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });

    }

    // bom
    function setSidebar() {

      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });

    }
    ```

  - [25.3](#25.3) <a name='25.3'></a> Para queries no DOM utilize cascading  `$('.sidebar ul')` ou parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [25.4](#25.4) <a name='25.4'></a> Utilize `find` com queries jQuery DISCUSS

    ```javascript
    // ruim
    $('ul', '.sidebar').hide();

    // ruim
    $('.sidebar').find('ul').hide();

    // bom
    $('.sidebar ul').hide();

    // bom
    $('.sidebar > ul').hide();

    // bom
    $sidebar.find('ul').hide();
    ```

**[⬆ voltar ao topo](#conteúdos)**


## Anotações de Tipo

<a name="ts-type-annotations"></a>
  - [26.1](#26.1) <a name='26.1'></a> Placeholder de anotações de tipos.

<a name="ts-generics"></a>
  - [26.2](#26.2) <a name='26.2'></a> Utilize `T` para o tipo da variável.

```javascript
function identify<T>(arg: T): T {

    return arg;
    
}
```

  - [26.3](#26.3) <a name='26.3'></a> Se mais de um tipo é necessário, começe por `T`e siga em ordem alfabética.

```javascript
function find<T, U extends Findable>(needle: T, haystack: U): U {

  return haystack.find(needle)

}
```

  - [26.4](#26.4) <a name='26.4'></a> Quando possível, utilize a inferência de tipos do Typescript.

```javascript
// ruim
const output = identify<string>("myString");

// bom
const output = identity("myString");
```

  - [26.5](#26.5) <a name='26.5'></a> Quando criar factories utilizando generics, não esqueça do construtor.

```javascript
function create<t>(thing: {new(): T;}): T {

  return new thing();

}
```

**[⬆ voltar ao topo](#conteúdos)**


## Interfaces

<a name="ts-interfaces"></a>
  - [27.1](#27.1) <a name='27.1'></a> Nomeie a interface sempre iniciando pela letra `I`.

**[⬆ voltar ao topo](#conteúdos)**


## Organização

<a name="ts-modules"></a>
  - [28.1](#28.1) <a name='28.1'></a> Placeholder

**[⬆ voltar ao topo](#conteúdos)**


## Compatibilidade com ECMAScript 5

  - [29.1](#29.1) <a name='29.1'></a> Utilize a tabela feita por [Kangax](https://twitter.com/kangax/) da  [tabela de compatibilidade ES5](http://kangax.github.com/es5-compat-table/).

**[⬆ voltar ao topo](#conteúdos)**

## ECMAScript 6

  - [30.1](#30.1) <a name='30.1'></a> ES6 Style Features

1. [Arrow Functions](#arrow-functions)
1. [Classes](#constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

**[⬆ voltar ao topo](#conteúdos)**

## Typescript 1.5

  - [31.1](#31.1) <a name='31.1'></a> ES6 Typescript Features

1. [Type Annotations](#ts-type-annotations)
1. [Interfaces](#ts-interfaces)
1. [Classes](#ts-classes)
1. [Modules](#ts-modules)
1. [Generics](#ts-generics)

**[⬆ voltar ao topo](#conteúdos)**

## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ voltar ao topo](#conteúdos)**
