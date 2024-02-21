# Dart

### DATA TYPES



- 기본적인 데이터 타입

  ```dart
  void main() {
    String name = "nico";
    bool alive = true;
    int age = 12;
    // double은 소숫점 붙일 수 있음
    double money = 59.99;
  }
  ```

  - Dart의 거의 모든 자료형은 object로 이루어져 있음
  - function도 object임



- num 자료형
  - num 자료형을 사용하면 그 숫자는 integer일 수도 있고 double일 수도 있음
  - num은 int와 double의 부모 class임



- List

  - 기본 형태

    ```dart
    void main() {
    //   var numbers = [1, 2, 3, 4];
      List<int> numbers = [1, 2, 3, 4];
      // 이 상태에서는 리스트에 숫자만 추가 가능
      numbers.add(1);
    }
    
    ```

  

  - collection if

    if로 존재할 수도 안할 수도 있는 요소를 가지고 만들 수 있다.

    ```dart
    void main() {
      var giveMeFive = true;
      var numbers = [1, 2, 3, 4, if (giveMeFive) 5,];
    }
    
    // giveMeFive가 true라면 numbers는 [1, 2, 3, 4, 5]가 된다.
    ```

    

  - collection for

     ```dart
     void main() {
       var oldFriends = ['jun', 'nico'];
       var newFriends = [
         'lewis',
         'ralph',
         'darren',
         for(var friend in oldFriends) "💖 $friend",
       ];
       print (newFriends);
     }
     ```

  

  - 이 기능들은 UI 인터페이스를 만들 때, 게임 체인저이다.

    특히 collection if는 메뉴나 navbar를 만들 때, user가 로그인 했는지 안했는지 나타내는 버튼을 추가할 수 있다.

    collection for는 코드를 간략하고 단순하게 만들어준다.



- String Interpolation

  -  달러 기호($) 뒤에 변수를 사용

  ```dart
  void main() {
    var name = 'jun';
    var greeting = 'Hi, My name is $name!';
    print(greeting);
  }
  ```

  

  - 계산을 실행할 때

  ```dart
   void main() {
    var name = 'jun';
    var age = 10;
    var greeting = "Hi, My name is $name and I'm ${age + 17}";
    print(greeting);
  }
  ```

  

- Maps

  - JS나 TS의 object, python의 dict와 같은 역할
  - key와 value로 이루어진 자료구조 Map을 만들었을 때, key는 String이고, value는 Object(어떤 자료형이든 가능하다)일 때, Map의 자료구조는 Map<String, Object>가 된다.

  ```dart
  void main() {
    var player = {
      'name': 'jun',
      'xp' : 19.99,
      'superpower':false,
    };
    Map<List<int>, bool> player2 = {
      [1, 2, 3, 5]: true
    };
    List<Map<String, Object>> player3 = [
      {
        'name' : 'jun',
        'xp': 19.90,
      },
      {
        'name' : 'jun',
        'xp': 19.90,
      },
      {
        'name' : 'jun',
        'xp': 19.90,
      },
    ];
    print(player2.keys);
    print(player3[1]);
  }
  
  ```

  - 이 방법을 추천하지는 않음
  - key와 value로 이루어진 무언가를 정의할 때, 특히 API를 사용할 때는 class 를 사용하는 것이 좋다.

  

  

- Sets

  - Set\<int>

  - 기본 형태

    ```dart
    void main() {
      var numbers = {1, 2, 3, 4};
    }
    ```

  - set과 List의 차이는 Set에 속한 모든 아이템들은 유니크하다.

  - 요소가 항상 하나씩만 있어야 할 때(중복X) Set을 사용하면 된다.