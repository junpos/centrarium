---
layout: post
title:  "React: 'aHa' Moments"
date:   2017-04-02 17:20:59
author: Jun Kim
categories: react
---

### React "aHa" Moments

이 글은 React Newsletter 이슈 69에 실린 [React "Aha" Moments](https://tylermcginnis.com/react-aha-moments/?utm_campaign=React%2BNewsletter&utm_medium=web&utm_source=React_Newsletter_69) 의 번역글이고 매끄러운 번역을 위해 과감하게 생략/의역/부연설명을 하였다.


#### 1) `fn(d) = V`. Your UI is a function of your state and props are to components what arguments are to functions.

React의 장점중에 하나는 컴포넌트를 어디서 또는 어떻게 사용하지에 관해 자바스트립트에서 배운 함수에 대한 통찰/경험을 그대로 이용할수 있다는 점이다. 함수가 파라미터를 받아 어떤 값을 리턴하는데 비해서, React의 함수는 파라미터를 받아 UI의 상태를 Object로 반환(an object representation of your UI)한다는 점에서 다르다. 이런 아이디어는 `fn(d) = V` 라는 식으로 표현될 수 있는데, 데이타(d)를 받아 뷰(V) 를 반환하는 함수(fn) 라는 것이다. 유저 인터페이스를 위의 방식으로 생각하는 방식은 UI가 단순히 여러개의 함수들의 조합(composition)으로 이루어져 있다는 사실을 알게해준다. 우리는 이런 장점들을 적극 활용하여 UI를 만들수 있다.

#### 2) In React, your entire application’s UI is built using function composition and JSX is an abstraction over those functions.

처음 리액트를 사용하는 사람들의 대부분의 반응은, "리액트는 정말 멋지지만 JSX는 마음에 들지 않아요. 복잡성의 분리(separation of concerns)를 해치는 것 같구요." 을 보인다. 하지만 JSX는 HTML과는 다르고 단순한 템플레이팅 언어를 수준을 넘어선다. JSX의 사용에 있어 집고 넘어갈 두가지 중요한 사실을 살펴보자. 첫번째로 JSX는 `React.createElement`의 추상화를 제공하는데 JSX가 쓰여지고 트랜스파일(transpile)이 되면 이것은 실제 DOM을 표현하는 자바스트립트 Object로 변환된다. 이후 리엑트는 이 Object를 분석하고 이전에 구성된 DOM표현과 비교하여 바뀐 부분에 대해서 DOM 업데이트를 실행하게 된다. 이런 과정은 성능개선을 가져오게 되는되는데 가장 중요한 사실은 JSX가 순전히 자바스크립트라는 점이다. 두번째로 JSX가 자바스트립트 이므로 언어상 제공되는 여러 이점들(composition, linting, debugging)을 자동으로 얻게 되지만 여전히 HTML의 명시성을 유지할수 있다는 점이다.


#### 3) “Components don’t necessarily have to correspond to DOM nodes.”

처음 리액트를 배울때는 "컴포넌트들은 리액트를 구성하는 요소(Block)들이다. 컴포넌트는 input을 받아 UI를 반환한다" 라고 배운다. 이것이 모든 컴포넌트들이 직접적으로 UI만을 반환한다는 말일까? 만약 우리가 다른 컴포넌트를 반환하는 컴포넌트(Higher Order Component)가 필요하면 어떨까? 상태의 부분을 관리하는 컴포넌트를 원한다면? 넘겨받은 상태를 실행시키는 함수를 반환하기 원한다면? 비쥬얼 UI가 아닌 소리를 담당하는 컴포넌트 라면?
리액트는 가장 큰 장점은 반드시 뷰(view)를 반환할 필요가 없다는 점이다. 궁극적으로 리액트 element, null, false 를 반환한다면 모두 허용된다.

다른 컴포넌트를 반환하는 경우
```js
render() {
  return <MyOtherComponent />;
}
```
다른 함수를 실행시켜 반환하는 경우
```js
render () {
  return this.props.children(this.someImportantState)
}
```
단순히 아무것도 반환하지 않는 경우
```js
render () {
  return null
}
```
나는 Ryan Florence의 [React Rally talk](https://www.youtube.com/watch?v=kp-NOggyz54)을 좋아하는데 여기서 이러한 원리들이 자세히 다뤄지고 있다.


#### 4) “When two components need to share state, I need to lift that state up instead of trying to keep their states in sync.”

컴포넌트로 구성된 구조/설계는 컴토넌트간 상태공유가 자연적으로 어렵다. 만약 두개의 컴포넌트가 하나의 상태에 의존하고 있다면 어디에 그 상태를 저장해야 할것인가? 이것은 상당히 유명한 문제로서 전체 ecosystem에 화두를 번젔고 그 결과 Redux가 소개되었다. Redux의 해결방식은 그 상태를 또 다른 장소인 스토어(store)에 넣는것이다. 컴포넌트들은 이 스토어의 한 부분을 subscribe 하고 action을 dispatch 해서 스토어를 업데이트 시킬수도 있다. 반면 리액트의 해결방법은 두 컴포넌트의 가장 가까운 조상(parent)을 찾아 이 컴토넌트에 상태관리(state management)를 맡기고 자식들에게 데이타를 전달해 주는 방식을 택한다. 두 방식 모두 장단점이 존재하지만 이 모두를 이해하는 것이 중요하다.


#### 5) “Inheritance is unnecessary in React, and both containment and specialization can be achieved with composition.”

리액트는 언제나 함수형 프로그래밍의 원리를 잘 흡수하여 왔다. 상속(Inheritance)에서 함수조합(composition)으로의 변화는 리액트 v0.13이 소개되었을 때 Mixin과 ES6 classes를 지원하지 않기로 함으로써 더욱 확실해 졌다. 그 이유는 Mixin이나 상속을 사용하여 만들수 있는 거의 모든것들이 함수조합를 통해서 더 작은 side effect를 발생시키며 가능하게 때문이다. 만약 당신이 상속에만 얶매어 있는 상태로 리액트를 처음 보게 된다면 이러한 새로운 사고방식이 어렵거나 자연스레 이해되지 않을수 있다. 다행히도 많은 글들이 당신을 도와줄 수 있는데 특히 리액트에 한정되어 있지 않는 이 비디오, [composition over Inheritance](https://www.youtube.com/watch?v=wfMtDGfHWpA),를 추천한다.

#### 6) “The separation of container and presentational components."

리엑트 컴포넌트의 내부를 떠올리면 대부분 어떤 states(상태)와 lifecycle hooks 그리고 JSX를 통한 markup이 생각난다. 만일 이 모든 것들은 하나의 컴포넌트에 넣지 않고 두 개의 컴포넌트로 분리하여 관리한다면 어떨까? 첫번째 컴포넌트는 상태와 lifecycle hooks를 가지고 컴포넌트가 *어떻게 작동* 되는지에 초점을 맞추는 반면 두번재 컴포넌트는 데이타를 props으로 받아서 컴포넌트가 *어떻게 보여야* 하는지에 관심을 두게 하는 것이다. 이러한 방법은 두번째 컴포넌트(presentational component)가 어떤 데이타를 받는지 관심이 없게 만듦으로 이 컴포넌트의 재사용성을 높여준다. 이런 분리과정을 통해 디자이너들도 UI를 데이타(상태, lifecycle hooks) 걱정없이 작업할 수 있게 도와준다.

#### 7) “If you try to keep most of your components pure, stateless things become a lot simpler to maintain.”

이것은 presentational 컴포넌트와 container 컴포넌트의 분리에서 오는 여러 장점중에 하나이다. 상태는 결국 inconsistency를 동반하기 때문이다. 이 두가지를 분리하고 복잡성을 각자 관리하면서 우리는 어플리케이션이 어떻게 작동할지 의 예측을 더욱 쉽게 할수 있게 된다.
