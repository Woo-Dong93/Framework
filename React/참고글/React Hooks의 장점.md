# React Hooks의 장점



### 1. Hook이란

- 함수형 컴포넌트에서 상태와 생명주기에 엮인 부수효과를 관리하는 새로운 방법입니다.



### 2. 장점

- 라이플 사이클 API보다 단순하다

  - 수많은 라이플 사이클 API를 몇가지 훅으로 간편하게 구현할 수 있습니다.

- Wrapper Hell을 방지할 수 있다.

  - hook 이전에는 HOC(Higher Order Component)를 이용하여 코드를 반복해서 작성을 해야 하는 문제를 해결하려고 했습니다.

  - "HOC"이란 화면에서 재사용 가능한 로직만을 분리해서 component로 만들고, 재사용 불가능한 UI와 같은 다른 부분은 parameter로 받아서 처리하도록 하는 패턴입니다. 즉, 리액트 컴포넌트를 인자로 받아서 새로운 리액트 컴포넌트를 리턴하는 함수라고 생각하시면 됩니다.

  - 하지만 HOC을 사용했을 때, wrapper hell이라는 또 다른 문제가 발생할 수 있는데요!

  - "wrapper hell"이란 중첩되는 component가 많아져 depth가 깊어지는 것을 의미합니다.

  - 함수형은 로직을 분리할 수 있고 wrapper hell을 줄일 수 있게 됩니다.

    

### 3. 단점

- 미지원 기능

  - 오류핸들링하는 `componentDidCatch` 등 미지원 기능이 존재.

  

### 참고

- https://ddwroom.tistory.com/75