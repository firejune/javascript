# Airbnb JavaScript 스타일 가이드() {

*자바 스크립트에 대한 가장 합리적인 접근 방식*

[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb.svg)](https://www.npmjs.com/package/eslint-config-airbnb)
[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

다른 스타일 가이드들
 - [ES5](es5/)
 - [React](react/)
 - [CSS & Sass](https://github.com/airbnb/css)
 - [Ruby](https://github.com/airbnb/ruby)

## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6 Styles](#ecmascript-6-styles)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Chat With Us About JavaScript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)

## Types

  - [1.1](#1.1) <a name='1.1'></a> **Primitives**: 원시형(Primitive type)은 그 값을 직접 조작합니다.

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
  - [1.2](#1.2) <a name='1.2'></a> **Complex**: 참조형(Complex type)은 참조를 통해 값을 조작합니다.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## References

  - [2.1](#2.1) <a name='2.1'></a> 모든 참조에는 `const`를 사용하고 `var`를 사용하지 않습니다.

    > 왜죠? 참조를 다시 할당할 수 없으므로, 버그로 연결되거나 이해하기 어려운 코드가 되는 것을 방지합니다.

    eslint rules: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html).

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> 참조를 다시 할당해야 하는 경우 `var` 대신에 `let`을 사용하세요.

    > 왜죠? `var`는 함수-범위(function-scoped)이고 `let`은 블록-범위(block-scoped)이기 때문입니다.

    eslint rules: [`no-var`](http://eslint.org/docs/rules/no-var.html).

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  - [2.3](#2.3) <a name='2.3'></a> `let`과 `const`는 모두 블록-범위(block-scoped)인 것에 주의해야 합니다.

    ```javascript
    // const와 let은 선언 된 블록 안에서만 존재함. 
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## Objects

  - [3.1](#3.1) <a name='3.1'></a> 개체를 만들 때에는 리터럴 구문을 사용합니다.

    eslint rules: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html).

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> 코드가 브라우저에서 실행되는 경우 [예약어](http://es5.github.io/#x7.6.1)를 키로 사용하지 마세요. 이것은 IE8에서 작동하지 않습니다. [More info](https://github.com/airbnb/javascript/issues/61). ES6 모듈과 서버 사이드에서는 사용할 수 있습니다.

    ```javascript
    // bad
    const superman = {
      default: { clark: 'kent' },
      private: true,
    };

    // good
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true,
    };
    ```

  - [3.3](#3.3) <a name='3.3'></a> 예약어 대신에 알기 쉬운 동의어(Readable synonyms)를 사용하세요.

    ```javascript
    // bad
    const superman = {
      class: 'alien',
    };

    // bad
    const superman = {
      klass: 'alien',
    };

    // good
    const superman = {
      type: 'alien',
    };
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> 동적인 속성 이름을 가진 객체를 만들 때에는 계산된 속성 이름(Computed property names)을 사용하세요.

    > 왜죠? 이렇게하면 객체 속성을 1개의 장소에서 정의 할 수 있습니다.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> 메소드에 단축 구문(Shorthand)을 사용하세요.

    eslint rules: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html).

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name='3.6'></a> 속성에 단축 구문을 사용하세요.

    > 왜죠? 표현이나 설명이 간결해지기 때문입니다.

    eslint rules: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html).

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

  - [3.7](#3.7) <a name='3.7'></a> 속성의 단축 구문은 객체 선언의 시작 부분에 무리를 지어줍니다.

    > 왜죠? 어떤 속성이 단축 구문을 사용하고 있는지를 알기가 쉽기 때문입니다.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  - [3.8](#3.8) <a name="3.8"></a> 이용한 속성에 작은 따옴표를 사용하는 경우는 오직 잘못된 식별자인 경우에만 사용합니다.

  > 왜죠? 주관적으로 쉽게 읽을 수 있는 것을 항상 고민해야 합니다. 이 것은 구문이 강조되고, 수많은 JS엔진에 쉽게 최적화되어 있습니다.

  eslint rules: [`quote-props`](http://eslint.org/docs/rules/quote-props.html).

  ```javascript
  // bad
  const bad = {
    'foo': 3,
    'bar': 4,
    'data-blah': 5,
  };

  // good
  const good = {
    foo: 3,
    bar: 4,
    'data-blah': 5,
  };
  ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - [4.1](#4.1) <a name='4.1'></a> 배열을 만들 때 리터럴 구문을 사용하세요.

    eslint rules: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html).

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> 배열에 항목을 직접 대체하지 말고 Array#push를 사용하세요.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> 배열을 복사하는 경우, 배열의 확장 연산자인 `...`을 사용하세요.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```
  - [4.4](#4.4) <a name='4.4'></a> array-like 객체를 배열로 변환하려면 Array#from을 사용하세요.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  - [5.1](#5.1) <a name='5.1'></a> 여러 속성에서 객체에 접근할 때 객체 구조화 대입을 사용하세요.

    > 왜죠? 구조화 대입을 이용하여 그 속성에 대한 중간 참조를 줄일 수 있습니다.

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  - [5.2](#5.2) <a name='5.2'></a> 배열의 구조 대입(Destructuring)을 사용하세요.

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  - [5.3](#5.3) <a name='5.3'></a> 여러 값을 반환하는 경우, 배열의 구조 대입이 아니라 객체의 구조 대입을 사용하세요.

    > 왜죠? 이렇게하면 나중에 새 속성을 추가하거나 호출에 영향을 주지않고 순서를 변경할 수 있습니다.

    ```javascript
    // bad
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    const { left, right } = processInput(input);
    ```


**[⬆ back to top](#table-of-contents)**

## Strings

  - [6.1](#6.1) <a name='6.1'></a> 문자열에는 작은 따옴표`''`를 사용하세요.

    eslint rules: [`quotes`](http://eslint.org/docs/rules/quotes.html).

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // good
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> 100자 이상의 문자열은 문자열 연결을 사용하여 여러 행에 걸쳐 기술할 수 있습니다.
  - [6.3](#6.3) <a name='6.3'></a> 주의: 문자열 연결이 많으면 성능에 영향을 줄 수 있습니다. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name='6.4'></a> 프로그램에서 문자열을 생성하는 경우, 문자열 연결이 아니라 template strings을 사용하세요.

    > 왜죠? Template strings 문자열 완성 기능과 다중 문자열 기능을 가진 간결한 구문으로 가독성이 좋아지기 때문입니다.

    eslint rules: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html).

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```
  - [6.5](#6.5) <a name='6.5'></a> 절대로 `eval()`을 사용하지 않습니다. 이것은 지금까지 수많은 취약점을 만들어 왔기 때문입니다.

**[⬆ back to top](#table-of-contents)**


## Functions

  - [7.1](#7.1) <a name='7.1'></a> 함수 선언 대신에 함수 표현식을 사용합니다.

    > 왜죠? 이름이 붙은 함수 선언은 콜스택에서 쉽게 알수 있습니다. 또한 함수 선언의 몸 전체가 Hoist됩니다. 반면 함수는 참조만 Hoist됩니다. 이 규칙은 함수 부분을 항상 [Arrow Functions](#arrow-functions)로 대체 사용할 수 있습니다.

    ```javascript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name='7.2'></a> 함수식(Function expressions):

    ```javascript
    // immediately-invoked function expression (IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name='7.3'></a> 함수 이외의 블록 (if 나 while 등)에 함수를 선언하지 마세요. 브라우저는 변수에 함수를 할당하는 것을 처리할 수는 있지만, 모두 다르게 해석됩니다.

  - [7.4](#7.4) <a name='7.4'></a> **주의:** ECMA-262에서 `block`은 statements 목록에 정의되지만, 함수 선언은 statements가 없습니다. [이 문제는 ECMA-262의 설명을 참조하세요](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  - [7.5](#7.5) <a name='7.5'></a> 매개변수(parameter)에 `arguments`를 절대 지정하지 않습니다. 이것은 함수 Scope로 전달 될 `arguments`객체의 참조를 덮어 써버릴 것입니다.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name='7.6'></a> `arguments`를 사용하지 않습니다. 대신 rest syntax인 `...`을 사용하세요.

    > 왜죠? `...` 를 이용하여 여러가지 매개변수를 모두 사용할 수 있습니다. 추가로 rest 매개변수인 `arguments`는 Array-like 객체가 아니라 진정한 Array입니다.

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> 함수의 매개변수를 조작하지 말고 기본 매개변수를 사용하세요.

    ```javascript
    // really bad
    function handleThings(opts) {
      // 안되! 함수의 매개변수를 조작하지 않습니다. 
      // 만약 opts가 falsy 인 경우는 바란대로 객체가 설정됩니다. 
      // 그러나 미묘한 버그를 일으키는 원인이 될수도 있습니다. 
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> 부작용이 있는 기본 매개변수를 사용하지 않습니다.

    > 왜죠? 혼란스럽기 때문입니다.

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  - [7.9](#7.9) <a name='7.9'></a> 항상 기본 매개변수는 앞쪽에 배치하세요.

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  - [7.10](#7.10) <a name='7.10'></a> 새로운 함수를 만드는 데 Function 생성자를 사용하지 않습니다.

    > 왜죠? 이 방법은 문자열을 구분하는 새로운 함수를 만들 수 있는 eval()과 같은 취약점이 발생할 수 있습니다.

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');
    ```

  - [7.11](#7.11) <a name="7.11"></a> 함수에 사용되는 공백

    > 왜죠? 일관성이 좋고, 함수이름을 추가 하거나 삭제할 때 공백을 제거할 필요가 없습니다.

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  - [7.12](#7.12) <a name="7.12"></a> 절대로 매개변수를 조작하지 않습니다.

    > 왜죠? 매개변수로 전달 된 객체를 조작하는 것은 원래의 호출에 원치 않는 변수 부작용을 일으킬 수 있습니다.

    eslint rules: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html).

    ```javascript
    // bad
    function f1(obj) {
      obj.key = 1;
    };

    // good
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    };
    ```

  - [7.13](#7.13) <a name="7.13"></a> 절대로 매개변수를 다시 지정하지 않습니다.

    > 왜죠? `arguments` 객체에 접근하는 경우 다시 지정된 매개변수는 예기치 않은 동작이 발생할 수 있습니다. 그리고 특히 V8 최적화에 문제가 발생할 수 있습니다.

    eslint rules: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html).

    ```javascript
    // bad
    function f1(a) {
      a = 1;
    }

    function f2(a) {
      if (!a) { a = 1; }
    }

    // good
    function f3(a) {
      const b = a || 1;
    }

    function f4(a = 1) {
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Arrow Functions

  - [8.1](#8.1) <a name='8.1'></a> (익명 함수 통과와 같은) 함수를 이용하는 경우, Arrow 함수를 사용하세요.

    > 왜죠? 애로우 함수는 함수가 실행되는 컨텍스트의 `this`를 속박합니다. 이것은 일반적으로 예상대로 작동하고 구문이 더 간결합니다.

    > 언제 쓰죠? 복잡한 함수 논리를 정의한 함수의 바깥쪽으로 이동하고 싶은 경우.

    eslint rules: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html).

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - [8.2](#8.2) <a name='8.2'></a> 함수의 본체가 하나의 식으로 구성되어있는 경우 중괄호를 생략하고 암묵적 return을 사용할 수 있습니다. 그렇지 않으면 `return` 문을 사용해야 합니다.

    > 왜죠? 가독성이 좋아지기 때문입니다. 여러 함수가 연결되는 경우에 쉽게 읽을 수 있습니다.

    > 언제 쓰죠? 객체를 반환하는 경우.

    eslint rules: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html).

    ```javascript
    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });
    ```

  - [8.3](#8.3) <a name='8.3'></a> 식의 길이가 여러 행에 걸치는 경우 가독성을 향상시키기 위해 괄호 안에 써주세요.

    > 왜죠? 함수의 시작과 끝 부분을 알아보기 쉽게 합니다.

    ```js
    // bad
    [1, 2, 3].map(number => 'As time went by, the string containing the ' +
      `${number} became much longer. So we needed to break it over multiple ` +
      'lines.'
    );

    // good
    [1, 2, 3].map(number => (
      `As time went by, the string containing the ${number} became much ` +
      'longer. So we needed to break it over multiple lines.'
    ));
    ```


  - [8.4](#8.4) <a name='8.4'></a> 함수의 인수가 1개인 경우 괄호를 생략할 수 있습니다.

    > 왜죠? 시각적 혼란이 덜하기 때문입니다.

    eslint rules: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html).

    ```js
    // bad
    [1, 2, 3].map((x) => x * x);

    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we’ve broken it ` +
      'over multiple lines!'
    ));

    // bad
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

**[⬆ back to top](#table-of-contents)**


## Constructors

  - [9.1](#9.1) <a name='9.1'></a> `prototype`의 직접 조작을 피하고 항상 `class`를 사용하세요.

    > 왜죠? `class` 구문은 간결하고 의도를 알아내기가 쉽기 때문입니다.

    ```javascript
    // bad
    function Queue(contents = []) {
      this._queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }


    // good
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

  - [9.2](#9.2) <a name='9.2'></a> 상속은 `extends`를 사용하세요.

    > 왜죠? 프로토타입을 상속하기 위해 내장된 방식으로 `instanceof` 를 파괴할 수 없기 때문입니다.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> 메소드의 반환 값에 `this`를 돌려주는 것으로, 메소드 체인을 할 수 있습니다.

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
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


  - [9.4](#9.4) <a name='9.4'></a> 사용자화 된 toString() 메소드를 쓰는 것도 좋습니다. 단, 제대로 작동하는지, 부작용이 없는 지를 꼭 확인하세요.

    ```javascript
    class Jedi {
      constructor(options = {}) {
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

**[⬆ back to top](#table-of-contents)**


## Modules

  - [10.1](#10.1) <a name='10.1'></a> 비표준 모듈 시스템이 아니라면 항상 (`import`/`export`) 를 사용하세요. 이렇게 함으로써 원하는 모듈 시스템에 언제든지 transpile 할 수 있습니다.

    > 왜죠? 모듈은 곧 미래입니다. 미래를 선점하고 사용합시다.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a> 와일드카드를 이용한 가져오기는 사용하지 않습니다.

    > 왜죠? single default export인 것에 주의할 필요가 있기 때문입니다.

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  - [10.3](#10.3) <a name='10.3'></a>import 문에서 직접 export하지 않습니다.

    > 왜죠? 한개의 라인이라 간결하기는 하지만, import와 export하는 방법을 명확하게 함으로써 일관성을 유지할 수 있습니다.

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

**[⬆ back to top](#table-of-contents)**

## Iterators and Generators

  - [11.1](#11.1) <a name='11.1'></a> iterators를 사용하지 않습니다. `for-of` 루프 대신  `map()`과 `reduce()`같은 Javascript의 고급 함수(higher-order functions)을 사용하세요.

    > 왜죠? 이것은 immutable(불변)의 규칙을 적용합니다. 값을 반환하는 함수를 처리하는 것이 부작용을 추측하기가 더 쉽습니다.

    eslint rules: [`no-iterator`](http://eslint.org/docs/rules/no-iterator.html).

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - [11.2](#11.2) <a name='11.2'></a> 현재 generators는 사용하지 않습니다.

    > 왜죠? ES5의 transpile이 잘 작동하지 않습니다.

**[⬆ back to top](#table-of-contents)**


## Properties

  - [12.1](#12.1) <a name='12.1'></a> 속성에 접근하려면 점을 사용하세요.

    eslint rules: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html).

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  - [12.2](#12.2) <a name='12.2'></a> 변수를 사용하여 속성에 접근하려면 대괄호`[]`를 사용하세요.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - [13.1](#13.1) <a name='13.1'></a> 변수를 선언 할 때는 항상 `const`를 사용하세요. 그렇지 않을 경우 전역 변수로 선언됩니다. 글로벌 네임 스페이스가 오염되지 않도록 캡틴 플래닛(역자주: 환경보호와 생태를 테마로 한 슈퍼히어로 애니메이션)도 경고하고 있습니다.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> 하나의 변수 선언에 대해 하나의 `const`를 사용하세요.

    > 왜죠? 이 방법은 새로운 변수를 쉽게 추가할 수 있습니다. 또한 구분 기호의 차이에 의한 `;`를 `,`로 다시금 대체하는 작업에 대해 신경쓸 필요가 없습니다.

    eslint rules: [`one-var`](http://eslint.org/docs/rules/one-var.html).

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> 먼저 `const`를 그룹화하고 그다음으로 `let`을 그룹화 하세요.

    > 왜죠? 이전에 할당 된 변수에 따라 나중에 새로운 변수를 추가하는 경우에 유용하기 때문입니다.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> 변수를 할당할 때는 필요하고 합리적인 장소에 넣습니다.

    > 왜죠? `let`과 `const`는 함수 Scope에는 없는 블록 Scope이기 때문입니다.

    ```javascript
    // good
    function () {
      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad - unnecessary function call
    function (hasName) {
      const name = getName();

      if (!hasName) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function (hasName) {
      if (!hasName) {
        return false;
      }

      const name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Hoisting

  - [14.1](#14.1) <a name='14.1'></a> `var` 선언은 할당이 없는 상태로 범위(Scope)의 위로 Hoist될 수 있습니다. 하지만 `const`와 `let` 선언은 시간적 데드 존([Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let))이라는 새로운 개념의 혜택을 받고 있습니다. 이것은 왜 typeof가 안전하지 않은가([typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15))를 알고있는 것이 중요합니다.

    ```javascript
    // (notDefined가 글로벌 변수에 존재하지 않는다고 가정했을 경우)
    // 이것은 잘 작동하지 않습니다. 
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 변수를 참조하는 코드 후에 그 변수를 선언한 경우
    // 변수가 Hoist되어서 작동합니다.
    // 주의: `true` 값 자체는 Hoist할 수 없습니다.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 인터프린터는 변수 선언을 Scope의 시작 부분에 Hoist합니다.
    // 위의 예는 다음과 같이 다시 작성할 수 있습니다:
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // const와 let을 사용하는 경우
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  - [14.2](#14.2) <a name='14.2'></a> 익명 함수 표현식에서는 함수가 할당되기 전에 변수가 Hoist될 수 있습니다.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> 명명된 함수의 경우도 마찬가지로 변수가 Hoist될 수 있습니다. 함수이름과 함수본문는 Hoist되지 않습니다.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 함수이름과 변수이름이 같은 경우에도 같은 일이 일어납니다.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - [14.4](#14.4) <a name='14.4'></a> 함수 선언은 함수이름과 함수본문이 Hoist됩니다.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 더 자세한 정보는 [Ben Cherry](http://www.adequatelygood.com/)의 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/)을 참조하세요.

**[⬆ back to top](#table-of-contents)**


## Comparison Operators & Equality

  - [15.1](#15.1) <a name='15.1'></a> `==`와 `!=` 보다는 `===`와 `!==`를 사용하세요.

  - [15.2](#15.2) <a name='15.2'></a> `if`와 같은 조건문은 `ToBoolean`방법에 의한 강제 형 변환으로 구분되고 항상 다음과 같은 간단한 규칙을 따릅니다:

    eslint rules: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html).

    + **Objects**는 **true**로 구분됩니다.
    + **Undefined**는 **false**로 구분됩니다.
    + **Null**은 **false**로 구분됩니다.
    + **Booleans**은 **boolean형의 값**으로 구분됩니다.
    + **Numbers**는 **true**로 구분됩니다. 그러나, **+0, -0, 또는 NaN**인 경우 **false**로 구분됩니다.
    + **Strings**는 **true**로 구분됩니다. 그러나, 비어있는 `''`경우는 **false**로 구분됩니다.

    ```javascript
    if ([0]) {
      // true
      // 배열은 객체이므로 true로 구분됩니다.
    }
    ```

  - [15.3](#15.3) <a name='15.3'></a> 손쉬운 방법을 사용하세요.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - [15.4](#15.4) <a name='15.4'></a> 더 자세한 내용은 여기를 참조하세요. [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**


## Blocks

  - [16.1](#16.1) <a name='16.1'></a> 여러 줄의 블록은 중괄호를 사용합니다.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function () { return false; }

    // good
    function () {
      return false;
    }
    ```

  - [16.2](#16.2) <a name='16.2'></a> 여러 블록에 걸친 `if`와 `else`를 사용하는 경우, `else`는 `if`블록의 끝 중괄호와 같은 행에 두세요.

    eslint rules: [`brace-style`](http://eslint.org/docs/rules/brace-style.html).

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ back to top](#table-of-contents)**


## Comments

  - [17.1](#17.1) <a name='17.1'></a> 여러 줄의 주석에는 `/** ... */`를 사용하세요. 그 안에는 설명과 모든 매개변수와 반환 값에 대한 형식과 값을 표기합니다.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - [17.2](#17.2) <a name='17.2'></a> 한 줄 주석에는 `//`를 사용하세요. 주석을 추가하싶은 코드의 상단에 배치하세요. 또한 주석 앞에 빈 줄을 넣어주세요.

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  - [17.3](#17.3) <a name='17.3'></a> 문제를 지적하고 재고를 촉구하거나 문제의 해결책을 제시하는 경우 등, 주석 앞에 `FIXME` 또는 `TODO` 를 붙이는 것으로 다른 개발자의 빠른 이해를 도울 수 있습니다. 이들은 어떠한 액션을 따른다는 의미에서 일반 댓글과 다를 수 있습니다. 액션은 `FIXME -- 해결책 필요` 또는 `TODO -- 구현 필요`.

  - [17.4](#17.4) <a name='17.4'></a> 문제에 대한 주석으로 `// FIXME:`를 사용하세요.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn't use a global here
        total = 0;
      }
    }
    ```

  - [17.5](#17.5) <a name='17.5'></a> 해결책에 대한 주석으로 `// TODO:`를 사용하세요.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  - [18.1](#18.1) <a name='18.1'></a> 탭에 공백 2개를 설정하세요.

    eslint rules: [`indent`](http://eslint.org/docs/rules/indent.html).

    ```javascript
    // bad
    function () {
    ∙∙∙∙const name;
    }

    // bad
    function () {
    ∙const name;
    }

    // good
    function () {
    ∙∙const name;
    }
    ```

  - [18.2](#18.2) <a name='18.2'></a> 중괄호 앞에 공백을 넣어주세요.

    eslint rules: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html).

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  - [18.3](#18.3) <a name='18.3'></a> 제어 구문(`if`, `while` 등)의 괄호 앞에 공백을 넣어주세요. 함수 선언과 함수 호출시 인수 목록 앞에는 공백을 넣지 않습니다.

    eslint rules: [`space-after-keywords`](http://eslint.org/docs/rules/space-after-keywords.html), [`space-before-keywords`](http://eslint.org/docs/rules/space-before-keywords.html).

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - [18.4](#18.4) <a name='18.4'></a> 연산자 사이에는 공백이 있습니다.

    eslint rules: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html).

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  - [18.5](#18.5) <a name='18.5'></a> 파일의 마지막에 빈 줄을 하나 넣어주세요.

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function (global) {
      // ...stuff...
    })(this);↵
    ```

  - [18.6](#18.6) <a name='18.6'></a> 메소드 체인이 길어지는 경우 적절히 들여쓰기(indentation) 하세요. 행이 메소드 호출이 아닌 새로운 문장임을 강조하기 위해 선두에 점을 배치하세요.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - [18.7](#18.7) <a name='18.7'></a> 블록과 다음 Statement 사이에 빈 줄을 넣어주세요.

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  - [18.8](#18.8) <a name='18.8'></a> 블록에 빈 줄을 끼워넣지 않습니다.

    eslint rules: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html).

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // also bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  - [18.9](#18.9) <a name='18.9'></a> 괄호 안에 공백을 추가하지 않습니다.

    eslint rules: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html).

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  - [18.10](#18.10) <a name='18.10'></a> 대괄호 안에 공백을 추가하지 않습니다.

    eslint rules: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html).

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  - [18.11](#18.11) <a name='18.11'></a> 중괄호 안에 공백을 추가하지 않습니다.

    eslint rules: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html).

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

  - [18.12](#18.12) <a name='18.12'></a> 한 줄에 100문자(공백 포함)가 넘는 코드는 피하세요.

    > 왜죠? 가독성과 유지 보수성을 보장합니다.

    eslint rules: [`max-len`](http://eslint.org/docs/rules/max-len.html).

    ```javascript
    // bad
    const foo = 'Whatever national crop flips the window. The cartoon reverts within the screw. Whatever wizard constrains a helpful ally. The counterpart ascends!';

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = 'Whatever national crop flips the window. The cartoon reverts within the screw. ' +
      'Whatever wizard constrains a helpful ally. The counterpart ascends!';

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - [19.1](#19.1) <a name='19.1'></a> 쉼표로 시작: **제발 그만하세요.**

    eslint rules: [`comma-style`](http://eslint.org/docs/rules/comma-style.html).

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [19.2](#19.2) <a name='19.2'></a> 마지막에 쉼표: **좋습니다.**

    eslint rules: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html).

    > 왜죠? 이것은 깨끗한 git의 차이로 이어집니다. 또한 Babel관 같은 트랜스 컴파일러는 끝에 불필요한 쉼표를 알아서 제거합니다. 이것은 기존 브라우저에서 [불필요한 쉼표 문제](es5/README.md#commas)를 걱정할 필요가 없다는 것을 의미합니다.

    ```javascript
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'modern nursing']
    };

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - [20.1](#20.1) <a name='20.1'></a> **물론 사용합시다.**

    eslint rules: [`semi`](http://eslint.org/docs/rules/semi.html).

    ```javascript
    // bad
    (function () {
      const name = 'Skywalker'
      return name
    })()

    // good
    (() => {
      const name = 'Skywalker';
      return name;
    })();

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(() => {
      const name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

  - [21.1](#21.1) <a name='21.1'></a> 문장의 시작 부분에서 형을 강제합니다.
  - [21.2](#21.2) <a name='21.2'></a> String:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - [21.3](#21.3) <a name='21.3'></a> Number: `Number`로 형 변환하려면 `parseInt`를 사용하세요. 항상 형변환을 위한 기수(radix)를 인수로 전달합니다.

    eslint rules: [`radix`](http://eslint.org/docs/rules/radix).

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  - [21.4](#21.4) <a name='21.4'></a> 어떤 이유로 `parseInt`가 병목이되고, [성능적인 이유](http://jsperf.com/coercion-vs-casting/3)에서 Bitshift를 사용해야 하는 경우, 무엇을(what) 왜(why)에 대한 설명을 댓글로 남겨 주세요.

    ```javascript
    // good
    /**
     * parseInt가 병목이되고 있었기 때문에, 
     * Bitshift 문자열을 수치로 강제로 변환하여 
     * 성능을 향상시킵니다.
     */
    const val = inputValue >> 0;
    ```

  - [21.5](#21.5) <a name='21.5'></a> **주의:** Bitshift를 사용하는 경우 수치는 [64-비트 값들](http://es5.github.io/#x4.3.19)로 표현되어 있지만, Bitshift를 연산하면 항상 32-비트 단 정밀도로 돌려 주어집니다([source](http://es5.github.io/#x11.7)). 32-비트 이상의 값을 비트 이동하면 예상치 못한 행동을 일으킬 가능성이 있습니다. [Discussion](https://github.com/airbnb/javascript/issues/109). 부호있는 32-비트 정수의 최대 값은 2,147,483,647입니다:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - [21.6](#21.6) <a name='21.6'></a> Booleans:

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - [22.1](#22.1) <a name='22.1'></a> 하나의 문자로 구성된 이름은 피하세요. 이름에서 의도를 읽을 수 있도록 해야 합니다.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - [22.2](#22.2) <a name='22.2'></a> 객체, 함수 인스턴스에는 camelCase(소문자로 시작)를 사용하세요.

    eslint rules: [`camelcase`](http://eslint.org/docs/rules/camelcase.html).

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - [22.3](#22.3) <a name='22.3'></a> 클래스와 생성자는 PascalCase(대문자로 시작)를 사용하세요.

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  - [22.4](#22.4) <a name='22.4'></a> Private 속성 이름은 앞에 밑줄`_`을 사용하세요.

    eslint rules: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html).

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - [22.5](#22.5) <a name='22.5'></a> `this`에 대한 참조를 저장하지 않습니다. Arrow 함수 또는 Function#bind를 사용하세요.

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - [22.6](#22.6) <a name='22.6'></a> 파일을 하나의 클래스로 export 할 경우 파일 이름은 클래스 이름과 정확하게 일치해야 합니다.

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // in some other file
    // bad
    import CheckBox from './checkBox';

    // bad
    import CheckBox from './check_box';

    // good
    import CheckBox from './CheckBox';
    ```

  - [22.7](#22.7) <a name='22.7'></a> export-default 함수의 경우, camelCase(소문자로 시작)를 사용하세요. 파일이름은 함수이름과 동일해야 합니다.

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - [22.8](#22.8) <a name='22.8'></a> 싱글톤(singleton) / function library / 단순한 객체(bare object)를 export하는 경우, PascalCase(대문자로 시작)를 사용하세요.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```


**[⬆ back to top](#table-of-contents)**


## Accessors

  - [23.1](#23.1) <a name='23.1'></a> 속성에 대한 접근자(Accessor) 함수는 필요하지 않습니다.
  - [23.2](#23.2) <a name='23.2'></a> 접근자 함수가 필요한 경우 `getVal()`과 `setVal('hello')`로 하세요.

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - [23.3](#23.3) <a name='23.3'></a> 속성이 `boolean`의 경우 `isVal()` 또는 `hasVal()`로 하세요.

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - [23.4](#23.4) <a name='23.4'></a> 일관된 것이라면, `get()`과 `set()` 함수를 만들수 있습니다.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Events

  - [24.1](#24.1) <a name='24.1'></a> (DOM 이벤트, Backbone 이벤트)처럼 자신의 이벤트 페이로드 값을 전달하려면 원시값 대신 해시인수를 전달합니다. 이렇게 하면 나중에 개발자가 이벤트에 관련된 모든 핸들러를 찾아 업데이트하지 않고 이벤트 페이로드에 값을 추가할 수 있습니다. 예를 들면:

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function (e, listingId) {
      // do something with listingId
    });
    ```

    이쪽이 더 선호됨:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId: listing.id });

    ...

    $(this).on('listingUpdated', function (e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**


## jQuery

  - [25.1](#25.1) <a name='25.1'></a> jQuery 개체 변수 앞에 `$`를 부여합니다.

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');

    // good
    const $sidebarBtn = $('.sidebar-btn');
    ```

  - [25.2](#25.2) <a name='25.2'></a> jQuery의 검색 결과를 캐시합니다.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - [25.3](#25.3) <a name='25.3'></a> DOM의 검색에는 `$('.sidebar ul')` 또는 `$('.sidebar > ul')`과 같은 Cascading을 사용하세요. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [25.4](#25.4) <a name='25.4'></a> jQuery 객체의 검색에는 범위가있는 `find` 를 사용하세요.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**


## ECMAScript 5 Compatibility

  - [26.1](#26.1) <a name='26.1'></a> [Kangax](https://twitter.com/kangax/)의 ES5 [호환성 표](http://kangax.github.io/es5-compat-table/)를 참조하세요.

**[⬆ back to top](#table-of-contents)**

## ECMAScript 6 Styles

  - [27.1](#27.1) <a name='27.1'></a> 이것은 ES6 명세 링크를 모아 놓은 것입니다.

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

**[⬆ back to top](#table-of-contents)**

## Testing

  - [28.1](#28.1) <a name="28.1"></a> **물론 해야 합니다.**

    ```javascript
    function () {
      return true;
    }
    ```

  - [28.2](#28.2) <a name="28.2"></a> **물론 심각하게**:
   - 대부분 테스트 프레임워크를 이용하여 테스트를 작성합니다.
   - 작은 기능의 함수를 자주 쓰고 이변이 발생할 수 있는 부분을 최소화하기 위해 노력합니다.
   - stubs와 mocks에 주의하세요. 이 것들로 인해 테스트가 부서지기 쉽습니다.
   - Airbnb는 [`mocha`](https://www.npmjs.com/package/mocha)를 이용하고 있습니다. 작게 분할된 개별 모듈은 [`tape`](https://www.npmjs.com/package/tape) 사용합니다.
   - 현실에서 도달할 필요가 없어도 100%의 테스트 커버리지를 목표로하는 것이 좋습니다.
   - 버그를 수정할 때 마다 _회귀 테스트를 씁니다_. 회귀 테스트 없는 버그 수정은 나중에 반드시 다시 깨어날 것입니다.

**[⬆ back to top](#table-of-contents)**


## Performance

  - [On Layout & Web Performance](http://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ back to top](#table-of-contents)**


## Resources

**Learning ES6**

  - [Draft ECMA 2015 (ES6) Spec](https://people.mozilla.org/~jorendorff/es6-draft.html)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**Tools**

  - Code Style Linters
    + [ESlint](http://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    + [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Other Style Guides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
  - [You Don't Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](https://bocoup.com/weblog)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](https://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://code.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](https://devchat.tv/js-jabber/)


**[⬆ back to top](#table-of-contents)**

## In the Wild

  이 것은 본 스타일 가이드를 사용하는 조직의 목록입니다. 이 목록에 추가하고 싶다면, pull request를 하거나 issue를 통해 알려주세요.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Bisk**: [bisk/javascript](https://github.com/Bisk/javascript/)
  - **Blendle**: [blendle/javascript](https://github.com/blendle/javascript)
  - **ComparaOnline**: [comparaonline/javascript](https://github.com/comparaonline/javascript-style-guide)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Ecosia**: [ecosia/javascript](https://github.com/ecosia/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **Evolution Gaming**: [evolution-gaming/javascript](https://github.com/evolution-gaming/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Expensify** [Expensify/Style-Guide](https://github.com/Expensify/Style-Guide/blob/master/javascript.md)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript-style-guide)
  - **Huballin**: [huballin/javascript](https://github.com/huballin/javascript)
  - **HubSpot**: [HubSpot/javascript](https://github.com/HubSpot/javascript)
  - **Hyper**: [hyperoslo/javascript-playbook](https://github.com/hyperoslo/javascript-playbook/blob/master/style.md)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JeopardyBot**: [kesne/jeopardy-bot](https://github.com/kesne/jeopardy-bot/blob/master/STYLEGUIDE.md)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/Javascript-style-guide)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **MitocGroup**: [MitocGroup/javascript](https://github.com/MitocGroup/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **OutBoxSoft**: [OutBoxSoft/javascript](https://github.com/OutBoxSoft/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **React**: [/facebook/react/blob/master/CONTRIBUTING.md#style-guide](https://github.com/facebook/react/blob/master/CONTRIBUTING.md#style-guide)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Springload**: [springload/javascript](https://github.com/springload/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/guide-javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ back to top](#table-of-contents)**

## Translation

  이 스타일 가이드는 다른 언어로도 이용할 수 있습니다.

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [sivan/javascript-style-guide](https://github.com/sivan/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Chat With Us About JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


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

**[⬆ back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };
