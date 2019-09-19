# Some tips for Mobile developers

  
 ## WeakReference
  
 앱 개발이 마무리 단계에 들어가면서 직면하는 문제는 MemoryLeak입니다
 이것에 대한 해결책으로 WeakReference(약한 참조)가 제시된다
 
 1. WeakReference vs StrongReference
 
  객체 생성 방법중에, new()메소드를 사용해서 생성하는 StrongReference와 WeakReference로 생성하는 약한 참조는 가비지 컬렉터에의해 메모리상에서 회수되는 기준
  이 다르다. 가비지 컬렉터가 메모리를 정리할 대상인지 판단하기위해서 reachability라는 기준을 가지고 있는데 가장 먼저 참조되어지는 root객체로부터 사슬이 계속 연결
  되어 있는가의 여부이다. 이 사슬이 끊어져있다면, unreachable로 보고, 가비지 컬렉터의 메모리 회수대상이 된다. 하지만, WeakReferecne를 사용하여 구현하였다면, 
  가비지 컬렉터의 메모리 회수 대상이 되며, 참조값을 null값으로 해주어 메모리 누수가 일어나지 않도록 할 수 있다.
  
 2. WeakReference 구현
  ```bash
  WeakReference<User> mWeak = 
          new WeakReference<>(new User("940602","CheolHunChoi"))
  User user = mWeak.get()
  user = null;
  ```
  위 코드는 단순히 new()메소드로 생성해서, wrapping한 객체이다. 이렇게 생성한 객체는 mWeak객체의 get()메소드로 얻어온다.user객체에 null을 대입하면 User객체
  에 대한 접근은 WeakReference의 mWeak로만 가능하게 된다. WeakReference에 의해서만접근할 수 있는 상태가 될때, reachability가 약하다고 할 수 있다. 이 
  상태가 되면 가비지컬렉터가 동작하면 WeakReference객체에 대한 참조값을 null로 해주고, 메모리상에서 정리대상이 될수 있다는 것이다. 
