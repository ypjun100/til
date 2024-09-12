# Higher Order Component (고차 컴포넌트)

```jsx
// HOC 함수
const withAuthentication = (WrappedComponent) => {
	return (props) => {
		const [isAuthenticated, setIsAuthenticated] = useState(false);
		
		useEffect(() => {
			auth.isAuthenticated().then((authenticated) => {
				setIsAuthenticated(authenticated);
			});
		}, []);
		
		return isAuthenticated ? (
			<WrappedComponent {...props} />
		) : (
			<div>Loading...</div>
		);
	};
};

// 사용
const MyComponent = (props) => {
	return <div>My Component</div>;
};
export default withAuthentication(MyComponent);
```

* 컴포넌트를 입력으로 받아 새로운 컴포넌트를 반환하는 함수로, 재사용성, 유연성의 이점을 얻을 수 있음
* 리액트 훅이 등장한 이후에는 HOC 대신 훅을 사용하는 경우가 많아졌지만, 그래도 활용되는 경우가 있음

### 코드 재사용

* 여러 컴포넌트에서 사용되는 공통 로직이 있는 경우, 로직을 HOC로 작성하여 중복 코드를 줄일 수 있음
* HOC에서 반환된 컴포넌트에는 공통된 로직이 포함되어 있어, 추가 작업 없이 해당 로직을 활용할 수 있음
* 유지보수 측면에서도 HOC 내부의 로직만 수정하면, 모든 컴포넌트에 변경 사항이 적용되므로 효율적임

### props 조작

* HOC를 사용하여 컴포넌트에 전달되는 props를 수정하거나 추가하여 컴포넌트 간의 결합도를 낮춤
* 컴포넌트에 전역 상태나 컨텍스트 데이터를 props로 주입할 때 활용하여 재사용성과 유연성을 높임

### 렌더링 제어

* HOC를 사용하여 특정 상황에 따라 다른 UI를 렌더링하거나, 로딩 인디케이터를 표시하는 작업을 수행함
* 인디케이터뿐만 아니라 에러 메시지나 미인증 유저는 다른 페이지로 리다이렉션하는 등의 동작을 수행함