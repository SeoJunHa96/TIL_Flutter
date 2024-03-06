# Dart

### Classes

- 기본 형태

  ```dart
  class Player {
    String name = 'jun';
    int xp = 1500;
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  void main() {
    var player = Player();
    print(player.name);
    player.sayHello();
    player.name = 'lalala';
    print(player.name);
    player.sayHello();
  }
  
  /*
  jun
  Hi my name is jun
  lalala
  Hi my name is lalala
  */
  ```

  

- Constructor

  ```dart
  class Player {
    // late는 선언은 여기서 했지만 값은 나중에 받아온다는 의미
    late final String name;
    late int xp;
    
    Player(String name, int xp) {
      this.name = name;
      this.xp = xp;
    }
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  void main() {
    var player = Player("nico", 1500);
    player.sayHello();
    var player2 = Player("jun", 9000);
    player2.sayHello();
  }
  ```

  ```dart
  // 좀 더 간단하게 만들기
  class Player {
    final String name;
    int xp;
    
    Player(this.name, this.xp);
    
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  void main() {
    // 단 여기선 자리가 중요함
    var player = Player("nico", 1500);
    player.sayHello();
    var player2 = Player("jun", 9000);
    player2.sayHello();
  }
  ```

  

- Named Constructor Parameters

  클래스가 아주 클 때 통제하기가 힘들어진다.

  따라서 좀 더 쉽게 클래스를 다루기 위한 방법

  ```dart
  class Player {
    final String name;
    int xp;
    String team;
    int age;
    
    Player({required this.name, required this.xp, required this.team, required this.age});
    
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  void main() {
    // 단 여기선 자리가 중요함
    var player = Player(
      name: "jun",
      xp : 1200,
      team : 'blue',
      age : 28,
    );
    player.sayHello();
  
  }
  ```

  

  - 생성자 함수 정리

    타입을 선언하고, 그것을 required로 만들고, key-value쌍으로 작성

  

- Named Constructors

  ```dart
  class Player {
    final String name;
    int xp, age;
    String team;
    
    Player({required this.name, required this.xp, required this.team, required this.age});
    
    Player.createBluePlayer({
      required String name, 
      required int age
        // 콜론을 넣음으로써 여기서 Player객체를 초기화하겠다고 선언
      }) :  this.age = age,
            this.name = name,
            this.team = 'blue',
            this.xp = 0;
    
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  void main() {
    var player = Player.createBluePlayer(
      name: "jun",
      age: 21,
    );
  }
  ```

   

- Cascade Notation

  ```dart
  class Player {
    String name;
    int xp;
    String team;
    
    Player({required this.name, required this.xp, required this.team});
    
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  void main() {
    var nico = Player(name: 'nico', xp:1200, team: 'red')
    ..name = 'las'
    ..xp = 120000
    ..team = 'blue'
    ..sayHello();
  }
  ```

  콜론 확인 잘하기

  dot(..)은 nico를 가르킴(nico.name과 같은 역할을 한다.)



- Enums

  개발자들이 오타와 같은 실수 하지 않게 도와주는 역할  

  ```dart
  enum Team { red, blue }
  // Team red, Team blue가 됨
  // 더이상 Team은 String이 아니게 되고, enum인 Team이 된다.
  // 두 가지 옵션 중 하나가 된다. 
  
  class Player {
    String name;
    int xp;
    Team team;//더 이상 String이 아니라 Team이 된다.
    
    Player({required this.name, required this.xp, required this.team});
    
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  void main() {
    var nico = Player(name: 'nico', xp:1200, team: Team.red)
    ..name = 'las'
    ..xp = 120000
    ..team = Team.blue
    ..sayHello();
  }
  ```

  

- Abstract Classes

  추상화 클래스

  다른 클래스들이 직접 구현 해야하는 메소드들을 모아 놓은 일종의 청사진

  수많은 청사진에 메소드의 이름과 반환 타입만 정해서 정의할 수 있다.
  
  추상화 클래스는 이를 상속받는 모든 클래스가 가지고 있어야 하는 메소드를 정의하고 있다.
  
  ```dart
  abstract class Human {
    void walk();
  }
  
  enum Team { red, blue }
  
  class Player extends Human {
    String name;
    int xp;
    Team team;
    
    Player({required this.name, required this.xp, required this.team});
    
    void walk() {
      print('im walking'); 
    }
    
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  class Coach extends Human {
    void walk() {
      print('the coach is walking');
    }
  }
  
  void main() {
    var nico = Player(name: 'nico', xp: 1200, team: Team.red)
      ..name = 'las'
      ..xp = 120000
      ..team = Team.blue;
      
    nico.sayHello();
  }
  
  ```



- Inheritance

  ```dart
  class Human {
    final String name;
    Human({required this.name});
    void sayHello() {
      print("Hi my name is $name");
    }
  }
  
  enum Team { blue, red }
  
  class Player extends Human {
    final Team team;
    
    // named argument를 사용하는 Player의 생성자 함수 만들기
    Player({
      required this.team,
      required String name,
      // super 키워드를 통해 확장을 한 부모 클래스와 상호작용할 수 있게 한다.
    }) : super(name: name) ;
    
    @override
    void sayHello(){
      super.sayHello();
      print('and I play for $team');
    }
  }
  
  void main() {
    var player = Player(team : Team.red, name:'jun');
    player.sayHello();
  }
  ```

  

- Mixins

  - 생성자가 없는 클래스를 의미한다.
  - 클래스에 프로퍼티들을 추가할 때 사용
  - 다른 클래스의 프로퍼티와 메소드를 긁어오는 것.

  ```dart
  //Mixins
  mixin class Strong {
    final double strengthLevel = 1500.99;
  }
  
  mixin class QuickRunner {
    void runQuick() {
      print("ruuuuuun!");
    }
  }
  
  mixin class Tall {
    final double height = 1.99;
  }
  
  enum Team { blue, red }
  
  // with를 사용해서 Quick과 Strong클래스에 있는 프로퍼티와 메소드를 Player에 담아줌
  class Player with Strong, QuickRunner, Tall {
    final Team team;
  
    Player({
      required this.team,
    });
  }
  
  void main() {
    var player = Player(
      team: Team.red,
    );
    player.runQuick();
  }
  
  ```

  



---

- Named Constructor는 flutter에서 많이 사용하기 때문에 제대로 이해하고 넘어가기!!

```dart
class Player {
  final String name;
  int xp;
  String team;
  
  Player.fromJson(Map<String, dynamic> playerJson)
    : name = playerJson['name'],
      xp = playerJson['xp'],
      team = playerJson['team'];
  
  void sayHello() {
    print("Hi my name is $name");
  }
}

void main() {
  var apiData = [
    {
      "name" : "nico",
      "team" : "red",
      "xp" : 0,
    },
    {
      "name" : "jun",
      "team" : "red",
      "xp" : 0,
    },
    {
      "name" : "dal",
      "team" : "blue",
      "xp" : 0,
    },
    {
      "name" : "ryn",
      "team" : "blue",
      "xp" : 0,
    },
  ];
  
  apiData.forEach((playerJson) {
    var player = Player.fromJson(playerJson);
    player.sayHello();
  });
}
```





- super 키워드 관련해서 상속 부분은 추가 공부 필요
