# Dart

### Variables

- class나 type은 main 밖에 있지만 실제로 뭔가를 하는 코드는 반드시 main 내부에 있음

  ```dart
  void main(){
      print('hello world!');
  }
  ```

  > 세미 콜론(;) 꼭 필요함!!!



- 변수 설정

  - Var를 사용해도 되고, 타입을 명시적으로 적어줘도 된다.
  - 하지만, 변수를 재설정 할 때는 그 타입을 맞춰줘야 한다.

  ```dart
  void main() {
    var name = 'JH';
    // String name = 'JH
  
    // 변수를 재할당 할 때는 기존의 타입과 일치시켜야 한다.
    name = 'HH';
  }
  ```

  - 함수나 메소드 내부에 지역 변수를 선언할 때에는 var를 사용(관습)
  - class에서 변수나 property를 선얼할 때는 타입을 지정해줌



- dynamic

  ```dart
  void main(){
      var name;
  }
  ```

  이 경우,  변수의 타입이 dynamic이기 때문에 어떤 타입이든 할당이 가능하다.

  단, 정말 필요한 경우에만 사용할 것.

  ```dart
  void main() {
      dynamic name;
      if (name is String) {
          name.
              // 여기서는 스트링임
      }
  }



- Null safety

  - Dart에서는 기본적으로  null값을 허용하지 않는다.
  - 그럼에도 null값을 표현하고 싶을 때는 ?를 사용한다.

  ```dart
  bool isEmpty(String string) => string.length == 0;
  
  main() {
      isEmpty(null);
  }
  // string을 보내야 하는 곳에 null을 보냈기 때문에 에러 발생
  
  void main() {
    String? name = 'nico';
    name = null;
  }
  // String뒤에 ? 를 쓰면 null 값 가능
  ```

  

- fianl

  한 번 정의된 변수를 변경되지 않게 할 때 사용 (javaScript의 const와 같음)

  ```dart
  void main() {
    final name = 'nico';
      // final String name = 'nico'; 도 가능
  }
  ```



- late

  - var 나 final 앞에 붙음
  - 초기 데이터 없이 변수를 선언할 수 있게 해줌

  ```dart
  void main() {
    late final String name;
    // do something
    // 예를 들어 api를 받는다던가.
    name = 'nico';
    name = 'nico2'; //이건 안됨
    // final로 선언했기 때문에 다시 선언은 불가.
  }
  ```

  - data fetching 할 때 유용하다.



- const

  - JavaScript나 TypeScript의 const와는 다르다.
  - compile-time constant를 만들어 준다.
  - 예를 들어 API를 const에 넣어주면, 절대 바뀌지 않고, 컴파일 될 때 그 값을 알고 있는 것이다.
  - const는 컴파일 할 때 알고 있는 값에 사용하는 것
  - 즉, 앱스토어에 앱을 올리기 전에 알고 있는 값
  - 어떤 값인지 모르고, 그 값이 API로 부터 온다거나  사용자가 화면에서 입력해야 하는 값이라면 그것은 final이나 var가 되어야 한다.

  - 앱스토어에 올리기 전에 이미 알고 있는 값을 다룰 때 const 사용(max_allowed_price, 앱에서 사용할 상수들)