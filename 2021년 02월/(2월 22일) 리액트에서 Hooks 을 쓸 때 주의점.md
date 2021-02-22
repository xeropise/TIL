### 리액트에서 Hooks 을 쓸 때 주의점

1) useHooks 를 사용할 때는 순서를 잘 지켜라

2) useHooks 를 조건문에서 쓰거나, useEffect 혹은 반복문안에서 쓰는 것을 자제해라. (웬만하면 절대 사용하지 말 것) 

3) useCallback 은 함수, useMemo는 값을 메모리제이션 한다. 자식 컴포넌트에게 함수를 전해줄때는 반드시 useCallback 하는것이 좋다.  
   함수형 컴포넌트에서 값이 바뀔때 컴포넌트 안에 모든 값들이 전부 재생성 되는데, useCallback 을 사용하면 최적화를 막기 때문
   
4) useEffect 에서 setInterval 을 사용하면 반드시 제거 해줘야 한다. 메모리를 사용하기 때문 return 에서 clearInterval 할 것   

5) useEffect 를 사용할 때는 console.log 를 찍어서, 최적화에 이용할 것. 습관을 들이는 것이 좋다.

6) useEffect 에서 componentDidMount 시 실행 안하게 하려면, 아래의 패턴을 이용해 사용할 수 있다.
```react
const mounted = useRef(false);
useEffect(() => {
  if (!mounted.current) {
    mounted.current = true;
  } else {
  
  }
}, [바뀌는 값]);
```

7) React.memo 를 사용해서 자식 컴포넌트에서 props 가 변경시에만 재 렌더링 하도록 최적화 할 수 있다. (클래스는 PureComponent)
