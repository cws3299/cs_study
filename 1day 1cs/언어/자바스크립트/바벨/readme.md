### 1Day 1CS



##### 2021.04.14 바벨이란 무엇인가?



### 바벨(1) / 바벨이란 과연 무엇인지 및 기본 사항 및 기초적 활용

### 바벨의 활용(2) / 추후 작성





#### :tokyo_tower:바벨이란 무엇인가?



- <b>바벨</b>이란 입력과 출력이 모두 자바스크립트 코드인 컴파일러(?)이다.
- 엄밀하게 말하면 <b>컴파일러</b>라기 보다는 <b>트랜스파일러</b>가 맞는 표현이다.
- 과거에는 ES6 코드를 ES5 코드로 변환해 주었지만, 현재는 리액트의 JSX문법 , 타입스크립트 등 다양한 분야의 역할도 수행함
- 요약 : 
  - <b>개발자들이 실행환경에 구애받지 않고 최신 문법의 자바스크립트로 코딩할 수 있도록 도와주는 도구</b>



#### :tokyo_tower: 바벨이 필요한 이유

- 브라우저 호환성 표에 따르면 ES2015 문법은 대부분의 브라우저가 평균 98%이상을 지원하며 최근 버전의 크롬 브라우저는 EX2016+ 문법도 어느정도 지원해줌
- 그러나 익스플로러와 같이 최신문법을 지원하지 않으며 업데이트가 중단된 브라우저들이 많은 상황 -> 이러한 브라우저를 쓰는 유저들에게는 과거의 ES문법이 필요함
- 이러한 상황에서 바벨은 현재 최신의 ES문법을 과거 문법으로 동일한 결과가 나오게 변환해주는 역할을 수행한다.



#### :tokyo_tower: 바벨은 컴파일(트랜스 파일)만 담당한다

- 다른 많은 일들은 주로 webpack이 담당한다.

#### :tokyo_tower: 바벨 코어자체는 최신 ECMA-Script문법을 해석할 줄 모른다.

- 컴파일의 작업은 @babel/core가 전담하는 것이아니라 @babel/core는 자바스크립트 코드를 분석하고 바벨 설정 파일(.babelrc.json)에 따라 컴파일 한다.
- 코드를 어떻게 변환할지는 프리셋이나 플러그인이 알고 있다.

#### :tokyo_tower: 바벨은 설치할게 너무 많은가?

```javascript
yarn add -D @babel/core @babel/cli \
              @babel/register @babel/preset-env \
              @babel/preset-foo @babel/plugin-bar
              @babel/why-should-i-install-this
```

- babel을 설치할때 다음과 같이 굉장히 많은 내용을 설치한다. 근데 대체 저 내용들은 무엇인가?
  - @babel/core 코어 패키지는 <b>요리사</b>이며 재료와 레시피가 있다면 요리를 만들어 낼 수 있다.
  - @babel/cli CLI패키지는 <b>조리도구</b>이다. 간단한건 손으로도 할 수 있지만 복잡한 요리를 하려면 조리 도구가 필요하다
  - 플러그인은 <b>레시피</b>로서 요리하는 방법이 정리되어있다.
  - 프리셋은 <b>레시피 북</b>으로서 요리를 만들 때 참고할 수 있도록 여러 레시피를 모아둔 책이다.



#### @babel/core 설치

`yarn add -D @babel/core`

- @babel/core는 로컬 설정 파일을 바탕으로 코드를 변환하는 api만 제공함



#### @babel/cli 설치

`yarn add -D @babel/cli`

- api 방식 말고, 터미널에서 babel 명령어를 입력하여 사용하기 위해서는 @babel/cli 패키지를 설치해야한다.

<b>샘플코드</b> 

```javascript
// sample.js
const arr = [1, 2, 3, 4, 5];

arr.map(value => value ** 2).forEach(value => console.log(value));
```

터미널에 명령어를 입력하여 컴파일을 시도

`yarn babel smaple.js`

변경되지 않은 것을 알 수 있음



#### @babel/preset-env 설치

- 위에서 플러그인과 프리셋을 설명할 때 레시피와 레시피 북으로 비유함
- 새로운 문법이나 기능을 위해 플러그인을 전부 나열할 수도 있지만 편의를 위해 프리셋이 제공됨
- 현재는 @babel/preset-env로 설정됨

`yarn add -D @babel/preset-env` = 현재 가장 많이 사용되는 프리셋

`yarn babel sample.js --presets=@babel/preset-env`로 실행가능

결과적으로 실행결과는 다음과 같이 출력된다.

```javascript
'use strict';

var arr = [1, 2, 3, 4, 5];
arr
  .map(function(value) {
    return value * 2;
  })
  .forEach(function(value) {
    return console.log(value);
  });
```



#### @바벨이 프리셋 패키지를 찾는 방법

`yarn babel sample.js --presets=@babel/preset-env`

`yarn babel sample.js --presets=@babel/env`

위와 아래는 같은 결과를 실행시킴

```txt
1. 기존에 사용한대로 접두사를 붙여서 사용하면 별도의 정규화를 거치지 않는다.
2. 별도의 접두사를 붙이지 않는 경우, babel-preset-을 붙여서 패키지를 찾는다. --presets=env는 babel-preset-env를 사용한다.
3. @babel 스코프를 사용하는 경우, 스코프와 이름 사이에 preset-을 붙여서 패키지를 찾는다. --presets=@babel/env는 @babel/preset-env를 사용한다.
4. 바벨이 아닌 다른 스코프, 예를 들어 @scope를 단독으로 사용하면, /babel-preset을 붙여서 패키지를 찾는다. --presets=@scope는 @scope/babel-preset을 사용한다.
5. 바벨이 아닌 다른 스코프, 예를 들어 @scope/foo를 사용하면, 스코프와 이름 사이에 babel-preset-을 붙여서 패키지를 찾는다. --presets=@scope/foo는 @scope/babel-preset-foo를 사용한다.
```

위 규칙은 플러그인에도 동일하게 적용된다.



#### .babelrc.json

- 매번 --presets 옵션을 주고 터미널에서 실행하는 것은 번거롭기 때문에 설정 파일으 추가함

- 해당 설정파일은 바벨 코어가 자동으로 읽어서 사용

  - `touch .barbelrc.json`

- `babelrc.json` 파일에 아래와 같이 입력 후 저장

  - 

    ```javascript
    {
      "presets": ["@babel/env"]
    }
    ```

- 이제 --presets 옵션을 빼고 바벨을 실행해도 @babel/preset-env 프리셋을 사용한 결과물이 나온다





#### .babelrc vs .babelrc.json 











참조 래퍼런스

(https://www.daleseo.com/js-babel/)

(http://tlog.tammolo.com/blog/babel-1-d6a5bf231ba044dab160698d01353eed/)

(https://imch.dev/posts/babel-practice/)