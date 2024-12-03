---
title: Reactjs
tags: [Web, Framework]

---

###### tags: `Web` `Framework` 
# Reactjs

# **Rules of HOOKS**

- Chỉ gọi react hooks trong React Functions
- Chỉ gọi react hooks ở top level, k gọi trong vòng lặp, if, func lồng nhau  => đảm bảo gọi đúng thứ tự mỗi lần render, có được đúng state giữa n lần gọi useState, useEffect

# **Higher-Order Components**
- **làm một hàm nhận vào một component và return về một components**
- `const EnhancedComponent = higherOrderComponent(WrappedComponent);`
- Hook là hàm bắt đầu bằng `use`, chỉ có sẵn khi React rendering

# **Render Props**

- Là kĩ thuật chia sẻ code giữa các React Components bằng cách dùng giá trị props có giá trị là một hàm (func)

```javascript=
- Một component có một render prop sẽ lấy một hàm trả về một phần tử 
React (React element) và gọi hàm đó thay vì phải thực hiện render với 
logic riêng biệt.

<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```
# Component

- Vòng đời Component
    - A component mounts when it’s added to the screen.
    - A component updates when it receives new props or state. - This usually happens in response to an interaction.
    - A component unmounts when it’s removed from the screen.

# **State: A component's memory**
:::info
Happens in action
**const [index, setIndex] = useState(0);**
1. Render lần đầu: init = 0 => [0, setIndex] và React nhớ 0 là `state index` mới nhất
2. Update State: click btn gọi setIndex(index + 1) => setIndex(1). React nhớ index = 1 và trigger render khác
3. Second Render: React vẫn nhìn thấy useState(0) nhưng nó nhớ index = 1 nên nó return [1, setState]
4. And so on!

- Khi render lại thì sẽ reset lại tất cả, bao gồm cả local variable
:::

# **Render and Commit**

- Hiểu về các bước để component hiển thị ra màn hình
:::info
- Tưởng tường: component là đầu bếp trong nhà bếp, chế biến các món ăn ngon từ nguyên liệu. Quy trình gồm 3 bước
1. Triggering a render (giao đơn đặt hàng của khách đến nhà bếp)
2. Rendering the component (chuẩn bị đơn hàng trong nhà bếp)
3. Commiting to the DOM (đặt đơn hàng lên bàn của khách)
:::

# **State as a Snapshot**

![](https://i.imgur.com/LWMUGGZ.png)


## **Rendering takes a snapshot in time**
- "Rendering" là React gọi component, JSX return từ func như một snapshort của UI lúc đó. Props, event hanldes, local variables sẽ được tính toán bằng cách sử dụng state tại thời điểm render đó
- Setting state chỉ thay đổi trog lần render tiếp theo
:::info
- Setting state does not change the variable in the existing render, but it requests a new render.
- React processes state updates after event handlers have finished running. This is called batching.
:::
- Khi React hiển thị một component
    1. React gọi func
    2. Func return một snapshot JSX mới
    3. React update lại màn hình cho phù hợp với snapshot
- e.g:
    1. Bạn yêu cầu React update state
    2. React update state
    3. React truyền một snapshot của một giá trị state vào component
- React waits until all code in the event handlers has run before processing your state updates.
    - React sẽ chờ tất cả các event handlers chạy(chạy hết setState) rồi mới update state. Đây là lý do tại sao re-render chỉ xảy ra khi setState() được gọi
 => Giống với: ng phục vụ gọi món, ng phục vụ k chạy vào bếp để đặt đơn hàng mà sẽ chờ cho khách hàng hoàn thành đơn đặt hàng (thay đổi đơn) và thậm chí còn nhận đơn từ khách hàng khác trong bàn
 => Update nhiều state, component mà k cần re-render n lần
 ```javascript=
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);
  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}

- (n => n + 1) gọi là update func khi bạn truyền nó cho setState
1. React sẽ xếp queue để xử lý sau khi tất cả các mã khác trong trình xử lý sự kiện đã chạy.
2. Trong suốt quá trình render tiếp theo, React sẽ đi qua từng queues và update state cuối cùng
- React thực hiện khi thực thi event hanldes
1. setNumber(n => n + 1): n => n + 1 is a function. React adds it to a queue.
2. setNumber(n => n + 1): n => n + 1 is a function. React adds it to a queue.
3. setNumber(n => n + 1): n => n + 1 is a function. React adds it to a queue.
=> khi gọi useState ở lần render tiếp theo, React sẽ đi qua từng queue
```
- Ta coi state là read-only chỉ thay đổi được giá trị khi gọi hàm setState()

# **Queueing a Series of State Updates**

- UI sẽ không update cho đến khi các event handler được gọi (các setState được thực hiện) và bất kì code trong nó được hoàn thành. Hành vi này được gọi là **batching** -> Giúp React app chạy nhanh hơn

# **Chia sẻ State giữa các component**

- Nếu bạn muốn state của hai component luôn luôn thay đổi cùng nhau, xóa state của componet đó và chuyển nó state đó lên parent component chung gần nhất => Gọi là: **lifting state up**

:::info
1. When you want to coordinate two components, move their state to their common parent.
2. Then pass the information down through props from their common parent.
3. Finally, pass the event handlers down so that the children can change the parent’s state.
4. It’s useful to consider components as “controlled” (driven by props) or “uncontrolled” (driven by state).
:::

# **Bảo quản và thiết lập lại State**

![](https://i.imgur.com/fiabMxg.png)

## **Trạng thái được gắn với vị trí của cây**

- React duy trì State của một component miễn là nó được hiện thị ở vị trí của nó trong cây. Nếu nó bị xóa hoặc một components khác được hiển thị ở cùng một vị trí, React sẽ loại bỏ State của nó.
```javascript=
  <>
      <Counter />
      {isShow && <Counter />} 
  <>
      
- <Counter> là 2 Counter riêng biệt vì mỗi Counter hiển thị ở một vị 
trí riêng trên cây
```
showB: false
![](https://i.imgur.com/9a9jBnT.png)
showB: true
![](https://i.imgur.com/eDejDIx.png)

## **Cùng component ở cùng một vị trí bảo toàn State**

```javascript=
<>
    {isFancy ? (
        <Counter isFancy={true} /> 
      ) : (
        <Counter isFancy={false} /> 
      )}
      ...
</>

- Update App sẽ không cập nhập lại state của <Counter> vì <Counter> ở 
cùng một vị trí trên cây (vì khi isFancy thay đổi thì <Counter> vẫn 
là element đầu tiên của tree)=> theo quan điểm của React cùng một 
<Counter>(cùng vị trí: if A else B (A, B cùng vị trí))
```
:::success
-  **Cùng vị trí ở đây là trên UI Tree, không phải trên JSX (component {return (*JSX*)})**
:::
![](https://i.imgur.com/Avt20uV.png)

## **Các component khác nhau ở cùng một vị trí reset lại State**

```javascript=
 {isPaused ? (
        <p>See you later!</p> 
      ) : (
        <Counter /> 
      )}

- Khi <Counter> thay đổi thành <p> thì <Counter> bị xóa và <p> được 
thêm vào vị trí đó, khi chuyển lại thì xóa <p> và thêm một <Counter> 
mới(reset State) 
```
:::success
- Nếu bạn muốn duy trì State giữa các lần hiển thị, cấu trúc cây của bạn cần phải "khớp" từ lần hiển thị này sang lần hiển thị khác. Nếu cấu trúc khác nhau, State sẽ bị phá hủy vì React phá hủy trạng thái khi nó loại bỏ một thành phần khỏi cây.
:::
```javascript=
--> Đây là lý do k nên lồng các định nghĩa Component
- MyTextField() được lồng vào trong MyComponent()
export default function MyComponent() {
  const [counter, setCounter] = useState(0);
  function MyTextField() {
    const [text, setText] = useState('');

    return (
      <input
        value={text}
        onChange={e => setText(e.target.value)}
      />
    );       
```

## Reset State Component ở cùng một vị trí

- C1: Hiển thị component ở vị trí khác nhau
```javascript=
  {isPlayerA &&
    <Counter person="Taylor" />
  }
  {!isPlayerA &&
    <Counter person="Sarah" />
  }
```
![](https://i.imgur.com/zLqFbGQ.png)

- Cách 2: Reset state với một key
    - Sử dụng `key` để React phân biệt vị trí giữa các components
```javascript=
- Mặc định, React sử dụng thứ tự bên trong parent 
("counter[person="Taylor"]", "counter thứ 2") để phân biệt giữa các 
component nhưng `key` cho phép nói với React là đây là <Counter> cụ thể giúp nó phân biệt các <Counter>

 {isPlayerA ? (
    <Counter key="Taylor" person="Taylor" />
  ) : (
    <Counter key="Sarah" person="Sarah" />
  )}
```

# useState

- **Syntax**: const [state, setState] = useState(initialState)

:::info
- Với lần render đầu tiên, trạng thái trả về của (state) là giống với giá trị mà bạn để ở tham số đầu tiên (initialState), thay đổi state (k bằng giá trị cũ) -> re-render
- Trong những lần re-renders tiếp theo, giá trị đầu tiên trả về bởi useState sẽ luôn là state mới nhất sau khi hoàn thành các thay đổi.
- Nếu initState là một func(sử dụng tham chiếu func :not()) thì nó sẽ lấy giá trị return của func đó để làm giá trị ban đầu cho state
- useState return về một arr có 2 giá trị: state và setState, params: initState
:::
:::warning
- Gọi ở top level component, k thể gọi trong vòng lặp hoặc if
- setState chỉ cập nhập lại state ở lần render tiếp theo, nếu đọc state sau khi gọi func setState thì nó vẫn trả về giá trị cũ
- setState(param): nếu param là một func thì React hiểu rằng param là initializer function (hàm khởi tạo k có () )
- In Strict Mode, React will call your updater function twice in order to help you find accidental impurities. 
:::
- **Usage**:
    - Thêm một state vào component
    - Update một state trước đó
    - Tránh tạo lại trạng thái ban đầu
    - Lưu trữ thông tin từ các renders trước đó
- ==**Pitfall**== : gọi `state` k làm thay đổi giá trị hiện tại của code thực thi, nó chỉ hiệu quả khi useState trả về từ lần render tiếp theo
## **Update state dựa trên trạng thái trước đó:**
```javascript=
function handleClick() {
  setAge(age + 1); // setAge(42 + 1)
  setAge(age + 1); // setAge(42 + 1)
  setAge(age + 1); // setAge(42 + 1)
}
```
--> khi gọi setState function, k cập nhập lại state trong code đang chạy
-> Để cập nhập ta thực hiện
```javascript=
function handleClick() {
  setAge(a => a + 1); // setAge(42 => 43)
  setAge(a => a + 1); // setAge(43 => 44)
  setAge(a => a + 1); // setAge(44 => 45)
}
```
```javascript!
1. a => a + 1 nhận 42 là pending state và return 43 là next state.
2. a => a + 1 will receive 43 as the pending state and return 44 as the next state.
3. a => a + 1 will receive 44 as the pending state and return 45 as the next state.

=> k có update queued(hàng đợi) nên nó sẽ return 45 ở state hiện tại
```
:::info
- Thường đặt tiền tố **prevState**
:::

## **Updating objects and arrays in state**

- Trong Reactjs, `State` là read-only => replace hơn là change exist obj

```javascript!=
// 🚩 Don't mutate an object in state like this:
form.firstName = 'Taylor';

// ✅ Replace state with a new object
setForm({
  ...form,
  firstName: 'Taylor'
    
// e.g:
    const [person, setPerson] = useState({
        name: 'Niki de Saint Phalle',
        artwork: {
          title: 'Blue Nana',
          city: 'Hamburg',
          image: 'https://i.imgur.com/Sd1AgUOm.jpg',
        }
  });

// ✅ Replace state with a new object
   setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        title: e.target.value
      }
    });

```

## **Tránh tạo lại init State**

```javascript=
// 🚩 False
function TodoList() {
  const [todos, setTodos] = useState(createInitialTodos());
  // ...

- createInitialTodos() chỉ dùng cho lần hiển thị ban đầu n ta vẫn gọi cho mỗi lần 
hiển thị => lãng phí

// ✅ True
function TodoList() {
  const [todos, setTodos] = useState(createInitialTodos);
  // ...
   
// ✅ True
function TodoList() {
  const [todos, setTodos] = useState(() => createInitialTodos());
  // ...
    
- Truyền một initializer vào function (createInitialTodos), nó chỉ gọi trong quá 
trình khởi tạo (lần đầu 
tiên) k chạy lại khi render
```

## **Resetting state with a key**

- Khi state trong component thay đổi thì hàm render() sẽ trả về một tree mới khác với tree cũ (state chưa thay đổi). React sẽ tìm điểm khác biệt trên tree và update trên UI (e.g: có một li*3 thêm li0 vào trước thì nó sẽ update lại tất cả - ghi đè theo thứ tự li1 -> l0;l2->l1, ..., mặc dù chỉ có li0 là thay đổi) -> state có thể bị thay ==> dùng key để so sánh tree  (chỉ update li0)

```javascript=
App() {
    stateApp
    <Form key={stateApp}/>
        stateForm 
}

- reset state của Form khi `key` (stateApp) thay đổi
- React re-creates the Form component (and all of its children) from scratch, 
so its state gets reset.
```
## **Store value from prev render**

[https://beta.reactjs.org/apis/react/useState#storing-information-from-previous-renders](https://)

```javascript=

// App.jsx
import { useState } from 'react';
import CountLabel from './CountLabel.js';

export default function App() {
  const [count, setCount] = useState(0);
  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      <button onClick={() => setCount(count - 1)}>
        Decrement
      </button>
      <CountLabel count={count} />
    </>
  );
}


// CountLabel.jsx
import { useState } from 'react';

export default function CountLabel({ count }) {
    // useState chỉ khởi tạo cho giá trị prevCount cho lần đầu render
  const [prevCount, setPrevCount] = useState(count); 
  const [trend, setTrend] = useState(null);
  if (prevCount !== count) {
    setPrevCount(count);
    setTrend(count > prevCount ? 'increasing' : 'decreasing');
  }
  return (
    <>
      <h1>{count}</h1>
      {trend && <p>The count is {trend}</p>}
    </>
  );
}

```

# **useEffect**

## **The Life Of Effect**

- Mỗi useEffect đại diện cho một quá trình đồng bộ hóa riêng biệt

```javascript=
function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId);
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId]);
  // ...
}


function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId);
  }, [roomId]);

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    // ...
  }, [roomId]);
  // ...
}
```
:::success
- Mỗi một lần components render thì React sẽ chờ **update DOM** rồi mới chạy code bên trong useEffect(), nói cách khác, react "delay" code bên trong useEffect() cho đến khi render phản ánh ra màn hình. Nếu useEffect update state thì nó sẽ re-render lại component
::: 

:::info
1. useEffect(callback)
    - Gọi callback mỗi khi component re-render
    - Gọi callback sau khi component thêm element vào DOM (giống với chuyển callback của useEffect xuống cuối cùng)
    - ( nếu call API ở TH này thì nó sẽ tạo ra một vòng lặp vô hạn nếu có setState trg lúc call vì mỗi khi gọi thì nó sẽ xét setState một lần => re-render => call API => setState...)
2. useEffect(callback, [])
    - Gọi callback một lần mỗi khi component mounting (chạy trong lần render đầu tiên) (on mout: khi component xuất hiện)
3. useEffect(callback, [deps]) (deps === dependencies ( phụ thuộc ))
     - Callback gọi lại mỗi khi deps thay đổi ( sử dụng toán tử === để biết deps có thay đổi không)
:::

- (1,2,3) 
  :::info
    - Callback luôn được gọi sau khi component mounted (khi trigged use, khi component render ra screen)
    - **Clean up function để dọn dẹp khi component unmounting** (remove khỏi screen)
        - useEffect( () => {
            .....
            
            // Clean up
            return () => ...(delete something) 
        })
    - Clean up function luôn đc gọi trước khi callback được gọi (trừ mouted đầu tiên)
    - có thể bị memory leak khi unmount trong TH sử dụng setInterval, setTimeOut, async, listen event, subscribe, window listened
    :::
:::info
// Trình tự thực hiện **useEffect** trong TH có deps
1. Cập nhập lại state
2. Cập nhập lại DOM (mutated)
3. Render lại UI
4. Gọi cleanup nếu deps thay đổi (nếu có deps)
5. Gọi useEffect callback
:::

:::info
// Trình tự thực hiện **useLayoutEffect**: có deps
1. Cập nhập lại state
2. Cập nhập lại DOM (mutated)
3. Gọi cleanup nếu deps thay đổi (nếu có deps) (sync)
4. Gọi useEffect callback (sync)
5. Render lại UI 
==> áp dụng cho thời gian chạy có giới hạn

:::

:::info
- useEffect: Sẽ là lựa chọn đúng, giúp việc tối ưu tốc độ chạy khỏi phải chờ đợi gì thường được dùng để fetching data.
- useLayoutEffect: Nhưng nếu bạn muốn xử lý đồng bộ với UI thì hãy dùng useLayoutEffect

:::

:::info
- Hàm được gọi bởi useEffect sẽ chạy sau khi render hoàn thành
- Khi trình duyệt vẽ xog thì useEffect mới chạy. Render mới sẽ loại bỏ effect của render cũ
:::

## **Bạn không cần sử dụng useEffect**

- Không cần sử dụng useEffect trong user event: e.g GET user/buy
- Lưu các phép tính tốn kém bộ nhớ
```javascript=
// 🔴 BAD
function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');

  // 🔴 Avoid: redundant state and unnecessary Effect
  const [visibleTodos, setVisibleTodos] = useState([]);
  useEffect(() => {
    setVisibleTodos(getFilteredTodos(todos, filter));
  }, [todos, filter]);

  // ...
}

//✅ GOOD: sử dụng useMemo() hạn chế được việc tạo state
function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  const visibleTodos = useMemo(() => {
    // ✅ Does not re-run unless todos or filter change
    return getFilteredTodos(todos, filter);
  }, [todos, filter]);
  // ...
}
```
### **Reset State khi props thay đổi**
```javascript=
// 🔴 BAD
export default function ProfilePage({ userId }) {
  const [comment, setComment] = useState('');

  // 🔴 Avoid: Resetting state on prop change in an Effect
  useEffect(() => {
    setComment('');
  }, [userId]);
  // ...
}

//  ✅ GOOD
export default function ProfilePage({ userId }) {
  return (
    <Profile
      userId={userId}
      key={userId} 
    //key: khi userId thay đổi thì Profile sẽ reset tất cả 
state
    />
  );
}
```

### **Chỉnh sửa một số state khi props thay đổi**
```javascript=
// BAD: 
// vì useEffect run khi DOM update => state change => re-render again
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);

  // 🔴 Avoid: Adjusting state on prop change in an Effect
  useEffect(() => {
    setSelection(null); 
  }, [items]);
  // ...
}

// ✅ GOOD
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);

  // Better: Adjust the state while rendering
  const [prevItems, setPrevItems] = useState(items);
  if (items !== prevItems) {
    setPrevItems(items);
    setSelection(null);
  }
  // ...
}

// ✅ GOOD
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selectedId, setSelectedId] = useState(null);
  // ✅ Best: Calculate everything during rendering
  const selection = items.find(item => item.id === selectedId) ?? null;
  // ...
}
```

### **Khởi tạo ứng dụng**
```javascript=
// 🔴 Avoid: trog quá trình dev sẽ chạy hai lần có thể error (e.g xác thực )
function App() {
  // 🔴 Avoid: Effects with logic that should only ever run once
  useEffect(() => {
    loadDataFromLocalStorage();
    checkAuthToken();
  }, []);
  // ...
}

// GOOD

let didInit = false;

function App() {
  useEffect(() => {
    if (!didInit) {
      didInit = true;
      // ✅ Only runs once per app load
      loadDataFromLocalStorage();
      checkAuthToken();
    }
  }, []);
  // ...
}
```

# **useContext**

**Syntax:** `const value = useContext(SomeContext)`

:::info
- SomeContext: context được tạo với createContext
- Return: value được lấy từ SomeContext.Provider gần nhất, nếu k được truyền value thì mặc định là giá trị defautl (or undefined)
:::

:::info
- createContext: trả về một context obj
:::
## **Passing data deeply into the tree**

```javascript=
import { createContext, useContext } from 'react';

const ThemeContext = createContext(null);

export default function MyApp() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  )
}

function Form() {
  return (
    const theme = useContext(ThemeContext); //dark
    <p title="Welcome" class={theme}>Form</p>
  );
}
```

## **Updating data passed via context**

```javascript=
const ThemeContext = createContext(null);
function MyPage() {
  const [theme, setTheme] = useState('dark');
  return (
    <ThemeContext.Provider value={theme}>
      <Form />
      <Button onClick={() => {
        setTheme('light');
      }}>
        Switch to light theme
      </Button>
    </ThemeContext.Provider>
  );
}
```

## **Specifying a fallback default value**

- const ThemeContext = createContext(default value);
- Khi k có giá trị nào được truyền vào context thì nó sẽ lấy giá trị mặc định

# **useRef**

- **Syntax**: `const ref = useRef(initialValue)`

:::info
- initialValue: giá trị ban đầu gán cho ref.current
- useRef returns về một obj với attr `current`. Trong lần re-render nó sẽ trả về cùng một obj
- Nếu truyền gán ref obj trong Reactjs dưới dạng property trong một node JSX thì React sẽ cài đặt property current cho ref (khi đó ref.current đại diện cho DOM node đó)
- Thay đổi ref k làm re-render
:::
:::warning
- khi thay đổi ref.current, react k re-render component vì React k biết khi nào bạn thay đổi vì ref là một object js thuần túy
- Không get hoặc set value ref.current trong khi render, thay vào đó sử dụng event handles hoặc trong useEffect
:::
:::danger
- Nếu truyền ref vào Function components sẽ gây error
const inputRef = useRef(null);
return <MyInput ref={inputRef} />;
==> sử dụng React.forwardRef()
:::
- useRef: tham chiếu đến một giá trị k cần render
- Use:
    - tham chiếu một giá trị
    - thao tác với DOM

## **Referencing a value with a ref** 

:::info
- Không viết hoặc độc ref.current trong khi render mà thay vào đó viết nó trong useEffect() vì React mong muốn body của component là một func thuần túy
:::

```javascript=
// 🚩 Avoid
function MyComponent() {
  // ...
  // 🚩 Don't write a ref during rendering
  myRef.current = 123;
  // ...
  // 🚩 Don't read a ref during rendering
  return <h1>{myOtherRef.current}</h1>;
}

// ✅: GOOD
function MyComponent() {
  // ...
  useEffect(() => {
    // ✅ You can read or write refs in effects
    myRef.current = 123;
  });
  // ...
  function handleClick() {
    // ✅ You can read or write refs in event handlers
    doSomething(myOtherRef.current);
  }
  // ...
}
```

## **Manipulating the DOM with a ref**

```javascript=
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}

- ref trong input, input.current đại diện cho tag input
```

# **useImperativeHandle**
- Syntax: **useImperativeHandle(ref, createHandle, [deps])**

:::info
- Tùy biến luồng giá trị thông qua conponent cha
- e.g tag video có hai attr là play() và pause() nếu chỉ sử dụng ref thì nó sẽ lấy ra cả tag video(gây lộ thông tin) còn khi sử dụng useImperativeHandle thì nó sẽ tách luồng play và pause ra
::: 

```javascript=
// Có sử dụng forwardRef ở cuối Video.jsx
// App.jsx
import Video from "./Video";

export default function App() {
  const videoRef = useRef();

    useEffect(() => {
        console.log(videoRef.current) // play(), pause()
    })
  return (
    <div className="App">
      <Video ref={videoRef} />
      <button onClick={() => videoRef.current.play()}>Play</button>
      <button onClick={() => videoRef.current.pause()}>Play</button>
    </div>
  );
}


//Video.jsx
import { forwardRef, useImperativeHandle, useRef } from "react";
import video_001 from "./videos/video_001.mp4";

function Video(props, ref) {
  const videoRef = useRef();

  useImperativeHandle(ref, () => {
    // Được gán cho videoRef.current ở App.jsx
    return {
      play() {
        videoRef.current.play();
      },
      pause() {
        videoRef.current.pause();
      }
    };
  });

  return (
    <div>
      <video ref={videoRef} muted controls width={220} src={video_001} />
    </div>
  );
}

export default forwardRef(Video);

```
# **forwardRef**

:::info
- Chuyển tiếp một ref qua nhiều components
:::

```javascript=
import { useRef } from 'react';
import MyInput from './MyInput.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus(); 
    // tự động focus khi press btn Edit, 
    // ref được truyền xuống component con (MyInput)
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}

//File MyInput.jsx
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});

export default MyInput;

```

# **useReducer**

- **Syntax**: const [state, dispatch] = useReducer (reducer, initialArg, init?)

:::info
1. initState
2. actions
3. reducer
4. dispatch

- dispatch(action)
:::
## **Adding a reducer to a component**

:::info
- useReducer giống useState, nhưng chuyển logic cập nhập state từ event handler sang một func duy nhất bên ngoài component
:::

```javascript=
import { useReducer } from 'react';

function reducer(state, action) {
  // ...
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });
  // ...
    
    
- useReducer trả
```

# **useMemo**

- useMemo giúp lưu vào bộ nhớ cache kết quả giữa các lần re-render, trả về kết quả của một func (js), tránh lặp lại các logic trong một func (js)
- **Syntax**:  `const memoizedValue = useMemo(calculateValue, dependencies)`
- Usage
    - Tránh thực hiện một logic nào đó không cần thiết
    - Bỏ qua các phép tính toán lại

```javascript=
import { useMemo } from 'react';

function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}

- `calculation function`: k có đối số, như () =>, return value muốn 
tính toán
- dependencies [todos, tab]: mỗi khi re-render thì, react so sánh các 
dependenciesc có thay đổi k, nếu thay đổi thì chạy lại phép tính và trả 
về giá trị mới

- Thường áp dụng cho bài toán tính tổng giá của sản phẩm
```
# **memo**

- https://react.dev/reference/react/memo
- Tránh render lại một func component k cần thiết
- cho phép bạn bỏ qua việc re-render một thành phần khi các prop của nó không thay đổi.
- Dùng khi:
    - Trước hết thì component của bạn phải là functional component đã nhé.
    - Component của bạn thường xuyên bị re-render.
    - Nếu component của bạn luôn luôn bị re-render mặc dù prop không thay đổi.
    - Component của bạn chứa một lượng lớn tính toán logic và UI như Chart, Canvas, 3D library….

# **useCallback**

- **useCallback** thì tập trung giải quyết vấn đề về performance, khi mà các callback function được tạo ở functional component cha pass xuống component con luôn bị tạo mới, khiến cho con luôn bị re-render.
useCallback trả về một function (chính là function bạn pass vào ứng với tham số thứ nhất), callback function này sẽ được tạo lại khi một trong số các dependencies thay đổi. Nếu dependencies không đổi, function trả về sẽ là function trước đó -> tức là function pass xuống component con không bị tạo mới, tương đương không có object được tạo mới -> component con không bị re-render.

- Usage:
    - tránh tạo hàm mới một cách không cần thiết trong component, luôn đi cùng memo
    -  giữ cho một hàm không được tạo lại lần nữa, dựa trên mảng các phần phụ thuộc. Nó sẽ trả về chính function đó. Sử dụng nó khi mà bạn muốn truyền fuction vào component con và chặn không cho một hàm nào đó tiêu thời gian, tài nguyên phải tạo lại.

- useCallback là một hook trong ReactJS được sử dụng để tạo lại một phiên bản memoized của một hàm callback.

Khi bạn truyền một hàm callback vào các component con, các hàm callback này sẽ được tạo lại mỗi khi component cha render lại. Điều này có thể gây ra việc render không cần thiết và ảnh hưởng đến hiệu suất của ứng dụng.

Để tránh việc tạo lại các hàm callback mỗi khi component cha render lại, bạn có thể sử dụng useCallback. useCallback nhận vào một hàm callback và một mảng dependencies. Khi các dependencies thay đổi, useCallback sẽ tạo lại phiên bản memoized của hàm callback và trả về nó.

```reactjs=
import React, { useCallback } from 'react';

const MyComponent = () => {
  const handleClick = useCallback(() => {
    // Hành động khi button được nhấp
  }, []); // Mảng dependencies trống, hàm callback chỉ được tạo lại một lần

  return (
    <div>
      <button onClick={handleClick}>Click me</button>
    </div>
  );
};

```
# **lazy**

- cho phép trì hoãn việc tải code cho đến khi nó hiển thị lần đầu tiên
- Syntax: `const SomeComponent = lazy(load)`
- Usage:
    - Lazy-loading components with Suspense

# **Suspense**

- là một React component, nó sẽ hiển thị một fallback (dự phòng) cho đến khi component con loading xong
```javascript=
`Syntax:`
    <Suspense fallback={<Loading />}>
        <SomeComponent />
    </Suspense>
```

:::

# **Portals**

- Portals cung cấp một cách render các phần tử DOM bên ngoài phân cấp của DOM chính.
- **Syntax**: `ReactDOM.createPortal(child, container)`
    - Ngang cấp với root in file index.html
    - Trong file index.html phải chứa tag ngang cấp với root eg: div#overlay-root

[https://github.com/academind/react-complete-guide-code/tree/09-fragments-portals-refs](https://)



# Interview Reacjs
2: SetState là hàm đồng bộ hay bất đồng bộ. Sẽ như nào nếu hàm này là hàm đồng bộ. 
    => bất đồng bộ vì nó không cập nhập ngay lập tức
3: DOM ảo là gì, quá trình render, re-render được thực hiện như thế nào.
![image](https://hackmd.io/_uploads/BJ4Ri3ETa.png)

9: Trong function component(hooks) lifecycle được viết ở đâu. Có nhưng loại nào.
10: Tại sao team Reacr lại viết chung 3 lifecycle vào trong useEffect mà không tách riêng ra.
11: Giải thích ý nghĩa của từng Dependency trong useEffect.
12: Ngoài useEffect ra em còn biết về các hooks nào khác không, em đã custom 1 hooks nào chưa.
13: HOC là gì.
14: Promise, callback, async/await là gì.
15: Trình bày về lý do khi nào nên sử dụng redux.
16: Ngoài redux ra còn có cách nào để share dữ liệu không.
17: Chỉ ra 1 vài ưu điểm của state management mà em lựa chọn.
18: Em thường làm việc với các middleware nào.
19: Em biết là redux-thunk không.
20: Em biết Sass không.
21: Quy tắc đặt tên BEM.
22: Ngoài ra em có biết thêm về thư viện hỗ trợ việc css hay xây dựng UI nào không(tailwind,....)
23: Em biết TypeScript không.
24: Em có biết về thư viện hay framework nào hỗ trợ server-render-side không.
25: Theo em ReactJS là CRS hay SRS.