## Context API

> Context API는 react의 내장 api로 props 없이 component로 state를 쉽게 공유할 수 있게 해준다.

1. 이해

   React는 component 간에 데이터를 전달할 때 props를 이용한다. 하지만 여러 component를 지나서 전달해야 하거나(props drilling), 여러 component들이 동일한 데이터를 필요로 하는 경우가 있다. 이때 중간의 component에 불필요한 데이터를 전달한 다는 점에서 코드가 다소 복잡해 질 수 있다.

   ```javascript
   function App() {
     const data = "data in props";
     return (
       <>
         <ParentComponent data={data} />
       </>
     );
   }

   function ParentComponent({ data }) {
     return (
       <>
         <ChildComponent data={data} />
       </>
     );
   }

   function ChildComponent({ data }) {
     return (
       <>
         <p>{data}</p>
       </>
     );
   }

   export default App;
   ```

   Context API는 이러한 상황에서 중간 component를 모두 건너뛰고 데이터를 필요로 하는 component에게 즉시 전달할 수 있다. Component의 상하 관계를 무시하고 필요한 component를 불러 사용할 수 있다는 것이다.

2. rendering

   Context API에서 state를 변경하게 되면, provider로 감싼 모든 component들이 rerendering된다. 따라서 전역적으로 쓰이지만 자주 변경할 필요가 없는 값을 이용해야 할 때 context API를 채택하면 좋다.

3. 사용

   `React.createContext`를 통해 context를 생성할 수 있으며, 인자에 어떠한 값을 넣으면 그 값으로 초기화된다. 또한 `Provider`를 사용하여 하위 component들에게 데이터를 전달할 수 있다.

   ```javascript
   const contextExample = createContext();

   function App() {
     const [value, setValue] = useState("data in context");
     return (
       <contextExample.Provider value={value}>
         <ParentComponent />
       </contextExample.Provider>
     );
   }

   function ParentComponent() {
     return (
       <>
         <ChildComponent />
       </>
     );
   }

   function ChildComponent() {
     const data = useContext(contextExample);
     return (
       <>
         <p>{data}</p>
       </>
     );
   }
   ```

   위 예제에서 `MyContext`를 생성하고, `App` component에서 `MyContext.Provider`를 사용하여 `value`를 전달하고 있다. `ChildComponent`에서는 `useContext` hook을 사용하여 `MyContext`의 값을 받아와서 사용하고 있다.

---
