# Sass

- Sass(Syntactically Awesome Style Sheets)는 CSS pre-processor 로서, 복잡한 작업을 쉽게 할 수 있게 해주고, 코드의 재활용성을 높여줄 뿐 만 아니라, 코드의 가독성을 높여주어 유지보수를 쉽게해준다.

  > 참고 : [VELOPERT Sass](https://velopert.com/1712) , [Sass Guidelines](https://sass-guidelin.es/ko/)

- Sass는 두가지 확장자(.scss / .sass)를 지원하는데 문법이 다름.

```scss
/* sass */
$font-stack: Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

```scss
/* sass */
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

**.scss 확장자로 스타일 작성**

# Scss(Sass) Install

```bash
> npm i sass
```

# Button 컴포넌트 만들기

```javascript
// components/Button.js
import React from "react";
import "./Button.scss";

function Button({ children }) {
  return <button className="Button">{children}</button>;
}

export default Button;
```

```scss
// components/Button.js
$blue: #228be6; // 주석 선언

.Button {
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  height: 2.25rem;
  padding-left: 1rem;
  padding-right: 1rem;
  font-size: 1rem;

  background: $blue; // 주석 사용
  &:hover {
    background: lighten($blue, 10%); // 색상 10% 밝게
  }

  &:active {
    background: darken($blue, 10%); // 색상 10% 어둡게
  }
}
```

# Button 컴포넌트 사용

```javascript
// App.js
import React from "react";
import "./App.scss";
import Button from "./components/Button";

function App() {
  return (
    <div className="App">
      <div className="buttons">
        <Button>BUTTON</Button>
      </div>
    </div>
  );
}

export default App;
```

```scss
// App.scss
.App {
  width: 512px;
  margin: 0 auto;
  margin-top: 4rem;
  border: 1px solid black;
  padding: 1rem;
}
```

# Button 사이즈 조정하기

- 버튼 크기에 `large`, `medium`, `small` 을 설정해줄 수 있도록 구현

```javascript
// components/Button.js
import React from "react";
import "./Button.scss";

function Button({ children, size }) {
  return <button className={["Button", size].join(" ")}>{children}</button>;
}

Button.defaultProps = {
  size: "medium",
};

export default Button;
```

- className 에 CSS 클래스 이름을 동적으로 넣어주고 싶다면

```javascript
className={['Button', size].join(' ')};
// or
className={`Button ${size}`};
```

- 하지만, 조건부로 CSS 클래스를 넣어주고 싶을때 이렇게 문자열을 직접 조합해주는 것 보다 classnames 라는 라이브러리를 사용하는 것이 훨씬 편함
  - ex)
  ```javascript
  classNames("foo", "bar"); // => 'foo bar'
  classNames("foo", { bar: true }); // => 'foo bar'
  classNames({ "foo-bar": true }); // => 'foo-bar'
  classNames({ "foo-bar": false }); // => ''
  classNames({ foo: true }, { bar: true }); // => 'foo bar'
  classNames({ foo: true, bar: true }); // => 'foo bar'
  classNames(["foo", "bar"]); // => 'foo bar'
  // 동시에 여러개의 타입으로 받아올 수 도 있습니다.
  classNames("foo", { bar: true, duck: false }, "baz", { quux: true }); // => 'foo bar baz quux'
  // false, null, 0, undefined 는 무시됩니다.
  classNames(null, false, "bar", undefined, 0, 1, { baz: null }, ""); // => 'bar 1'
  ```

# classNames Install

```bash
> npm i classnames
```

## Usage

```javascript
// components/Button.js
import React from "react";
import classNames from "classnames";
import "./Button.scss";

function Button({ children, size }) {
  return <button className={classNames("Button", size)}>{children}</button>;
}

Button.defaultProps = {
  size: "medium",
};

export default Button;
```

- props 로 받은 props 값이 button 태그의 className 으로 전달됨
- Button.scss 에서 다른 크기를 지정하기

```scss
// components/Button.scss
$blue: #228be6;

.Button {
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  // 사이즈 관리
  &.large {
    height: 3rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1.25rem;
  }

  &.medium {
    height: 2.25rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1rem;
  }

  &.small {
    height: 1.75rem;
    font-size: 0.875rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  background: $blue;
  &:hover {
    background: lighten($blue, 10%);
  }

  &:active {
    background: darken($blue, 10%);
  }

  // .Button + .Button
  // 만약 함께 있다면 우측에 있는 버튼에 여백을 설정
  & + & {
    margin-left: 1rem;
  }
}
```

```javascript
// App.js
import React from "react";
import "./App.scss";
import Button from "./components/Button";

function App() {
  return (
    <div className="App">
      <div className="buttons">
        <Button size="large">BUTTON</Button>
        <Button>BUTTON</Button>
        <Button size="small">BUTTON</Button>
      </div>
    </div>
  );
}

export default App;
```

# 버튼 색상 설정

- 버튼에 파란색 외의 다른 색상을 설정하는 작업
- Button 에서 color 라는 props 를 받아올 수 있도록 해주고, 기본 값을 blue 로 설정
- size 와 마찬가지로 color 값을 className 에 포함

```javascript
// components/Button.js
import React from "react";
import classNames from "classnames";
import "./Button.scss";

function Button({ children, size, color }) {
  return (
    <button className={classNames("Button", size, color)}>{children}</button>
  );
}

Button.defaultProps = {
  size: "medium",
  color: "blue",
};

export default Button;
```

```scss
// components/Button.scss
$blue: #228be6;
$gray: #495057;
$pink: #f06595;

.Button {
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  // 사이즈 관리
  &.large {
    height: 3rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1.25rem;
  }

  &.medium {
    height: 2.25rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1rem;
  }

  &.small {
    height: 1.75rem;
    font-size: 0.875rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  // 색상 관리
  &.blue {
    background: $blue;
    &:hover {
      background: lighten($blue, 10%);
    }

    &:active {
      background: darken($blue, 10%);
    }
  }

  &.gray {
    background: $gray;
    &:hover {
      background: lighten($gray, 10%);
    }

    &:active {
      background: darken($gray, 10%);
    }
  }

  &.pink {
    background: $pink;
    &:hover {
      background: lighten($pink, 10%);
    }

    &:active {
      background: darken($pink, 10%);
    }
  }

  & + & {
    margin-left: 1rem;
  }
}
```

- 위 의 반복되는 코드를 Scss `mixin()`을 통해 재사용 가능

```scss
// components/Button.scss
$blue: #228be6;
$gray: #495057;
$pink: #f06595;

@mixin button-color($color) {
  background: $color;
  &:hover {
    background: lighten($color, 10%);
  }
  &:active {
    background: darken($color, 10%);
  }
}

.Button {
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  // 사이즈 관리
  &.large {
    height: 3rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1.25rem;
  }

  &.medium {
    height: 2.25rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1rem;
  }

  &.small {
    height: 1.75rem;
    font-size: 0.875rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  // 색상 관리
  &.blue {
    @include button-color($blue);
  }

  &.gray {
    @include button-color($gray);
  }

  &.pink {
    @include button-color($pink);
  }

  & + & {
    margin-left: 1rem;
  }
}
```

- App 컴포넌트에서 다음과 같이 다른 색상을 가진 버튼들도 렌더링

```javascript
// App.js
import React from "react";
import "./App.scss";
import Button from "./components/Button";

function App() {
  return (
    <div className="App">
      <div className="buttons">
        <Button size="large">BUTTON</Button>
        <Button>BUTTON</Button>
        <Button size="small">BUTTON</Button>
      </div>
      <div className="buttons">
        <Button size="large" color="gray">
          BUTTON
        </Button>
        <Button color="gray">BUTTON</Button>
        <Button size="small" color="gray">
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" color="pink">
          BUTTON
        </Button>
        <Button color="pink">BUTTON</Button>
        <Button size="small" color="pink">
          BUTTON
        </Button>
      </div>
    </div>
  );
}

export default App;
```

- 버튼 상하 여백 조정

```scss
// App.scss
.App {
  width: 512px;
  margin: 0 auto;
  margin-top: 4rem;
  border: 1px solid black;
  padding: 1rem;
  .buttons + .buttons {
    margin-top: 1rem;
  }
}
```

# outline 옵션 만들기

```javascript
// components/Button.js
import React from "react";
import classNames from "classnames";
import "./Button.scss";

function Button({ children, size, color, outline }) {
  return (
    <button className={classNames("Button", size, color, { outline })}>
      {children}
    </button>
  );
}

Button.defaultProps = {
  size: "medium",
  color: "blue",
};

export default Button;
```

- outline 값을 props 로 받아와서 객체 안에 집어 넣은 다음에 classNames() 에 포함시켜줌,
- 이렇게 하면 outline 값이 true 일 때에만 button 에 outline CSS 클래스가 적용 <br><br><br>

- 만약 outline CSS 클래스가 있다면, 테두리만 보여지도록 수정

```scss
// components/Button.scss
$blue: #228be6;
$gray: #495057;
$pink: #f06595;

@mixin button-color($color) {
  background: $color;
  &:hover {
    background: lighten($color, 10%);
  }
  &:active {
    background: darken($color, 10%);
  }
  &.outline {
    color: $color;
    background: none;
    border: 1px solid $color;
    &:hover {
      background: $color;
      color: white;
    }
  }
}

.Button {
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  // 사이즈 관리
  &.large {
    height: 3rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1.25rem;
  }

  &.medium {
    height: 2.25rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1rem;
  }

  &.small {
    height: 1.75rem;
    font-size: 0.875rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  // 색상 관리
  &.blue {
    @include button-color($blue);
  }

  &.gray {
    @include button-color($gray);
  }

  &.pink {
    @include button-color($pink);
  }

  & + & {
    margin-left: 1rem;
  }
}
```

- App.js 에서 사용하기

```javascript
// App.js
import React from "react";
import "./App.scss";
import Button from "./components/Button";

function App() {
  return (
    <div className="App">
      <div className="buttons">
        <Button size="large">BUTTON</Button>
        <Button>BUTTON</Button>
        <Button size="small">BUTTON</Button>
      </div>
      <div className="buttons">
        <Button size="large" color="gray">
          BUTTON
        </Button>
        <Button color="gray">BUTTON</Button>
        <Button size="small" color="gray">
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" color="pink">
          BUTTON
        </Button>
        <Button color="pink">BUTTON</Button>
        <Button size="small" color="pink">
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" color="blue" outline>
          BUTTON
        </Button>
        <Button color="gray" outline>
          BUTTON
        </Button>
        <Button size="small" color="pink" outline>
          BUTTON
        </Button>
      </div>
    </div>
  );
}

export default App;
```

# 전체 너비 차지하는 옵션

- fullWidth 라는 옵션이 있으면 버튼이 전체 너비를 차지하도록 구현

```javascript
// components/Button.js
import React from "react";
import classNames from "classnames";
import "./Button.scss";

function Button({ children, size, color, outline, fullWidth }) {
  return (
    <button
      className={classNames("Button", size, color, { outline, fullWidth })}
    >
      {children}
    </button>
  );
}

Button.defaultProps = {
  size: "medium",
  color: "blue",
};

export default Button;
```

```scss
// components/Button.scss
$blue: #228be6;
$gray: #495057;
$pink: #f06595;

@mixin button-color($color) {
  background: $color;
  &:hover {
    background: lighten($color, 10%);
  }
  &:active {
    background: darken($color, 10%);
  }
  &.outline {
    color: $color;
    background: none;
    border: 1px solid $color;
    &:hover {
      background: $color;
      color: white;
    }
  }
}

.Button {
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  // 사이즈 관리
  &.large {
    height: 3rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1.25rem;
  }

  &.medium {
    height: 2.25rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1rem;
  }

  &.small {
    height: 1.75rem;
    font-size: 0.875rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  // 색상 관리
  &.blue {
    @include button-color($blue);
  }

  &.gray {
    @include button-color($gray);
  }

  &.pink {
    @include button-color($pink);
  }

  & + & {
    margin-left: 1rem;
  }

  &.fullWidth {
    width: 100%;
    justify-content: center;
    & + & {
      margin-left: 0;
      margin-top: 1rem;
    }
  }
}
```

- App.js에서 사용

```javascript
// App.js
import React from "react";
import "./App.scss";
import Button from "./components/Button";

function App() {
  return (
    <div className="App">
      <div className="buttons">
        <Button size="large">BUTTON</Button>
        <Button>BUTTON</Button>
        <Button size="small">BUTTON</Button>
      </div>
      <div className="buttons">
        <Button size="large" color="gray">
          BUTTON
        </Button>
        <Button color="gray">BUTTON</Button>
        <Button size="small" color="gray">
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" color="pink">
          BUTTON
        </Button>
        <Button color="pink">BUTTON</Button>
        <Button size="small" color="pink">
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" color="blue" outline>
          BUTTON
        </Button>
        <Button color="gray" outline>
          BUTTON
        </Button>
        <Button size="small" color="pink" outline>
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" fullWidth>
          BUTTON
        </Button>
        <Button size="large" fullWidth color="gray">
          BUTTON
        </Button>
        <Button size="large" fullWidth color="pink">
          BUTTON
        </Button>
      </div>
    </div>
  );
}

export default App;
```

# ...rest props 전달하기

- 컴포넌트에 onClick 을 설정

```javascript
// components/Button.js
import React from "react";
import classNames from "classnames";
import "./Button.scss";

function Button({ children, size, color, outline, fullWidth, onClick }) {
  return (
    <button
      className={classNames("Button", size, color, { outline, fullWidth })}
      onClick={onClick}
    >
      {children}
    </button>
  );
}

Button.defaultProps = {
  size: "medium",
  color: "blue",
};

export default Button;
```

- 컴포넌트에 onMouseMove 을 설정

```javascript
// components/Button.js
import React from "react";
import classNames from "classnames";
import "./Button.scss";

function Button({
  children,
  size,
  color,
  outline,
  fullWidth,
  onClick,
  onMouseMove,
}) {
  return (
    <button
      className={classNames("Button", size, color, { outline, fullWidth })}
      onClick={onClick}
      onMouseMove={onMouseMove}
    >
      {children}
    </button>
  );
}

Button.defaultProps = {
  size: "medium",
  color: "blue",
};

export default Button;
```

- 위와 같은 방식으로 필요한 이벤트가 있을 때 마다 매번 이렇게 넣어주는건 번거롭기 때문에 **spread 와 rest** 사용
- 문법은 주로 배열과 객체, 함수의 파라미터, 인자를 다룰 때 사용하는데, 컴포넌트에서도 사용 할 수 있음

```javascript
// components/Button.js
import React from "react";
import classNames from "classnames";
import "./Button.scss";

function Button({ children, size, color, outline, fullWidth, ...rest }) {
  return (
    <button
      className={classNames("Button", size, color, { outline, fullWidth })}
      {...rest}
    >
      {children}
    </button>
  );
}

Button.defaultProps = {
  size: "medium",
  color: "blue",
};

export default Button;
```

- 이렇게 `...rest`를 사용해서 우리가 지정한 props 를 제외한 값들을 rest 라는 객체에 모아주고, `<button>` 태그에 `{...rest}` 를 해주면, rest 안에 있는 객체안에 있는 값들을 모두 `<button>` 태그에 설정

- App.js 에서 사용한 가장 첫번째 버튼에 onClick 을 설정

```javascript
// App.js
import React from "react";
import "./App.scss";
import Button from "./components/Button";

function App() {
  return (
    <div className="App">
      <div className="buttons">
        <Button size="large" onClick={() => console.log("클릭됐다!")}>
          BUTTON
        </Button>
        <Button>BUTTON</Button>
        <Button size="small">BUTTON</Button>
      </div>
      <div className="buttons">
        <Button size="large" color="gray">
          BUTTON
        </Button>
        <Button color="gray">BUTTON</Button>
        <Button size="small" color="gray">
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" color="pink">
          BUTTON
        </Button>
        <Button color="pink">BUTTON</Button>
        <Button size="small" color="pink">
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" color="blue" outline>
          BUTTON
        </Button>
        <Button color="gray" outline>
          BUTTON
        </Button>
        <Button size="small" color="pink" outline>
          BUTTON
        </Button>
      </div>
      <div className="buttons">
        <Button size="large" fullWidth>
          BUTTON
        </Button>
        <Button size="large" color="gray" fullWidth>
          BUTTON
        </Button>
        <Button size="large" color="pink" fullWidth>
          BUTTON
        </Button>
      </div>
    </div>
  );
}

export default App;
```

- 그래서 이렇게 컴포넌트가 어떤 props 를 받을 지 확실치는 않지만 그대로 다른 컴포넌트 또는 HTML 태그에 전달을 해주어야 하는 상황에는 이렇게 ...rest 문법을 활용하면 된다.
