# HOC와 Component Composition

### HOC(Higher-Order Component)

```jsx
function withStyling(WrappedComponent) {
	return function(props) {
		return (
			<div style={{ padding: '20px', color: 'blue' }}>
				<WrappedComponent {...props} />
			</div>
		);
	};
}
```

* 컴포넌트를 파라미터로 입력받은 후, 여기에 부가 기능을 추가한 새로운 컴포넌트를 반환하는 함수
	* 기존 컴포넌트를 수정할 필요없이, 컴포넌트를 기반으로 새로운 기능을 랩핑함
* HOC를 사용하는 예시로 페이지 전반의 인증 기능, 라우팅, 스타일링 및 특성을 통일하는 경우가 있음

> **사용 예시**
> * 앱 내의 전반적인 분석을 위해 HOC를 적용하여 로깅을 위한 함수를 주입함
> * 특정 페이지에서 유저의 권한을 확인하기 위해 HOC를 적용하여 조건부 렌더링을 구현함

### Component Composition

```jsx
function Layout({ header, content, footer }) {
	return (
		<div>
			<header>{header}</header>
			<main>{content}</main>
			<footer>{footer}</footer>
		</div>
	);
}
```

* 복잡한 컴포넌트를 관리하기 쉽도록 더 작은 컴포넌트로 분해하여 이를 하나의 컴포넌트에 포함하는 것
* 이러한 모듈러 구조는 개발을 간소화할 뿐 아니라 유지 보수성과 확장성을 강화시킬 수 있도록 도움

> **사용 예시**
> * 공통적으로 사용되는 버튼, 모달창을 UI Kit으로 구성하여 쉽게 재사용할 수 있도록 구현함
> * 헤더, 사이드바, 콘텐츠 영역을 포함하는 레이아웃 컴포넌트를 구현함

### 두 기법 간의 차이점

* **HOC**
	* HOC 호출자로부터 기존 컴포넌트를 감추기 위해 컴포넌트를 랩핑하여 새로운 컴포넌트를 반환함
	* 원본 컴포넌트의 렌더링 과정과 `props`를 관리하는 책임을 HOC에 위임하여 역할을 분리함
	* 이러한 분리는 컴포넌트 로직을 단순화할 수 있지만 책임이 모호해질 수 있기에 항상 바람직하진 않음
* **Component Composition**
	* HOC와 달리 직접적으로 JSX 마크업을 반환하며, 렌더링 메서드 내에서 명시적으로 사용됨
	* 컴포넌트들이 JSX 내에서 명시적으로 중첩되어 개발자가 컴포넌트 구조를 더 쉽게 확인할 수 있음