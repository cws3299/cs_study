### 1Day 1Cs



##### 2021.04.20 (1차)



#### :pencil:JavaScript 면접 질문 정리



##### :computer: 이벤트 버블링, 이벤트 캡쳐링에 대해서 설명하세요

- 이벤트 버블링은 특정 화면요소에서 이벤트가 발생했을 때 더 상위 요소들로 전달되어 가는 과정을 의미함 (Vue.js의 경우 emit)
- 이벤트 캡쳐링은 이벤트 버블링과는 반대로 상위 요소에서 하위 요소로 이벤트를 전파하는 방식 (Vue.js의 경우 props)

##### :computer:event delegation에 대해 설명하세요

- 특정 요소 하나하나에 개별적으로 이벤트를 부여하는 것이 아니라, 하나의 부모에 이벤트를 등록하여 부모가 이벤트를 위임하는 방심을 이벤트 위임이라고 한다. 이 방법은 동적인 요소들에 대한 처리가 수월하며 이벤트 핸들러를 더 적게 등록하기 때문에 메모리도 절약된다.
- 예를들어 A라는 상위 태그 아래에 B-1과 B-2의 하위태그가 존재한다. B-1과 ,B-2에 이벤트를 걸어놓은 상황, 이 상황에서 자바스크립트로 B-1,B-2와 똑같은 태그를 만든다. 그러다 B-3에서는 이벤트가 제대로 작동하지 않음. 이를 해결하기 위해 A(상위요소)에 이벤트를 등록해서 A의 자식이 새로 생겨도 즉 새로운 자식도 이벤트가 잘 발생하도록 해주는 방법

##### :computer: this는 JavaScript에서 어떻게 작동하는지 설명해주세요.

- this란 인터프리터에 의해 현재 실행되는 자바스크립트의 환경에서 현재 실행되는 컨텍스트를 가리킨다.

- 기본적으로 this는 전역객체를 가리키지만 strict모드 혹은 즉시 실행함수의 경우 그렇지 않다.

  1. 객체의 메소드 내부에서 this를 호출할 경우, 해당 객체를 가리킨다. 그러나 주의해야 할 점이 존재한다.

     1. 메소드 내부의 함수 내부에서 호출할 경우, 이는 함수 내부에서 호출한 것이 되어 전역객체를 가리키게 된다.

     ```javascript
     var obj = {
         print: function() {
           console.log(this); // obj 객체
           
           var print2 = function() {
             console.log(this); // window 객체
           }
           print2();
        }
     }
     ```

     2. 이 경우, 메소드 내부에서 this를 정의해 주어야한다.

     ```javascript
     var obj = {
         print: function() {
           console.log(this); // obj 객체
           
           var _this = this;
           var print2 = function() {
             console.log(_this); // obj 객체
           }
           print2();
        }
     }
     ```

     3. 어떻게 호출하느냐 또한 중요하다.

     ```javascript
     var obj = {
         print: function() {
           console.log(this);
         }
     }
     var print = obj.print
     
     obj.print();      // obj 객체
     print();          // window 객체
     ```

     `obj.print()`의 경우 메소드 방식으로 호출하고 있으므로 객체 자신을 가리키게 되지만,

     `print()`의 경우 일반적인 함수 호출 방식으로 호출하고 있으므로 전역객체를 가리키게 된다.

- :white_check_mark: <b>결론적으로 객체내에서 객체를 통하느냐 아니냐의 문제인 것 같음. 기본적으로 this는 전역객체이기 때문</b>

    1. 생성자 함수 내부에서도  중요한 문제를 유발한다.

       ```javascript
       var foo1="foo1"
       function print() {
           this.foo2 = "foo2"
           console.log(this.foo1, this.foo2)
       }
       print();                      // foo1 foo2
       var newPrint = new print();   // undefined "foo2"
       ```

       

         1. new 연산자로 생성자 함수를 호출할 때, 생성자 함수 내부에서 호출된 this는 생성자 함수를 통해 새로 생성되어 반환되는 객체를 가리킨다.

            - 일반함수를 호출할 경우, this는 전경객체를 가리키므로 window객체의 foo1과 foo2가 출력된다.

            - 그러나 new로 선언할 경우, this는 전역 객체가 아닌 생성된 객체를 가리키므로 foo1은 undefined로 나오게 된다.

##### :computer: 화살표 함수와 this 바인딩

- es6에서 추가된 화살표 함수 내부에서 this를 사용할 경우, this에 바인딩할 객체가 정적으로 결정되기 때문에 call,apply,bind로 this를 변경할 수 없다.

- 화살표 함수 내부의 this는 언제나 상위 스코프의 객체를 가리키는데, 이를 문맥적 this라고 한다. 또한 사용에 주의 해야한다

- ```javascript
  window.x = 1;
  const normal = function () { return this.x; };
  const arrow = () => this.x;
  
  console.log(normal.call({ x: 10 })); // 10
  console.log(arrow.call({ x: 10 }));  // 1
  ```

- 화살표 함수로 메소드를 선언하는 것은 위험하다.

  - 객체의 메서드를 화살표 함수로 선언할 경우, this는 해당 객체가 아닌 전역 객체를 가리키므로 주의해야한다.

```javascript
var obj = {
    foo: "foo",
    print1: () => { console.log(this.foo) },
    print2 () { console.log(this.foo) }
}
obj.print1() // undefined
obj.print2() // foo
```

- 화살표 함수는 Prototype속성이 없다. 
- addEventListener의 콜백함수를 사용할때도 화살표 함수를 사용 할 경우 전역객체를 가리키게 되므로 주의해야한다.

- this는 자바스크립트 런타임 시에 바인딩이 이루어지는 실행 컨텍스트 중 하나로, 해당 함수가 실행되는 동안에 사용할 수 있으며 함수 호출 부분에서 this가 가리키는 것이 무엇인지를 확인할 수 있다. 때로는 복잡한 코드에서 암시적 바인딩에 의해 혼란스러운 경우가 많은데, 이러한 문제를 해결하기 위해 call이나 apply같은 내장 유틸리티를 사용하여 명시적으로 바인딩을 해 준다.

##### :computer: Prototype 기반 상속은 어떻게 하는지 설명해주세요. (추가)

- 기본적으로 자바스크립트 객체에는 Prototype이라는 내부 프로퍼티가 존재하며, 이는 다른 객체를 참조할 때 사용한다. 자바스크립트에서 상속을 진행할 떄는 프로토타입끼리 연결을 하는데, 부모 프로토타입을 create()나 setPrototypeOf() 메서드를 사용하여 자식 프로토 타입과 연결한다.

##### :computer: null과 undefined의 차이점은 무엇인가요?

- null은 의도적으로 값이 없다는 것을 표현함
- undefined는 의도적이지 않은 상태에서 값이 없음을 보여줌 (주로 에러 상황)

##### :computer: 익명함수는 주로 어떤 상황에서 사용하나요?

- 익명함수는 즉시 실행이 필요한 상황에서 사용한다.



###### :heavy_check_mark:1차 엔딩





---------------------------- 이후는 추가할 예정





참고 래퍼런스

(https://devowen.com/276?category=778540)

(https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)

(https://velog.io/@danmin20/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-this-%EB%B0%94%EC%9D%B8%EB%94%A9%EC%9D%B4%EB%9E%80)

