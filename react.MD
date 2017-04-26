# React

## React.Component
https://facebook.github.io/react/docs/react-component.html

### mounting
constructor(props)
- mounted되기 전에  호출된다. 
- 제일 첫 문장에 super(props)를 호출해야 한다.그렇지 않으면 this.props는 undefined 로 남게된다.
- state를 초기화하기 적당한 장소 
- 상태를 초기화하고, methods를 바인드 하지 않는다면 구현할 필요가 없을지도 모른다. 
- props에 기반한 state초기화 패턴은 효과적으로 props을 가져와서 state를 initial props로 설정케한다.
- 계속해서 props의 변경을 state에 반영하고하고 싶다면 componentWillReceiveProps(nextProps)를 이용해야 한다. 

componentWillMount()
- 마운팅이 일어나기 직전에 일어나며, render()전에 호출된다.따라서 여기서 동기방식으로 state를 업데이트하면 re-rendering이 발생되지 않늗다.
- 부수효과를 일으키거나 이벤트 구독을 여기서 하지 말것

render()
- 반드시 구현해야함. 
- 아무것도 그려지지 않게 하려면 null이나 false를 반환한다. 그러면 ReactDOM.findDOMNode(this) 는 null을 반환하게 될것이다 .
- 구현은 pure해야 한다. 즉 state를 바꾸지 않고, 실행될때마다 같은 결과를 반환해야 한다. 브라우저와 직접 상호작용 할 수 없다. 필요하면 componentDidMount()에서
-  shouldComponentUpdate() 가 false를 반화하면 호출되지 않는다

componentDidMount(prevProps, prevState)
- 마운트 되자 마자 호출됨
- DOM nodes를 필요로 하는 초기화는 여기에서 구현 
- 원격지에서 데이터 로딩이 필요하다면 여기가  구현하기에 적합함.
- state변경은 re-rendering이 호출됨. 

### updating
componentWillReceiveProps(nextProps)
-  mounted컴포넌트가 새로운 프로퍼티를 받을때 발생
- 프로퍼티 변경에 state를 변경하고 싶으면, 여기서 this.props와 nextProps 비교 후 this.setState()를 통해 변경할 것 
- 실제 props자체 값의 변경이 없더라도 호출된다. (이는 부모 컴포넌트가 현재 컴포넌트가 rendering 되게할 때 발생할 수 있다.)
- 초기 마운팅시에는 호출되지 않음. this.setState변경으로는 호출되지는 않음.

shouldComponentUpdate(nextProps, nextState)
- 현재의 변경이 컴포넌트를 다시 렌더링 해야 하는지 결정할수 있다. (by returning true or false)
- props나 state의 변경을 받고 렌더링 전에 호출된다. 
- false를 반환해도 하위 컴포넌트가 자신의 state변경에 의해 re-rendering되는것을 막지는 못한다.
- false반환시 componentWillUpdate, render, componentDidUpdate는 호출되지 않는다. 향후에는 false를 반환해도 re-rendering은 일어날수 있다. 
- 특정 컴포넌트가 매우 느리면 React.PureComponent를 상속받아 컴포넌트를 만들수 있는데, 이는 얕은 state, props 비교를 수행하는 구현을 가지고 있다. 
- 초기 마운팅이나 forceUpdate시에는 호출되지 않음.


componentWillUpdate(nextProps, nextState)
- state 또는 props가 변경되고, 렌더링 전에 호출된다.
- 여기서는 this.setState를 호출할 수 없다. - props 변경에 state를 설정하고 싶다면 componentWillReceiveProps를 이용해라
- 초기 렌더링에는 호출되지 않는다. 

componentDidUpdate()
- state와 props가 변경된후 호출된다. 
- DOM에 대한 op는 여기서 수행해라 
- 원격지 데이터를 로딩하기에 적합한 곳이다. 
- 초기 렌더링에는 호출되지 않는다. 

### unmounting
componentwillUnmount

### properties
state
- 시간이 감에 따라 변경될 수 있는 데이터를 유지
- render시 사용되지 않는 데이터는 state의 일부가 아닐 수 있다, 예를 들면 timer의 IDs를 이 인스턴스에 직접 셋팅 할 수 있다.
- 직접 변경하지 말것. this.setState()이용

props
- 이 컴포넌트의 호출자에 의해 정의되는 props를 유지
- props는 내용은 읽기 전용
 
### apis
setState()
forceUpdate()

## Components And Props
https://facebook.github.io/react/docs/components-and-props.html
- 개념적으로 component는 함수, props는 인수, 결과는 react element이다. 아래는 유효한 React component이다. 단일 인수를 받고 React element를 반환하기 때문이다.
```javascript 1.6
function Welcome(props) { return <H1> Hello, {props.name}> </h1>;  }
 ```
- ES6 class 로 정의할 수 도 있다.
```javascript 1.6
class Welcome extends React.Component { render () { return <h1> Hello, {this.props.name}</h1>;}}
````
- React element는 html tag또는 React.Component일 수 있다.
```javascript 1.6
const element = <div />;
const element = <Welcome name="sara" />
```
- 컴포넌트 명은 대문자롤 시작해야한다. <h1 />은 DOM tag로 <Welcome />는 React.Component로 간주하며 범위내에 Welcome가 있어야 한다.
- 컴포넌트는 single root element를 반환해야 한다. 
- 프로퍼티명은 컴포넌트의 관점에서 짓는게 좋다. 재사용 관점에서 컨텍스트에 따라 달라지지 않아야 하기 때문이다.

## State and Lifecycle
https://facebook.github.io/react/docs/state-and-lifecycle.html
- 직접 바꾸지 말것
```javascript 1.6
//wrong way : 
this.state.comment= 'x'  -> wrong -> constructor()에서만 할것.
//correct way : 
setState({comment: "x"})  -> correct 
```
- this.setState호출에 의한 state의 변경은 비동기로 일어날 수 있고, 성능 최적화를 위해 react가  여러번의 호출을 한번의 배치 호출로 바꿀 수 있다.  따라서 아래는 기대한 값을 가지지 않을 수 있다.
```javascript 1.6
//wrong way : 
this.setState({ counter: this.state.counter + this.props.increment, }); 
// 아래와 같이 콜백함수를 넘겨 처리해라.  arrow function이 호출된 당시의 props와 이전 state값을 받게 된다.
//correct way : 
this.setState((prevState, props) => ({counter: prevState.counter + props.increment}));
````

- 상태변경은 merge된다 .
```javascript 1.6
this.state = {a: [], b[]}
this.setState({a:[1]})
this.setState({b:[1]})
this.state => {a:[1], b:[1]} 
```

##  Handling Events
- binding event callbacks:
```javascript 1.6
// way one : 
constructor(props) { super(props); this. boundOnClick = this.boundOnClick.bind(this) } 
// -> binding을 잊지말것  
// way two : 
property initializer syntax :  class Toggle extends React.Component { boundOnClick = () => console.log('this is:', this);  } 
// -> experimental syntax 임.
```
## Forms
https://facebook.github.io/react/docs/forms.html

## Uncontrolled Components
https://facebook.github.io/react/docs/uncontrolled-components.html
- 기본 DOM tag의 기본 핸들러 form submit 같은것을 react로 끌어들이기 그리고 반드시 호출 : e.preventDefault();
- compute property name syntax
 ```
name='field1'; 
this.setState{[name]:1}) 
https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names
```
e.target.[name, type, value, checked, ...] 등을 이용해 여러 form elements들이 있을때 단일한 handler를 작성하면 좋을듯 
모두 controlled-components하는건 힘들때 차선책 
https://facebook.github.io/react/docs/uncontrolled-components.html

## Lifting State Up 
복수의 컴포넌트가 동일한 데이터를 가져야할 때 필요하다면 공동의 부모 컴포넌트에 상태 정보를 저장하기


## Optimiziing  
https://facebook.github.io/react/docs/optimizing-performance.html
### Avoid Reconciliation

### The Power Of Not Mutating Data
```
// wrong way
const words = this.state.words;
words.push('tail');
this.setState({words:words});
// correct way
this.setState(
	(prevState, props) => ({
		words:prevState.words.concat(['tail']) //concat return new copied array
		or
		words:[...prevState.words, 'tail']
		or for object
		Object.assign({}, coord, {x:10}); // returns new copied coord object with x=10
	})
);
```





