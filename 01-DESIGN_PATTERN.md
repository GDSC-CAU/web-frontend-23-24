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
> `Reflect.get(obj, prop)`,  `Reflect.set(obj, prop, value)` 등

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
> - 이벤트 중심으로 객체간의 느슨한 결합을 제공하여 시스템의 유연성, 확장성, 유지보수성을 높이는 점에서 [Observer Pattern](#observer-pattern)과 유사하다. 하지만 Observer Pattern은 한 객체의 상태를 다른 객체들에게 직접적으로 통지하는 형태(일대다의 관계)로 구성되는 반면에, Mediator/Middleware Pattern은 중재자 객체가 여러 객체를 이어주는 역할을 하며 객체간 연결을 간접 연결로 바꿔주는 형태(다대다를 다대일의 형태로 연결해주는 관계)로 구성된다. 
> - 중간에서 요청을 받아 작용하는 중재 역할을 하는 측면에서는 [Proxy Pattern](#proxy-pattern)과 유사하다. 하지만 Proxy Pattern은 객체에 대한 접근을 제어하거나 추가적인 작업을 수행할 때 사용하는 패턴으로 한 객체를 wrapping하여 제어하거나 보호할 때 사용하는 반면에, Mediator/Middleware Pattern은 여러 객체를 연결하는 역할로 객체간의 상호작용을 조정하고 관리하는데 사용한다.


# HOC Pattern


# Render Props Pattern


# Hooks Pattern


# Flyweight Pattern


# Factory Pattern


# Compound Pattern


# Command Pattern

