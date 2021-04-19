# PureComponent



### 1. PureComponent와 Component의 차이

- `React.PureComponent`와  `React.Component`의 차이점은 `shouldComponentUpdate`를 어떻게 활용하는가에 중점을 둡니다.
- `shouldComponentUpdate` : 컴포넌트가 리렌더링 할지 말지를 결정하는 메서드 입니다.
  - 기본적으로 `true`를 반환 : 렌더링을 한다는 의미

```js
shouldComponentUpdate(prevProps, prevState) {
    return this.props.data !== prevProps.data;
}
```

- `componentDidUpdate` : `update` 가 이루어지고 `render`가 완료된 직후 실행되는 메소드이다.
  - 최초 **마운트** 될 때는 실행되지 않습니다.

```js
componentDidUpdate(prevProps, prevState, snapshot){}
```



### React.Component

```react
import React, { Component } from 'react';

class BlahBlah extends Component {
  state = {
    count: 0
  }
  counter () {
    this.setState({
      count: this.state.count // 주목! 값이 변경되지는 않으나, setState는 써줌!!
    })
  }
  componentDidUpdate () {
    console.log(this.state.count); // 컴포넌트가 업데이트되면 값을 출력시킨다.
  }
  render () {
    return (
      <div> 
        { this.state.count }
        <button onClick={this.counter.bind(this)}>counterButton</button>
      </div>
    )
  }
} 

export default BlahBlah;
```

-  `setState`가 실행되면 state, props의 변경 여부를 신경쓰지 않고 무조건적으로 컴포넌트를 업데이트시킵니다.



### React.PureComponent

```react
import React, { PureComponent } from 'react';

class BlahBlah extends PureComponent {
  state = {
    count: 0
  }
  counter () {
    this.setState({
      count: this.state.count // 주목! 값이 변경되지는 않으나, setState는 써줌!!
    })
  }
  componentDidUpdate () {
    console.log(this.state.count); // 컴포넌트가 업데이트되면 값을 출력시킨다.
  }
  render () {
    return (
      <div> 
        { this.state.count }
        <button onClick={this.counter.bind(this)}>counterButton</button>
      </div>
    )
  }
}

export default BlahBlah;
```

-  `setState`를 실행했음에도 불구하고 state, props가 변경되지 않았기 때문에 컴포넌트는 업데이트가 일어나지 않습니다.



### 2. PureComponent는 정확히 언제 어떻게 컴포넌트를 수행할 까?

- 얕은 비교( shallow-compare )
  - `PureComponent`는 `shouldComponentUpdate`에서 **얕은 비교**를 통해 업데이트 여부를 결정합니다.
  - 얕은 비교
    - 변수와 문자열에서는 값을 비교한다.
    - Object에서는 reference값을 비교한다.
- `PureComponent`는 현재 `state`, `props`와 바뀔 `state`, `props`를 비교하여 업데이트 여부를 결정합니다.
- 위의 코드에서 `setState`는 실행되었지만, count의 값이 바뀌지 않아 컴포넌트가 업데이트되지 않습니다.



### 3. PureComponent는 언제 활용하는 것이 좋을 까?

- 개발자가 `shouldComponentUpdate`를 따로 작업하지 않아도 될 때 사용해면 좋을 것 같습니다.
- 즉 불필요한 리렌더링을 방지하고 성능 향상 및 최적화를 손쉽게 하고 싶을 때 사용하기!