# JavaScript Start
- [JavaScript Start](#javascript-start)
  - [Spec](#spec)
  - [Data type](#data-type)
    - [typeof 연산자의 반환 값](#typeof-%ec%97%b0%ec%82%b0%ec%9e%90%ec%9d%98-%eb%b0%98%ed%99%98-%ea%b0%92)
    - [산술 연산](#%ec%82%b0%ec%88%a0-%ec%97%b0%ec%82%b0)
    - [==, === 비교](#%eb%b9%84%ea%b5%90)
  - [function](#function)
  - [object(instance)](#objectinstance)
    - [생성자](#%ec%83%9d%ec%84%b1%ec%9e%90)
    - [this](#this)
  - [array](#array)
  - [Reference](#reference)

## Spec

1. 인터프리터 언어
   - **JIT 컴파일러가** 내장되어 실행속도가 빨라짐
  
2. 동적 프로토타입 기반 객체 지향 언어
   - JAVA, C++ 에서, 프로그램 실행 중 클래스를 인스턴스화 하여 나온 객체들은 메서드 혹은 멤버변수를 수정할 수 없지만, **JS에서는 프로토 타입 상속으로 인해 변경 할 수 있다.**

3. 동적 타입 언어
   - **특정 변수 타입이 없다.**
   - const, let, var

4. 일급 객체 함수
   - **함수(fucntion)도 객체로 지정(Funciton)**
   - 즉, 값으로 평가한다.

5. 클로저 지원
   - 내장 함수(Nested Function)를 지원 하여 **하나의 인스턴스화, 은닉이 가능**

## Data type

- 원시타입 **(Primitive Type)**: 자바스크립트에서 객체가 **(Reference Type)** 아닌 것들이며 값 그 자체로 저장된다.
  - String, Number, Boolean 같은 경우 객체가 존재 하지만, 원시타입의 리터럴로 정의하여 프로퍼티를 사용할경우 **Wrapper Object**로 자동 변환 되어 프로퍼티를 리턴한다. (Auto Boxing) 
    ```js
      1.toString(); // "1" 
      new Number(1).toString(); // Wrapper Object 리턴후 property 값 리턴 내부에서 이루어진 평가이기 때문에 GC로 없어짐
    ```
    - 연산 이후, GC로 해당 Wrapper Object 삭제 (할당이 되어이 있지 않기 때문에)
    ```js
      "string".newProperty = 1; // no error 
      console.log("string".newProperty); // undefined
    ```
  - 하지만, `new 연산자로` 객체를 만들경우 Reference Type Objcet로 리턴되어 나와 **Primitive Type을 잃게 된다.**
    - (원시 값을 갖고 싶으면, new String("값").valueOf() 사용 해야한다.)

1. Boolean

2. Null
   - 산술 연산자에서는 '0'으로 평가

3. Undefined
   - 산술 연산자에서는 'NaN'으로 평가

4. Number (IEEE754로 규정된 64-bit 부동소수점,  자바스크립트에는 정수 타입은 존재하지 않다.)
   - 부호(1 bit), 지수(11 bit), 가수(52 bit)

5. String
  - 산술 연산자에서 '+' 연산 시 String 제외한 객체들(Primitve 포함)을 `'toString()'`, 혹은 `'valueOf()'(우선순위 높음)` 으로 반환된 값이랑 연산된다.
  - 그외 '*,/,%...' 연산자는 String 타입을 Number(String 타입)으로 연산을 하게된다.

6. Symbol (ECMAScript 6에 추가됨)

### typeof 연산자의 반환 값

| data   | ex     | return | 
|:-------|:-------|:-------|
|숫자, NaN|12,NaN, Number(12)|"number"|
|문자열|"값", String("값")|"string"|
|논리값|true, false|"boolean"|
|정의되지 않은 값|undefined|"undefined"|
|null 값|null|"object"|
|심벌|Symbol("값")|"symbol"|
|함수 외의 객체|[1,2,3], new String("값"), new Number(12)|"object"|
|함수|function(){}|"function"|

### 산술 연산

- String 타입의 '+' 연산 방식을 제외하고 `대부분의 Data Type의` **'valueOf()'(우선순위 높음) 혹은 toString()으로 리턴된 값으로 연산을 한다.**
```js
  null.valueOf(); // Error
  null + 1; // 1
  undefined + 1 // NaN
  NaN + "stirng" // "NaNstring"
```

### ==, === 비교

- '==' 비교는 타입이 일치 하지 않을 때, 강제 타입으로 변환시켜 비교하게 된다.
  ```js
    77 == "77" // => "77" => Number("77")로 변환 후 검사
    false == 0 // => false => Number(false)로 변환 후 검사
    
    null == null // =>  서로 같으며 자기 자신과도 같습니다. => true 
    undefined == undefined // =>  서로 같으며 자기 자신과도 같습니다. => true
    null == undefined // =>  서로 같으며 자기 자신과도 같습니다. => true
    null == 0 // => false
    undefined == 0 // => false

    NaN == null // 어떠한 값과도 동일하지 않다 false
    NaN == undefined // 어떠한 값과도 동일하지 않다 false
    NaN == NaN // 어떠한 값과도 동일하지 않다 false
  ```
- '===' 비교는 타입이일치 하진 않으면, return false, 타입이 일치할 때 값을 비교를 한다. (Best Code)

## function

- parameter(인자)
  - 유효범위는 지역함수 이며, 프로그램 실행하면서 필요한 변수 값

- argument(인수)
  - 함수에 인자값이 있지만, 인수를 안 보내도 함수는 실행이 된다.
  - 해당하는 인자값을 모두 `undefined`로 정의 됨(lexcal Scope)

- 지역 변수
  - 함수내의 변수를 선언 할 수 있으며 안에서 식별자(var, const, let)로 선언한 변수들은 모두 지역 변수로 들어간다.
  - 함수 안에 함수를 실행할 수 있다.(Nested Function)
  
- return
  - return이 없을 경우 undefined를 리턴 한다.

- **JS에서는 함수가 객체이다.** 
  - 함수 선언문으로 함수를 선언하면 내부적으로는 그 함수 이름을 변수 이름으로 한 변수와 **함수 객체가 만들어지고,**  함수 객체의 참조가 저장 된다.

## object(instance)

- **연관배열(associative array)** 또는 **맵(map),** **딕셔너리(Dictionary)** 라는 데이터 타입이 객체에 해당한다.
- key, value 쌍으로 구성되어 있으며, key를 `해당하는(this)` 객체의 프로퍼티로 명한다.
- 생성된 객체는 메모리(RAM-Heap)에 정의되며 해당하는 **주소값(Reference Type)을 저장하는 구조이다.**

```javascript
    const 객체 리터럴 = {'propertyName': 10, 'propertyName(Method)': function(){ console.log(this.propertyName)}};

    var 객체 리터럴 = {};
    객체 리터럴['propertyName'] = 10;
    객체 리터럴['propertyName(Method)'] = function(){ console.log(this.propertyName) };
    delete 객체 리터럴['propertyName']; // property delete -> confiualable일 때만!!
    
    var 객체 생성자 = new 객체를 만들려는 함수명();
    객체 생성자['propertyName'] = 10;
    객체 생성자['propertyName(Method)'] = function(){ console.log(this.propertyName) };
```

### 생성자

- 앞에 예제 처럼 `new 연산자로` 객체를 생성시킨 함수를 생성자라고 한다.
- 인스턴스는 **실체라는** 뜻이 있다. 
- 객체 지향 언어에서(ex. JAVA, C++) 인스턴스는 `클래스(설계도)로` 생성한 실체를 뜻한다.
  - 하지만, JS에서는 **클래스가 없다.**
  - 또한, 함수 자체도 **함수객체로** 만든 해당 **함수 인스턴스 이다.** 
  - 그래서, 생성자로 생성한 객체는 엄밀히 말해 인스턴스가 아니다.
    - 하지만, 생성자가 클래스처럼 객체를 생성하는 역할을 담당하고 있어 **생성자로 생성한 객체도 인스턴스라고 부르는것이 관례이다.**

### this

- 함수가 속해있는 객체를 가리키는 변수이다.
- 함수를 어떻게 생성하냐에 따라서 달라질 수 있지만, 맥락에서는 function을 가지고있는 **객체 생성자(value Name), 객체 리터럴(value Name)** 를 말한다.

## array

- 앞서 말한(Data type)에서도 원시 타입 이외는 모두 객체 참조이므로 array도 Array객체로 만들어진 **객체** 이다.
- 즉, 배열 넘버링(index)이 key값으로 저장된것이다.
  - 이 말은 즉, 여러 프로그래밍 언어에서 배열 요소는 **메모리에 연속된 공간에 저장 되지만, JS는 객체 그렇지 않다.**
  - ES6에서 `TypedArray에서는` 연속된 공간에 저장된다.
- 객체이기 때문에 delete가 가능한 프로퍼티가 있으면 **희소 배열로** 0 부터 시작되지 않은 배열이 만들어 질 수 있다.

## Reference

- [자바스크립트 개발자라면 알아야 할 33가지 개념 #2 자바스크립트의 원시 타입(Primitive Type) (번역)](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-2-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%9B%90%EC%8B%9C-%ED%83%80%EC%9E%85Primitive-Type-%EB%B2%88%EC%97%AD)
- [자바스크립트 개발자라면 알아야 할 33가지 개념 #3 값(value) vs 참조(reference) (번역)](https://velog.io/@jakeseo_me/2019-04-01-1904-%EC%9E%91%EC%84%B1%EB%90%A8-2bjty7tuuf)
- [자바스크립트 개발자라면 알아야 할 33가지 개념 #4 암묵적 타입 변환(implicit coercion) (번역)](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-4-%EC%95%94%EB%AC%B5%EC%A0%81-%ED%83%80%EC%9E%85-%EB%B3%80%ED%99%98-%EB%B2%88%EC%97%AD)
- [자바스크립트 개발자라면 알아야 할 33가지 개념 #5 == vs === 3분만에 배우기 (번역)](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-5-vs-3%EB%B6%84%EB%A7%8C%EC%97%90-%EB%B0%B0%EC%9A%B0%EA%B8%B0-%EB%B2%88%EC%97%AD#-%ED%91%9C%EC%8B%9C-2%EA%B0%9C%EC%9D%98-%EB%8F%99%EB%93%B1-%EB%B9%84%EA%B5%90%EC%97%B0%EC%82%B0%EC%9E%90)