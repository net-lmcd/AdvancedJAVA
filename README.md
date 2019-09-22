# Some tips for Mobile developers

  
 ## WeakReference
  
 앱 개발 마무리 단계에 들어가면서 직면하는 문제는 MemoryLeak이다. 이것의 해결책으로 WeakReference(약한 참조)가 제시되곤 한다.
 
 1. WeakReference vs StrongReference
 
  객체 생성 방법중에, new()메소드를 사용해서 생성하는 StrongReference와 WeakReference로 생성하는 약한 참조는 가비지 컬렉터에의해 메모리상에서 회수되는 기준
  이 다르다. 가비지 컬렉터가 메모리를 정리할 대상인지 아닌지 판단하기위해서 reachability라는 기준을 가지고 있는데 가장 먼저 참조되어지는 root객체로부터 사슬이 계
  속 연결되어 있는가의 여부이다. 이 사슬이 끊어져있다면, unreachable로 보고, 가비지 컬렉터의 메모리 회수대상이 된다. 하지만, WeakReferecne를 사용하여 구현하
  였다면, 명확하게 가비지 컬렉터의 메모리 회수 대상이 되며, 참조값을 null값으로 해주어 메모리 누수가 일어나지 않도록 미연에 방지할 수 있다.
  
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

 ## MVVM pattern 

  MVVM은 Model-View-ViewModel의 약자이다. Model은 UI에 표시될 데이터 와 비즈니스 로직을 담당하고 View는 UI를 의미하며 ViewModel은 이벤트 처리
  나, Model과의 인터랙션 등을 담당한다. 아래는 MVVM의 각 component에 대한 정의이다.

 1. Model
  
  DataModel 이라고도 불린다.
  ViewModel 에서 데이터를 가져갈 수 있게 데이터를 준비하고 이벤트를 보낸다.

 2. View
  
  Model의 존재를 알지 못한다.
  UI의 업데이트를 위해 ViewModel과 바인딩이 하게된다. 즉 ViewModel의 상태가 변경되면 구독하고있는 view가 그 이벤트를 받아 UI를 갱신한다.
 
 3. ViewModel
 
  View 와 Model 사이의 글루코드.
  Model에서 제공받은 데이터를 UI에서 필요한 정보로 가공한뒤 View가 가져갈 수 있도록 데이터 변경에 대한 이벤트를 보내주는 역할.
  View의 존재를 알지 못한다.  
  하나의 ViewModel은 여러개의 View 를 가질 수 있다. (MVP에서는 Presenter와 View가 one to one을 유지해야 하는데, MVVM에서는 many to one이다)
  View에 영향을 끼칠 수 있는 Model의 상태도 관리한다.
  View 또는 액티비티 Context에 대한 레퍼런스를 가져서는 안된다. View는 ViewModel의 레퍼런스를 가지지만, ViewModel은 View에 대한 정보가 전혀 없어야 한다.
  
  - 정리
 
  View, Model, ViewModel은 서로의 존재를 알지 못해야 한다.
  ViewModel에서 View에게 갱신을 알리기 위해서는, Observable 또는 DataBinding을 사용해야 한다. 즉 ViewModel의 데이터가 수정되더라도, View에서는 수정할
  것이 없도록 둘을 분리해야 한다.
  Model에는 데이터와 관련한 소스만, View는 화면과 관련한 소스만 넣는다. View에서 만약 데이터에 의한 변경들이 있으면 ViewModel의 옵저버 등을 통해서 값이 
  변경되었음을 알고 변경할 수 있다.

  자세한 내용은 https://docs.microsoft.com/en-us/xamarin/xamarin-forms/enterprise-application-patterns/mvvm 참고한다.
 
