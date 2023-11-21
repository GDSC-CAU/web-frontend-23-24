# 목차

1. [Introduction](#introduction)
2. [Singleton Pattern](#singleton-pattern)
3. [Proxy Pattern](#proxy-pattern)
4. [Provider Pattern](#provider-pattern)
5. [Prototype Pattern](#prototype-pattern)
6. [Presentational and Container Pattern](#presentational-and-container-pattern)
7. [Observer Pattern](#observer-pattern)
8. [Module Pattern](#module-pattern)
9. [Mixin Pattern](#mixin-pattern)
10. [Mediator/Middleware Pattern](#mediatormiddleware-pattern)
11. [HOC Pattern](#hoc-pattern)
12. [Render Props Pattern](#render-props-pattern)
13. [Hooks Pattern](#hooks-pattern)
14. [Flyweight Pattern](#flyweight-pattern)
15. [Factory Pattern](#factory-pattern)
16. [Compound Pattern](#compound-pattern)
17. [Command Pattern](#command-pattern)

# Introduction

웹 개발에서 사용할 수 있는 다양한 디자인 패턴을 공부합니다.
참고 사이트: https://patterns-dev-kr.github.io

# Singleton Pattern

Singleton Pattern은 여러 인스턴스를 만들 수 없도록 하여 단 하나의 유일한 객체를 만들기 위한 코드 패턴이다. 객체에 하나의 인스턴스만 허용하고, 만들어진 인스턴스를 앱 전역에서 공유할 수 있도록 한다.

Singleton Pattern은 객체를 사용할 때 새로운 인스턴스를 만들지 않고 기존의 인스턴스를 가져와 활용한다.

## 장점

- 리소스를 많이 차지하는 객체를 사용할 때, 메모리를 절약할 수 있다.
- 어플리케이션에서 객체가 유일해야 하거나 유일한 것이 좋을 때, 객체를 하나로 강제할 수 있다.

## 단점

- 단일 객체가 다른 객체 내에서 사용되는 과정 등에서 의존성 파악이 어려워진다.
- 테스트가 어렵다. 인스턴스를 매번 생성할 수 없고, 테스트들이 실행에 순서가 생기게 되어 작은 수정사항이 전체 테스트의 실패로 이어질 수 있다.
- Singleton 인스턴스는 전역에서 접근 가능해야 하기 때문에 데이터의 흐름을 파악하기 어려워진다.

## 예시

대표적인 예시로는 데이터베이스 연결, 네트워크 통신, 캐시, 로그 기록 객체 등이 있다.

# Proxy Pattern

Proxy Pattern은 어떤 객체를 직접 다루는 것이 아니라 Proxy 객체를 통해 객체의 값을 설정하거나 조회할 때 인터랙션을 제어할 수 있다.

객체의 프로퍼티를 읽을 때는 `get` 메서드, 객체의 프로퍼티를 수정할 때는 `set` 메서드가 호출된다.

> **Reflect**
> Proxy와 함께 사용시 대상 객체를 쉽게 조작할 수 있다. 이 외에도 다양한 용도로 객체를 조작할 때, Reflect를 사용할 수 있다.
> `Reflect.get(obj, prop)`, `Reflect.set(obj, prop, value)` 등

## 예시

[Observer Pattern | 개발자 황준일](https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Store/#_1-최적화)

Observer Pattern을 구현할 때, 이 Proxy 객체를 사용하여 구현하는 방법도 있다. 추가로 [Vue3](https://vuejs.org/guide/extras/reactivity-in-depth.html#How-Reactivity-Works-in-Vue#how-reactivity-works-in-vue), MobX에서 반응형 시스템(상태를 바꾸면 UI가) 또한, Proxy 객체로 구현이 되어있다. ([0.7KB로 Vue와 같은 반응형 시스템 만들기-NHN Cloud](https://meetup.nhncloud.com/posts/188))

# Provider Pattern

앱 내부의 다양한 컴포넌트에서 같은 데이터를 사용해야하는 경우가 있다. 이 때, 동일한 데이터를 사용하는 컴포넌트가 모두 포함되는 상위 컴포넌트에서 데이터를 `props`를 통해서 전달하는 방식을 사용할 수 있다.

하지만 이러한 방식은 데이터가 필요한 컴포넌트를 포함하는 중간 컴포넌트들에도 모두 props를 전달해주어야 하며, 컴포넌트 트리의 depth가 깊어질 경우 데이터가 어디서부터 내려오는지 알기 어려워진다. 의존성이 더욱 깊어져 리팩토링 또한 어렵다. 이러한 안티패턴을 `props drilling`이라고 부른다.

Provider Pattern는 이렇게 여러 컴포넌트에서 동일한 데이터를 사용해야할 때 유용한 패턴입니다. React에서는 모든 컴포넌트(혹은 동일한 데이터를 필요로하는 모든 컴포넌트나 그 상위 컴포넌트)를 Provider로 감싼다. Provider는 고차 컴포넌트(HOC)로 Context 객체를 제공하며 `createContext()` 메서드로 Context를 생성할 수 있다. 그 후, 데이터를 사용하는 컴포넌트에서 `useContext` 훅을 사용하여 해당 데이터들을 불러올 수 있다.

커스텀 훅을 사용하면 각 컴포넌트에서 useContext 훅이 아니라 필요로 하는 컨텍스트를 직접 반환하는 훅을 만들 수 있다.

### 장점

이러한 Provider 패턴은 컴포넌트 트리에서 하위 컴포넌트에 데이터를 직접 전달하지 않아도 여러 컴포넌트에 데이터를 전달할 수 있다. 또한, 데이터 흐름을 파악하기 좋아 개발하면서 생길 수 있는 실수를 줄일 수 있다. 상태관리 라이브러리 등 많은 라이브러리가 이러한 Provider 패턴을 통해서 기능을 제공한다.

### 단점

Provider 패턴을 과하게 사용할 경우 특정 상황에서 성능 이슈가 발생할 수 있다. 컨텍스트를 참조하는 모든 컴포넌트는 컨텍스트가 변경될 경우 Rerendering 되는데, 이는 성능이 저하될 수 있다.
따라서 Provider 패턴을 사용할 경우, 성능 저하를 방지하기 위해 여러 Provider로 쪼개어 사용하는 것이 좋다. (상태값 & 액션 등) 하지만 이러한 방식으로 사용시 너무 많은 Provider로 감싸야 하는 Provider Hell이라는 안티패턴이 등장하게 된다.
**전역 상태관리 라이브러리**를 사용하는 이유는 이러한 안티패턴을 방지하고 하나의 Provider로 데이터를 전달하기 위함도 있다고 생각한다. Context API

# Prototype Pattern

자바스크립트는 동일 타입의 여러 객체들이 프로퍼티를 공유할 때, 중복된 프로퍼티들이 존재하는 인스턴스를 매번 생성하는 것이 아니라 Prototype Chain을 통해서 연결되어 중복된 프로퍼티는 Prototype에서 가져온다. Prototype에 프로퍼티를 추가하면 모든 인스턴스가 Prototype 객체를 활용할 수 있다. **일종의 상속 기능이라고 볼 수 있다.**

`Object.create`메소드는 Prototype으로 사용할 객체를 인자로 받아 새로운 객체를 만들어낸다. 즉, 인자로 받는 객체를 Prototype으로 하는 새로운 객체를 생성해서 반환해주는 함수이다.

이는 메서드 중복을 줄임으로써 메모리를 절약할 수 있게 만들어준다.

그렇다면 프로토타입은 단순히 자바스크립트에서 상속을 지원하기 위해서 사용되는 방법일까? 생각해보자.
추천글 : [자바스크립트는 왜 프로토타입을 선택했을까](https://medium.com/@limsungmook/자바스크립트는-왜-프로토타입을-선택했을까-997f985adb42)

# Presentational and Container Pattern

> Presentational and Container Pattern을 처음으로 제시했던 Dan Abramov는 더 이상 이 패턴을 사용하여 컴포넌트를 분할하는 것이 좋지 않다고 말합니다.
> 이 패턴은 Hook이 나오기 이전에 복잡한 상태 저장 로직을 분리하기 위해 사용하던 것이었지만, 무분별하게 사용되기도 했으며 현재는 Hook의 등장으로 컴포넌트를 분할하지 않아도 동일한 작업을 수행할 수 있기 때문입니다.

비즈니스 로직과 뷰 로직을 분리하는 방법으로, 대표적인 예시로는 데이터를 서버로부터 패칭하는 컴포넌트와 데이터를 통해 컴포넌트를 렌더링하는 컴포넌트를 분할하는 것이 있다.

### 장점

- Presentational 컴포넌트는 앱의 여러곳에서 다양한 목적으로 재사용할 수 있다.
- UI를 담당하는 순수함수로 작성하게 되어 테스트가 쉽다.

### 단점

- 현재는 Hook을 사용하여 비즈니스 로직을 분리할 수 있어 사용하지 않는 것이 좋다.
- Hook을 사용하여 관심사를 분리하더라도 너무 작은 앱에서는 오버엔지니어링일 수 있다.

# Observer Pattern

Observable을 활용해 observer에게 이벤트 발생을 알린다.

이벤트가 발생할 때 마다 Observable은 모든 Observer에게 이벤트를 전파한다.

**Observer(관찰자) : 상태 변화를 감지하는 대상**

옵저버에는 함수나 객체 모두 등록 가능

**Observable(객체) : 상태가 변경되는 대상(관찰 대상)**

subscribe, unsubscribe, notify 등 행동을 처리하는 메서드 필요

⇒ **`Observer가 Observable을 관찰한다`**

## 적용 예시

선생님 : Tom (대상)
학생 : Jane, Kane, Sane (관찰자)

1. 학생은 선생님의 모든 변화를 **감지**해야 한다.
2. 선생님은 학생들에게 Notify 할 수 있어야 한다.

학생(Observer)은 변화를 감지하여 **자신의 상태를 변화**시키는 메소드 필요
선생님(Observable)은 학생 등록, 등록 해제, 알림할 수 있어야 한다.
선생님은 자신을 구독한 학생들을 알고 있고, 학생들도 자신이 구독하는 선생님을 알고 있다.
그리고 학생은 선생님으로부터 받은 알림이 오면, 각자 정의된 행동을 수행하게 된다. **(update)**
**subscribe와 unsubscribe**를 통해 구독 등록과 해제가 가능하고 **notify**를 통해 변화를 알린다.

```jsx
class Observable {
  constructor() {
    this.observers = [];
  }

  subscribe(func) {
    this.observers.push(func);
  }

  unsubscribe(func) {
    this.observers = this.observers.filter((observer) => observer !== func);
  }

  notify(data) {
    this.observers.forEach((observer) => observer(data));
  }
}
```

보통 유저의 입력을 받는 서비스를 제공하면 이벤트가 발생했을 때 데이터가 변경된다.

이때 **사용자의 인터렉션을 추적(관찰)하여 그에 맞게 화면을 변화시킨다.**

예를 들어 버튼 클릭과 스위치를 토글할 때 타임스탬프를 콘솔에 출력하려고 한다.

`handleClick` 또는 `handleToggle` 함수를 호출할 때마다 Observable의 `notify`  호출.

notify() 는 등록된 모든 Observer에게 전달된 데이터를 포함한 이벤트 전파.

1. `observable.subscribe()` : observable를 관찰할 관찰자를 등록한다.
2. button을 클릭하면 handleClick함수를 실행한다.
3. `observable.notify()` 가 실행되어 함수의 파라미터로 입력이 들어가 observer의 함수가 실행된다.

```jsx
import React from 'react';
import { Button, Switch, FormControlLabel } from '@material-ui/core';
import { ToastContainer, toast } from 'react-toastify';
import observable from './Observable';

function handleClick() {
  observable.notify('User clicked button!');
}

function handleToggle() {
  observable.notify('User toggled switch!');
}

function logger(data) {
  console.log(`${Date.now()} ${data}`);
}

function toastify(data) {
  toast(data, {
    position: toast.POSITION.BOTTOM_RIGHT,
    closeButton: false,
    autoClose: 2000,
  });
}

observable.subscribe(logger);
observable.subscribe(toastify);

export default function App() {
  return (
    <div className='App'>
      <Button variant='contained' onClick={handleClick}>
        Click me!
      </Button>
      <FormControlLabel control={<Switch name='' onChange={handleToggle} />} label='Toggle me!' />
      <ToastContainer />
    </div>
  );
}
```

## 핵심

**관찰자들이 “항상” 관찰 대상을 주시하고 있는 게 아니다.**

어떠한 변경 사항을 확인하기 위해서 계산 자원과 시간을 소비한다 → 신문을 보기 위해 매일 사러 나갔다 오는 것

상태 변화를 기다리는 동안 관찰자들이 하던 일을 계속 진행할 수 있다 → 신문 구독을 함으로써 매일 문 앞으로 신문이 배달되는 상황

**옵저버 패턴의 핵심은 의존성을 낮추는 것 ( = 결합도를 낮추는 것)**

객체 간 강한 결합 : A의 상태 변화에 따라 직접 B의 상태를 변경시키는 코드

옵저버 패턴을 통해 객체 간의 관계를 **`느슨한 결합`** 으로 만들 수 있음

객체의 상태 변화를 옵저버에서 알 수 있고 의존 관계가 1:N 이기 때문에 옵저버를 여러 개 만들 수도 있다.

## 옵저버 패턴 주의할 점

observer와 observable 간 모르는 상태가 아니다.
서로 아예 모르는 패턴 → **`구독-발행 패턴(pub-sub pattern)`**
pub-sub 패턴은 기존 옵저버와 옵저버블의 사이에, 브로커 혹은 메시지 큐라는 중개자를 두고
옵저버 <-> 브로커 <-> 옵저버블 의 소통을 수행한다.

## 정리

Observer 패턴을 사용하는 것도 **관심사의 분리와 단일 책임의 원칙을 강제하기 위한 좋은 방법**이다.
Observer 객체는 Observable 객체와 느슨한 결합 상태이기 때문에 언제든지 분리될 수 있다.
Observable : 이벤트 모니터링의 역할
Observer : 받은 데이터를 처리하는 역할

# Module Pattern

# Mixin Pattern

Mixin Pattern은 객체나 클래스에 상속 없이 재사용할 수 있는 기능을 추가하는 코드 패턴이다. 일종의 다중상속(클래스가 여러 상위 클래스로부터 상속받는 기능)과 같다고 할 수 있는데, 객체의 일정 부분을 쪼개어 조립하는 형태라고 보는 것이 좋다.

Javascript는 단일상속만을 허용하는 언어로, 하나의 Prototype만을 가지며 클래스는 하나의 클래스만 상속받을 수 있다. Mixin은 단독으로 사용되지 않고 여러 객체의 메서드(혹은 프로퍼티)를 조합하는 것을 말한다. Javascript에서는 `Object.assign` 메서드를 사용하여 mixin을 구현한다. Mixin이 기존 클래스의 메서드를 덮어쓸 경우 충돌이 발생할 수 있기 때문에 충돌이 발생하지 않도록 메서드 이름을 작성해야 한다.

## 예시

`Window` 객체는 `WindowOrWorkerGlobalScope`와 `WindowEventHandler`의 mixin으로 구성되어 있어 다양한 메서드를 사용할 수 있다. 하지만 해당 mixin을 직접 사용할 수는 없다.
React에서는 Higher Order Component 혹은 Hooks를 사용하여 기능을 추가할 수 있으며 mixin으로 인해 발생하는 복잡도 증가와 재사용성 감소 때문에 mixin 사용을 권장하지 않는다.

# Mediator/Middleware Pattern

Mediator/Middleware Pattern은 컴포넌트들간 직접적인 통신 대신, 중간에 객체 혹은 함수를 통해서 필요한 정보를 전달해주는 Middleware/Mediator(중재자)를 두는 코드 패턴으로, **객체간의 상호작용을 캡슐화시켜주는 역할**을 한다. Mediator/Middleware Pattern은 객체간의 상호작용을 캡슐화하여 다 통신을 다대일 통신으로 바꿔 객체간 느슨한 결합을 유지할 수 있도록 돕는다.

많은 비즈니스 로직이 여러 클래스들로 나누어져 있지만, 클래스 간의 상호작용이 필요한 로직도 존재한다. 이런 로직이 많아질수록 각 클래스 간의 결합도가 높아지면서 유지•보수가 어려워진다.

이 때, 객체들끼리 통신을 하는 것이 아니라 중재자 역할을 하는 객체를 만든다. 그리고 통신이 필요할 경우 중재자 객체에 요청을 보내면 중재자 객체에서 요청을 처리하여 필요로 하는 객체에게 전달한다.

## 장점

- 객체 간의 직접적인 통신 대신 중재자 객체를 통해 간접적으로 통신함으로써 **느슨한 결합**을 제공하여 유연성과 유지보수성을 향상시킨다.
- 객체 간의 복잡한 상호작용을 중재자에게 위임하여 **전체 시스템의 복잡성을 감소**시킨다.

## 단점

- 새로운 객체나 기능이 추가될 때, 중재자 객체에 추가되어야 하는 로직이 많아질 수 있다. 이로 인해 중재자 객체가 너무 많은 역할을 담당하게 되면서 중재자에 장애가 발생하면 시스템 전체에 영향을 미칠 수 있다. (단일지점 장애)

## 예시

대표적인 예시는 채팅 구현이다. 채팅 기능은 사용자끼리 직접 메시지를 주고받도록 설계하지 않는다. 사용자가 채팅 서버로 메시지를 전송하고 채팅 서버는 전달해야하는 사용자들에게 메시지를 전달하는 형태로 구현한다. 이 때, 채팅 서버가 중재자 역할을 하는 것이다. 이로 인해서 채팅 서버에 장애 발생시 전체 이용자가 채팅을 사용하지 못한다.

> [!Note]
>
> - 이벤트 중심으로 객체간의 느슨한 결합을 제공하여 시스템의 유연성, 확장성, 유지보수성을 높이는 점에서 [Observer Pattern](#observer-pattern)과 유사하다. 하지만 Observer Pattern은 한 객체의 상태를 다른 객체들에게 직접적으로 통지하는 형태(일대다의 관계)로 구성되는 반면에, Mediator/Middleware Pattern은 중재자 객체가 여러 객체를 이어주는 역할을 하며 객체간 연결을 간접 연결로 바꿔주는 형태(다대다를 다대일의 형태로 연결해주는 관계)로 구성된다.
> - 중간에서 요청을 받아 작용하는 중재 역할을 하는 측면에서는 [Proxy Pattern](#proxy-pattern)과 유사하다. 하지만 Proxy Pattern은 객체에 대한 접근을 제어하거나 추가적인 작업을 수행할 때 사용하는 패턴으로 한 객체를 wrapping하여 제어하거나 보호할 때 사용하는 반면에, Mediator/Middleware Pattern은 여러 객체를 연결하는 역할로 객체간의 상호작용을 조정하고 관리하는데 사용한다.

# HOC Pattern

# Render Props Pattern

# Hooks Pattern

# Flyweight Pattern

# Factory Pattern

# Compound Pattern

# Command Pattern
