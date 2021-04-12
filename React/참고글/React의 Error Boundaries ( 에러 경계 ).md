# React의 Error Boundaries ( 에러 경계 )

- React에서 개발 도중에 에러가 발생하면 브라우저 화면에서 표시가 되지만 `production`환경일 경우 백지상태만 보여주게 됩니다.

- 하위 컴포넌트 트리의 어디에서든 자바스크립트 에러를 기록하며 깨진 컴포넌트 트리 대신 폴백 UI를 보여주는 React 컴포넌트

- 에러 경계를 추가함으로써 문제가 발생했을 때 더 나은 사용자 경험을 제공할 수 있습니다.

- 포착하지 않은 에러 종류

  - 이벤트 핸들러
  - 비동기적 코드 (예: setTimeout 혹은 requestAnimationFrame 콜백)

  - 서버 사이드 렌더링
  - 자식에서가 아닌 에러 경계 자체에서 발생하는 에러

- `getDerivedStateFromError` & `componentDidCatch`
  - `getDerivedStateFromError` :  state에 저장해 화면에 나타내는 용도로 쓰입니다.
  - `componentDidCatch` : 에러 정보를 서버로 전송하는 용도로 사용됩니다.
- 에러 경계는 자바스크립트의 `catch {}` 구문과 유사하게 동작하지만 컴포넌트에 적용됩니다. 오직 클래스 컴포넌트만이 에러 경계가 될 수 있습니다.

```react
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // 다음 렌더링에서 폴백 UI가 보이도록 상태를 업데이트 합니다.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // 에러 리포팅 서비스에 에러를 기록할 수도 있습니다.
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 폴백 UI를 커스텀하여 렌더링할 수 있습니다.
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

- 배치하기
  - 최상위 경로의 컴포넌트를 감싸거나 에러 경계의 각 위젯을 에러 경계로 감싸서 애플리케이션의 나머지 부분이 충돌하지 않도록 보호할 수도 있습니다.

```react
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```



### 포착하지 않은 에러는 어떻게 동작할까?

- **React 16부터는 에러 경계에서 포착되지 않은 에러로 인해 전체 React 컴포넌트를 언마운트 합니다.**



### 이벤트처리 메서드에서 예외처리

- 이벤트처리 메서드에서 예외가 발생되는 경우에는 try/catch문을 사용합니다.

```react
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { error: null };
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    try {
      // 에러를 던질 수 있는 무언가를 해야합니다.
    } catch (error) {
      this.setState({ error });
    }
  }

  render() {
    if (this.state.error) {
      return <h1>Caught an error.</h1>
    }
    return <button onClick={this.handleClick}>Click Me</button>
  }
}
```

