# Airbnb React/JSX 스타일 가이드

*React와 JSX에 대한 가장 합리적인 접근 방식*

## Table of Contents

  1. [기본 규칙(Basic Rules)](#기본-규칙basic-rules)
  1. [클래스 대 `React.createClass`(Class vs `React.createClass`)](#클래스-대-reactcreateclassclass-vs-reactcreateclass)
  1. [명명(Naming)](#명명naming)
  1. [선언(Declaration)](#선언declaration)
  1. [조정(Alignment)](#조정alignment)
  1. [인용(Quotes)](#인용quotes)
  1. [공백(Spacing)](#공백spacing)
  1. [속성(Props)](#속성props)
  1. [괄호(Parentheses)](#괄호parentheses)
  1. [태그(Tags)](#태그tags)
  1. [메소드(Methods)](#메소드methods)
  1. [오더링(Ordering)](#오더링ordering)
  1. [`isMounted`](#ismounted)

## 기본 규칙(Basic Rules)

  - 하나의 파일에는 오직 하나의 React 컴포넌트를 사용합니다.
    - 그러나, 다중 [스테이트가 없는(Stateless) 또는 순수한 함수나 컴포넌트](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions)는 허용됩니다. eslint rule: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - 항상 JSX 문법을 사용합니다.
  - JSX파일이 아닌 다른 app에서 초기화하는 경우를 제외하고는 `React.createElement`를 사용하지 않습니다.

## 클래스 대 `React.createClass`(Class vs `React.createClass`)

  - 특별한 이유로 믹스인(mixin)하는 경우를 제외하고는 `class extends React.Component`를 사용하세요.

  eslint rules: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md).

    ```javascript
    // bad
    const Listing = React.createClass({
      render() {
        return <div />;
      }
    });

    // good
    class Listing extends React.Component {
      render() {
        return <div />;
      }
    }
    ```

## 명명(Naming)

  - **확장자**: React 컴포넌트는 `.jsx` 확장자를 사용합니다.
  - **파일명**: 파일명에는 PascalCase(대문자로 시작)를 사용합니다. 예), `ReservationCard.jsx`.
  - **참조명**: React 컴포넌트의 참조 이름에는 PascalCase를 쓰고 그 인스턴스의 이름에는 camelCase(소문자로 시작)를 사용합니다.

  eslint rules: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md).

    ```javascript
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **컴포넌트명**: 컴포넌트명으로 파일명을 씁니다. 예), `ReservationCard.jsx` 파일은 `ReservationCard`라는 참조명을 가집니다. 그러나, 루트 컴포넌트가 디렉토리에 구성되었다면 파일명을 `index.jsx`로 쓰고 디렉토리명을 컴포넌트명으로 사용합니다:

    ```javascript
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

## 선언(Declaration)

  - `displayName`을 이용하여 컴포넌트명을 정하지 않습니다. 그대신, 참조에 의해 이름을 지정합니다.

    ```javascript
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

## 조정(Alignment)

  - JSX 구문에 따른 정렬 스타일을 사용합니다.

  eslint rules: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md).

    ```javascript
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Spazz />
    </Foo>
    ```

## 인용(Quotes)

  - JSX 속성(attributes)에는 항상 큰 따옴표(`"`)를 사용합니다. 그러나 다른 모든 자바스크립트에는 작은 따옴표(single quotes)를 사용합니다.

  > 왜죠? JSX 속성(attributes)은 [따옴표(quotes)의 탈출(escaped)을 포함할 수 없습니다](http://eslint.org/docs/rules/jsx-quotes). 그래서 큰 따옴표를 이용하여 `"don't"`과 같은 접속사를 쉽게 입력할 수 있습니다.
  > 일반적으로 HTML 속성(attributes)에는 작은 따옴표 대신 큰 따옴표를 사용합니다. 그래서 JSX 속성역시 동일한 규칙이 적용됩니다.

  eslint rules: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes).

    ```javascript
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## 공백(Spacing)

  - 자신을 닫는(self-closing) 태그에는 항상 하나의 공백만을 사용합니다.

    ```javascript
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

## 속성(Props)

  - prop 이름은 항상 camelCase(소문자로 시작)를 사용합니다.

    ```javascript
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - 명시적으로 `true` 값을 가지는 prop은 그 값을 생략할 수 있습니다.

  eslint rules: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md).

    ```javascript
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />
    ```

## 괄호(Parentheses)

  - JSX 태그가 감싸여(Wrap) 있어 한 줄 이상인 경우 괄호(parentheses)를 사용합니다.

  eslint rules: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md).

    ```javascript
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## 태그(Tags)

  - 자식(children)을 가지지 않는다면 항상 자신을 닫는(self-close) 태그로 작성합니다.

  eslint rules: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md).

    ```javascript
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - 만약, 컴포넌트의 속성(properties)을 여러 줄에 있는 경우, 닫는 태그는 다음 줄에 작성합니다.

  eslint rules: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md).

    ```javascript
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## 메소드(Methods)

  - 렌더러 메소드에서 이벤트 핸들러에 바인드(Bind)가 필요한 경우에는 생성자(constructor)에서 합니다.

  > 왜죠? 렌더러 메소드에서 바인드(bind)를 호출하게 되면 랜더링 할 때 마다 매번 새로운 함수를 생성하게 됩니다.

  eslint rules: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md).

    ```javascript
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - React 컴포넌트의 내부 메소드에 밑줄(underscore)을 접두사로 사용하지 않습니다.

    ```javascript
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

## 오더링(Ordering)

  - `class extends React.Component`의 오더링(Ordering):

  1. `constructor`
  1. 추가적인(optional) `static` 메소드
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. `onClickSubmit()`와 같은 *clickHandlers 또는 eventHandlers* 또는 `onChangeDescription()`
  1. `getSelectReason()`와 같은 *`render`를 위한 getter methods* 또는 `getFooterContent()`
  1. `renderNavigation()`와 같은 *추가적인 렌더러 메소드* 또는 `renderProfilePicture()`
  1. `render`

  - `propTypes`, `defaultProps`, `contextTypes`, 등을 어떻게 정의할까요...

    ```javascript
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - `React.createClass`의 오더링(Ordering):

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. `onClickSubmit()`와 같은 *clickHandlers 또는 eventHandlers* 또는 `onChangeDescription()`
  1. `getSelectReason()`와 같은 *`render`를 위한 getter methods* 또는 `getFooterContent()`
  1. `renderNavigation()`와 같은 *추가적인 렌더러 메소드* 또는 `renderProfilePicture()`
  1. `render`

  eslint rules: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md).

## `isMounted`

  - `isMounted`는 사용하지 않습니다.

  > 왜죠? [`isMounted`는 안티-패턴(anti-pattern)][anti-pattern]입니다. ES6 클래스에서는 사용할수도 없습니다. 그리고 공식적으로 사용되지 않게(deprecated) 될 것입니다.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

  eslint rules: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md).

**[⬆ back to top](#table-of-contents)**
