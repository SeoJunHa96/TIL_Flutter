# Dart

### Functions

- Defining a Function

  - void

    이 함수가 아무것도 반환(return)하지 않는다는 의미

    그저 Dart에 의해 호출될 main function일 뿐이다.

    ```dart
    String sayHello(String potato) {
      return "Hello $potato, nice to meet you!";
    }
    
    void main() {
      print(sayHello('jun'));
    }
    ```

    

  - fat arrow syntax

    곧바로 return 하는 것

    ```dart
    String sayHello(String potato) => "Hello $potato!";
    
    // 숫자 a, b 를 받아서 합을 반환하는 함수
    num plus(num a, num b) => a + b; 
    ```

    

  

- Named Parameters

  - 변수가 여러 개일 때의 기본 형태

    ```dart
    String sayHello(String name, int age, String country){
     return "Hello $name, you are $age, and you come from $country"; 
    }
    
    void main() {
      print(sayHello('jun', 27, 'Korea'));
    }
    ```

    좋은 방법은 아님(사용자가 순서나 인자를 기억해야 하기 때문에, 또한 인자가 뭘 뜻하는지 모르기 때문에)

  

  - 기본값 넣어서 만들기

    (Dart는 null safety를 지원하기 때문에)

    ```dart
    String sayHello({String name="ana", int age=22, String country="japan"}) {
      return "Hello $name, you are $age, and you come from $country";
    }
    
    void main() {
      print(sayHello(
        age : 28+1,
        name : 'jun',
        country : 'Korea',
        ));
    }
    ```

  

  - required modifier

    ```dart
    String sayHello({required String name, required int age, required String country }) {
      return "Hello $name, you are $age, and you come from $country";
    }
    
    void main() {
      print(sayHello(
        age: 28 + 1,
        name: 'jun',
        country: 'Korea',
      ));
    }
    ```





- Optional Positional Parameters

  ```dart
  String sayHello(String name, int age, [String? country = 'cuba']) => 'Hello $name, you are $age years old from $country';
  
  void main() {
    var results = sayHello('nico', 12);
    print(results);
  }
  ```

  

- QQ Operator

  - 내 이름을 대문자로 return 하는 함수를 만드는 것.

    ```dart
    String capitalizeName(String name) => name.toUpperCase();
    
    void main() {
      capitalizeName('jun');
      capitalizeName(null);
    }
    ```

    이 경우 capitalizeName에 null이 들어갈 수 있기 때문에 실행할 수 없다.


    여기서 한 가지 방법은 null이 아닐 때만 실행시켜주는 것.

    ```dart
    String capitalizeName(String? name){
      if (name != null){
        return name.toUpperCase();
      }
      return 'ANON';
    }
    
    void main() {
      capitalizeName('jun');
      capitalizeName(null);
    }
    ```

    

    혹은 fat arrow를 사용할 수도 있다.

    ```dart
    // name 값이 null과 같지 않다면, name.toUpperCase()를 리턴
    String capitalizeName(String? name) => name != null ? name.toUpperCase() : 'ANON';
    
    void main() {
      print(capitalizeName('jun'));
      print(capitalizeName(null));
    }
    ```

    

    더 짧게도 가능

    ```dart
    String capitalizeName(String? name) => name?.toUpperCase() ?? 'ANON';
    
    void main() {
      print(capitalizeName('jun'));
      print(capitalizeName(null));
    }
    ```

  

  - QQ

    Question, Question

    ```
    left ?? right
    만약 좌항이 null이면 우항을 return
    ```

    ```dart
    void main() {
        // null이 될 수 있는 name 변수
      String? name;
      // name이  null이라면 값을 할당해주는 것.
      name ??= 'nico';
      print(name); //nico
      name = null;
      name ??= 'another';
      print(name); //another
    }
    ```

     

- Typedef

  - 자료형이 헷갈릴 때 도움이 될 alias를 만드는 방법.
  - 자료형에 alias를 붙일 수 있게 해준다.

  ```dart
  typedef ListOfInts = List<int>;
  
  // 숫자로 된 리스트를 가지고 List를 반대로 뒤집어서 리턴
  ListOfInts reverseListOfNumbers(ListOfInts list) {
  // List<int> reverseListOfNumbers(List<int> list) {
    var reversed = list.reversed;
    return reversed.toList();
  }
  
  void main() {
    print(reverseListOfNumbers([1, 2, 3, 4]));
  }
  ```

  ```dart
  typedef UserInfo = Map<String, String>;
  
  // 숫자로 된 리스트를 가지고 List를 반대로 뒤집어서 리턴
  String sayHi(UserInfo userInfo) {
    return "Hi ${userInfo['name']}";
  }
    
    
  void main() {
    print(sayHi({"name" : 'nico'}));
  }
  ```

  

​	