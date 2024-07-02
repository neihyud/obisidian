---
title: 'DOM, ES6, Promise, Async và Await'
tags: [Web]

---

###### tags: `Web`
# DOM, ES6, Promise, Async và Await

<h3>Callback</h3>

- **Callbacks** trong Js có tên đầy đủ là callbacks function . Nó là một hàm được truyền vào hàm khác dưới dạng một đối số. Hàm callback sẽ được gọi từ bên trong một hàm khác, và chúng ta gọi hàm nhận hàm callback dưới dạng đối số là hàm bậc cao hơn (higher-order function).

```javascript=
function A(){
   // code
}
 
// Hàm B có một tham số callback
function B(callback){
    callback();
}
 
// Gọi hàm B và truyền tham số là hàm A
B(A);
```

<h3>DOM Event Property</h3>

- `e.currentTarget`: trả về element đã kích hoạt nó


<h3>Dom (Document Object Model)</h3>

- The DOM is a W3C (World Wide Web Consortium) standard.
- Có 3 thành phần:
    - Element: là các thẻ html
    - Attribute: là các thuộc tính của thẻ
    - Text: chữ hiện ra màn hình
- HTML DOM: a standard object model and programming interface for HTML
    + The HTML elements as objects
    + The properties of all HTML elements
    + The methods to access all HTML elements
    + The events for all HTML elements
 - Document
      - là element lớn nhất đại điện cho cả web chứa tất cả các element khác
      - vì vậy muốn lấy các element khác đều phải đi qua document

![](https://i.imgur.com/inGPp1G.png)


<h3>Get element methods</h3>

- Element : ID, class, tag, CSS selector, HTML collection ( muốn truy cập từng phần tử thì phài dùng vòng lặp)
    + **id: `document.getElementById('...')`;**
    + **class: `document.getElementsByClassList('...')`** 
    ---> trả về HTMLCollection (giống mảng, không có các method như map(),...) chứa các phần tử có cùng class với class truyền vào 
    + **TagName: `document.getElementsByTagName('...')`**
        ---> trả về HTMLCollection - giống getElementsByClassList
    + **CSS selector: `document.querySelector('...')`** 
    ---> chỉ selector cho một phần tử, nếu có nhiều phần tử thì nó chỉ trả về selector đầu tiên
   + **CSS all`document.querySelectorAll(CSS selector)`** (có thể chọn các selector, css selector có thể ngăn cách nhau bằng dấu phẩy)
   --> trả về **NodeList** (giống mảng, không có các method như map(),...) chứa các phần tử có cùng CSS selector với CSS selector truyền vào
:::info
**Để sử dụng được mảng từ `querySelectorAll` (nodeList), getElementsByClassList(...)`(HTMLCollection) ta sử dụng** <br>
    - **Array.from(name_nodeList)** <br>
    - **Spead [...name_nodeList]**<br>
    - **Array.prototype.map.call(tiles, function...)**
:::
- document.write('...'): đặt ở đâu thì in ra ở đó, nếu ghi vào sau khi tải xog DOM thì nó sẽ ghi đè lại tất cả
- document.anchors: lấy ra tất cả thẻ `a` có attribute `name`
- document.forms: lấy ra tất cả thẻ `form`
- e.g other
>div.box-1>ul>li{Js1}^li{Php1}
 div.box-2>ul>li{Js2}^li{Php2} 
 
-->Công việc 1: sử dụng tới boxNode 
var boxNode = document.querySelector('.box-1');
...
Công việc 2: sử dụng tới các li elements là con của '.box-1'
--> boxNode.querySelectorAll('li') (Ta đứng từ boxNode để gọi tới)

<h3>Sự khác nhau giữa nodeList và HTMLCollection</h3>

- Một HTMLCollection là một tập hợp document elements, có thể được truy cập bằng tên, id hoặc số chỉ mục của chúng.
- Một nodeList là một tập hợp các document nodes (element nodes, attribute nodes, and text nodes), nó chỉ có thể truy cập bằng chỉ mục
- HTMLCollection luôn là một bộ sưu tập trực tiếp . Ví dụ: Nếu bạn thêm phần tử `<li>` vào danh sách trong DOM, danh sách trong HTMLCollection cũng sẽ thay đổi.
- NodeList thường là một tập hợp tĩnh . Ví dụ: Nếu bạn thêm phần tử `<li>` vào danh sách trong DOM, danh sách trong NodeList sẽ không thay đổi.
- The getElementsByClassName() and getElementsByTagName() methods return a live HTMLCollection.
- The querySelectorAll() method returns a **static NodeList**.
- The childNodes property returns a live NodeList.

<h3>DOM Attribute</h3>

>HTML: h1
>var headingElement = document.querySelector('h1')

- Muốn thêm attribute hợp lệ ta sử dụng:
    + id: headingElement.id = '...';
    + class: headingElement.className = '...';


- Muốn gán một attribute không nhất thiết phải hợp lệ với element, ta sử dụng method `setAttribute(keyAttribute, valueAttribute)`
- ...getAttribute('keyAttribute'): lấy ra giá trị của keyAttribute ()


<h3>InnerText & textContent Property</h3>

![](https://i.imgur.com/MJIJKZl.png)

![](https://i.imgur.com/UjuIZYE.png)

>HTML: h1.heading{Heading Text} <br>
>JS: var headingElement = document.querySelector('.heading')
>console.log(headingElement.innerText) 
>---> Result: Heading Text (innerText lấy ra nội dung text của element)
- Không thể thêm element vào một element khác
- Muốn sửa đổi text của element: sử dụng `innerText` or `textContent`
    + <strong>headingElement.innerText = 'New heading' </strong>
- Khi getter mà dùng `innerText` và `textContent`:
    + `innerText`: nội dung lấy ra giống với nội dung mà ta nhìn thấy ở trình duyệt
        + Chỉ thực hiện được trên Element Node
    + `textContent`: nó sẽ bỏ qua tagName và lấy ra nội dung bên trong thẻ đóng/mở, kể cả khoảng trắng, code (tag style), dấu các dòng, kể ..., không bị ảnh hưởng bởi các các thuộc tính của thẻ
        + Thực hiện được trên Element Node và Text Node
- Khi setter mà dùng `innerText` và `textContent` thì ngược lại
    + `innerText`: setter nội dung bao gồm khoảng trắng,...
    + `textContent`: chỉ setter text và bỏ qua khoảng trắng 

<h3>InnerHTML và OuterHTML</h3>

- Dùng để thêm một element và một element khác
- là attribute của Element Node

>HTML: div.box
>var boxElement = document.querySelector('.box');
>boxElement.innerText = 'h1{Heading}';---> không thêm được
>boxElement.innerHTML = 'h1{Heading}';---> thêm được HTML vào kể cả attribute
---> div.box>h1{Heading} <br>
--->Viết theo Emmet

- document.querySelector('h1').innerHTML ---> lấy nội dung HTML của thẻ h1
- Getter
    - `innerHTML`: chỉ lấy được nội dung bên trong khối .box
    - `outerHTML`: lấy được cả khối .box
- Setter
    - `innerHTML`: thêm vào bên trong khối
    - `outerHTML`: ghi đè cả khối

<h3>Node properties</h3>

- console.log([.box]) ---> in ra tất cả thuộc tính của div.box

<h3>DOM CSS</h3>

- DOM style
    - **Syntax**: <DOM_element>.style.<tên_thuộc_tính> = '<giá_trị_thuộc_tính>'
>var boxElement = document.querySelector('.box')<br>
>boxElement.style.width = '100px';<Br>
>boxElement.style.height = '200px';

- cú pháp viết nhiều thuộc tính gắn gọn hơn
   - **Syntaxt**: Object.assign(<DOM_element>.style, { 
      &emsp;<tên_thuộc_tính>: '<giá_trị_thuộc_tính>',
      &emsp;<tên_thuộc_tính>: '<giá_trị_thuộc_tính>',
      &emsp;...
  })
  
```javascript=
VD: 
   Object.assign(document.querySelector("div").style, {
       height: '100px',
       width: '200px',
       backgroundColor: 'red'
   })
```


<h3>DOM ClassList property</h3>

>var boxElement = document.querySelector('.box');
>console.log(boxElement.classList) (quản lý class list của element boxElement, trả về số class và tên class)

- `add('nameClass1', 'nameClass2', ...)`: thêm class vào một thẻ
    >boxElement.classList.add('red') (red là class có attribute color: red)
- `contains('nameClass...')`: kiểm tra xem một class có trong một element không
     > boxElement.classList.contains('red') ---> true
- `remove('...')` : xóa một class trong element
- `toggle('nameClass', ...)`: nếu element không có class thì nó sẽ thêm vào và sẽ xóa đi khi nó có class đó

<h3>DOM Events</h3>

- Attributes Event: Dom event(W3)
    + thêm tiền tố `on` vào attribute, ghi vào CSS Inline

    + Muốn lấy Element mà ta click vào thì sử dụng

```javascript=
C1: 
for(var i = 0; i < h1Elements.length; i++)<br> {
    h1.Element.onclick = function(event) {
        console.log(event.<strong>target</strong>)
    }
}

C2:
Dùng từ khóa `this`
VD: h1 onclick="console.log(this)">YouTube /h1 <br>
 --> khi click vào thẻ h1 thì in ra Element node của chính nó. 
    Khi click vào element con thì nó vẫn chạy, sau đó chạy tới element 
    cha(gọi là event nổi bọt)

    + h1.Element.onclick = function(event){
        console.log(event)
    }
    ---> in ra các event

```

- <strong> Method </strong>
    + `e.preventDefault()`: ngăn chặn hành vi mặc định của thuộc tính
    + `e.stopPropagation()`: ngăn chặn hành vi nổi bọt

<h3>Event Listener</h3>

- DOM events: thực hiện nhiều việc cùng một lúc trong function
  - xử lý nhiều việc trong 1 function
  - nếu gọi 2 sự kiện của một element thì sự kiện sau sẽ ghi đè sự kiện trước
  - hủy bỏ lắng nghe --> gán lại
  - sử dụng khi envent làm 1 việc đơn giản, không có nhu cầu hủy bỏ 
    >var btn = document.getElementById('btn');
    >btn.onclick = function() {
    &emsp;&emsp;Việc 1: console.log('1');
   &emsp;&emsp;Việc 2: console.log('2');
    &emsp;&emsp;Việc 3: alert('3');
} ---> Thực hiện 3 việc cùng một lúc
    > setTimeOut(functon() {
        &emsp;&emsp;btn.onclick = function(){}
    }, 3000) ---> remove event sau 3000s
- Listen Event: <strong> Syntax: element.addEventListener(event, function, useCapture)</strong>
    + add Event
    >btn.addEventListener('click', function(e) {
    &emsp;&emsp;console.log('2');
    })
        >btn.addEventListener('click', function(e) {
    &emsp;&emsp;console.log('2');
    })
        >btn.addEventListener('click', function(e) {
    &emsp;&emsp;console.log('2');
    })<br>
    ---> Có thể thêm events nhiều lần, và nó sẽ được gọi theo thứ tự thêm vào
    
    + removeEventListener(...): xóa bỏ event
    > + setTimeOut(functon() {
      &emsp;&emsp;btn.removeEventListener('click', function(...))
    }, 3000) ---> remove event sau 3000s

<h3>JSON</h3>

- Viết tắt: Javascript Object Notation
- là một định dạng dữ liệu (dạng chuỗi) (viết trên giấy hay máy tính, ... là JSON)
- Thể hiện dạng dữ liệu: Number, Boolean, Null, Array, Object, String
- **Stringify**: Từ Javascript types --> JSON
- **Parse**: Từ JSON --> Javascript types, trả về Obj
>e.g var a = 'true'(JSON)
>console.log(JSON.parse(a)) (Javascript types)

>console.log(JSON.stringify(2)) --> chuyển sang JSON
- Trong JSON, thể hiện dữ liệu kiểu bên trong dấu ' ' (vì JSON có dạng chuỗi) 
    + Array: var json = '["A","B"]' (trong JSON không cho phép dấu phẩy ở cuối..."B",])
    + Object: var json = '{"name":"Son Dang", "age": 18}' ---> key phải trong dấu " ", nếu value là string thì phải ở trong dấu " "  
    + Number: var json = '1' ---> Number
    + Boolean: var json = 'true'
    + Null: var json = 'null' 
    + String: var json = '"abc"' (phải thêm " ")

<h3>Promise</h3>

- Một **Object Promise** trong Js đại diện cho một giá trị ở thời điểm hiện tại có thể chưa tồn tại, nhưng sẽ được xử lý và có giá trị vào một thời gian nào đó trong tương lai
>Ví dụ, khi bạn oder một món đồ ở trên mạng và cần 2 ngày để ship về, có thể thấy hành động giao hàng ở đây là bất đồng bộ (cần 2 ngày mới có thể hoàn thành). Thì chủ shop đã trao cho bạn một "lời hứa"  đại diện cho món hàng đó. Sau đó, bạn vẫn thực hiện các hoạt động khác (ăn, ngủ, code) bình thường và cuối cùng sẽ nhận được món hàng sau 2 ngày và có thể sử dụng nó.
- Sử dụng trong bất đồng bộ, một promise đại diện cho một tiến trình hay một tác vụ chưa thể hoàn thành ngay được.
- Sync: Đồng bộ, chạy theo tuần tự từ trên xuống
- Async: Bất đồng bộ, không chạy theo tuần tự, các hàm chạy song song với nhau
(setTimeout, setInterval, fetch, XMLHttpRequest, file reading, request animation frame, ...)
- Khi callback hell thì ta sử dụng promise thì gọn hơn


1.Pendding: là trạng thái đang chờ khi không resolve(), reject()---> rò rỉ bộ nhớ, result là undefined
2.Fulfilled: thành công(resole()), chạy .then(...), a result value
3.Rejected: thất bại(reject()), chạy .catch(...), 	result:an error object


- Promise sinh ra để xử lý các thao tác bất đồng bộ.
- Trước khi có Promise, ngta thường dùng Callback => dễ gây ra Callback hell => Promise sinh ra từ ES6, để khắc phục callback hell
- Promise có 3 trạng thái: Pending (memory leak), Fulfilled, Rejected
- Để tạo ra 1 Promise, sử dụng từ khóa new.
- Trong constructor của Promise, truyền vào 1 executor function.
- Trong executor function nhận 2 tham số (resolve, reject). Resolve(): xử lý thành công >< Reject(): xử lý thất bại.
- Promise có 2 phương thức chính: .then và .catch (đều nhận callback).
- .then đc nhận khi resolve(), .catch được gọi khi reject().

<h3>Promise (Chain)</h3>

>var promise = new Promise (
&emsp;&emsp;function(resole, reject) {
&emsp;&emsp;&emsp;&emsp;resole();
&emsp;&emsp;}
);
promise
  &emsp;&emsp;.then(function() {
  &emsp;&emsp;&emsp;&emsp;console.log(1)
  &emsp;&emsp;})
   &emsp;&emsp;.then(function() {
  &emsp;&emsp;&emsp;&emsp;console.log(2)
  &emsp;&emsp;})
   &emsp;&emsp;.then(function() {
  &emsp;&emsp;&emsp;&emsp;console.log(3)
  &emsp;&emsp;})
---> thực thi từ trên xuống dưới, khi xg `.then` thứ nhất thì mới đến `.then` tiếp theo, `.then` thứ nhất return cái gì thì `.then` tiếp theo sẽ nhận giá trị đó, kết quả của `.then` trước là tham số đầu vào của `.then` sau
- **Nếu `.then` return new Promise(...) thì nó phải chờ `.then` đó chạy xg rồi mới trả về giá trị cho `.then` sau**
- Nếu trong quá trình chạy mà bị reject() thì nó sẽ không chạy xuống đoạn code dưới và phải dùng catch() để bắt lỗi

<h3>Promise Method</h3>

- Promise.resolve(): luôn thành công
- Promise.reject(): luôn thất bại
- Promise.all(...): nhận vào một mảng Promise, nếu các promise trong mảng là resolve() thì return một promise, nếu có ít nhất một `reject()` thì return `reject()`
>Promise.all([promise1, promise2])
    .then(function(result) {
    &emsp;&emsp;&emsp;&emsp;console.log(result);
    &emsp;&emsp;})
    ---> promist1: mất 2s để chạy xg, promise2: mất 5s để chạy xg ---> Promise.all mất 5s để  chạy xg

<h3>Async và Await</h3>

- Async
    - Khai báo một hàm bất đồng bộ
    - Tự động chuyển đổi func thông thường thành Promise
    - Async func cho phép sử dụng await
- Await
    - Await chỉ làm việc với Promise, không làm việc với Callback
    - Khi đặt trước một Promise, await sẽ đợi promise kết thúc và trả ra kết quả
    - Await chỉ có thể sử dụng khi đặt trong async func

<h3>Event loop</h3>


![](https://i.imgur.com/EX1rXbR.png)

![](https://i.imgur.com/k3DUhI9.png)



- Node.js là xử lý đơn luồng, nên nó sẽ chỉ làm một việc một lúc. 
- Thực thi theo Queue
- Call stack là một LIFO (Last In, First Out) queue, event loop liên tục kiểm tra call stack để xem có func cần thực thi hay k, và thực hiện theo thứ tự => lặp lại cho đến khi call stack rỗng
- **Event Queue** là nơi mà các sự kiện do người dùng thực hiện như click, gõ phím hay fetch API sẽ được đẩy vào một hàng đợi và chờ đến khi được lấy ra và thực thi.
- **Event Loop** sẽ liên tục tìm kiếm trong call stack những gì cần thực thi cho đến khi call stack rỗng nhưng Event Loop sẽ không dừng ở đó, nó sẽ bắt đầu đọc tiếp Event Queue để nhặt ra những gì được nhét vào đó và lôi ra để thực thi.

<h3>Fetch</h3>

- `API`: 
    - làm một url
    - cổng giao tiếp giữa các phần mềm
- Backend -> API (URL) ->Fetch -> JSON/XML ->JSON.parse -> Javascript types -> Render ra giao diện vưới HTML
- `fetch()`: là một promise và là một phương thức để lấy dữ liệu
>var postApi = '.......'; (....: link URL)
>fetch(postApi)
&emsp;&emsp;.then(function(response) {
&emsp;&emsp;&emsp;&emsp;return response.json() 
//**.json() trả về một Promise và trong đó có resolve của JSON.parse**
&emsp;&emsp;    })
 &emsp;&emsp;   .then(function(posts) {
  &emsp;&emsp;&emsp;&emsp;      console.log(posts);
  (posts là dữ liệu trả về từ `.then` trên và nó in ra nội dung của file)
    })



<h3>ECMAScript 6 (2015)</h3>

1. let, const
- var và let, const
  - Scope (phạm vi truy cập)
    + var: bên trong function(){}
    + let, const: bên trong code block {}
  - Hosting (đưa lên trên đầu)
    + var: được hỗ trợ
    + let, const: không được hỗ trợ
- const VS var, let
  - Assignment (gán lại) 
    + const: không thể gán lại lần thứ 2
    + var, let: có thể gán lại nhiều lần
>1. const logger = function(log) {
&emsp;&emsp;console.log(log);
}
>2. const sum = function(a, b) {
&emsp;&emsp;return a + b;
}

<h4>Sử dụng Arrow function</h4>

  >1. const logger = (log)=> {
  &emsp;&emsp;console.log(log);
  }
 >2. const sum = (a, b) => a + b (chỉ khi return luôn thì ms được viết ntn)
 >3. Giả sử return luôn Obj => const sum = (a, b) => ({a: a, b: b})
- cách viết gắn gọn hơn
 - **không có context, không nên dùng từ khóa `this` bên trong** 
- a. Arrow function không định nghĩa giá trị `this`.
b. Arrow function không định nghĩa giá trị arguments của riêng nó.
c. Arrow function không phù hợp làm phương thức cho object.
d. Arrow function không thể sử dụng làm hàm khởi tạo.
e. Arrow function không có thuộc tính prototype.

<h4>Default parameter values</h4>

>function logger(<strong>log = 'Gia tri mac dinh'</strong>) {
&emsp;&emsp;console.log(log)
} ---> Gán luôn giá trị khi truyền tham số

<h4>Enhanced object literals</h4>

>var name = 'Javascript'
>var price = 1000;
>var course = {
&emsp;&emsp;name: name,
&emsp;&emsp;price: price(name, price sau dấu : là biến ở trên)
};
---> Viết gọn lại: var course = {name, price};

- Có thể viết e.g trên
>const course = {
&emsp;&emsp;[name]: 'A',
&emsp;&emsp;[price]: 'B'
( ---> Javacript: 'A', 1000: 'B')
}

<h4>Destructuring</h4>

- Destructuring là cú pháp cho phép bạn **gán** thuộc tính của Obj hay giá trị của một Array
> e.g: var [a, b, c] = array[1, 2, 3] => a = 1, b = 2, c =3
- Lấy ra các phần tử từ arr, obj
>var array = ['Js', 'Php', 'Ruby']
var a = array[0]
var b = array[1]
var c = array[2]

>---> Destructuring:
>var [a, b, c] = array; (array là tên mảng)
>1. Muốn k lấy một giá trị thì ta bỏ qua k điền 1 biến vào (e.g bỏ b --> var [a, ,c] = array)
>2. Muốn lấy các biến còn lại: var [a, ...rest] = array (rest chỉ là tên biến nên có thể thay đổi tên, các giá trị còn lại sẽ lưu vào rest) ---> console.log(rest) Result: ['Php', 'Ruby'] 
>3. <strong>Đối với Obj thì tên phải trùng với key</strong>
>4. Có thể đặt tên mới cho obj, sử dụng (nameAttribute : newName) khi gán<br>
    - VD: let obj = {name: 'trần trọng nam', age: 19}<br>
      let {address = 'tuyên quang'} = obj
      
<h4>Spread</h4>

- Là dấu ba chấm (...) có thể chuyển đổi một mảng thành một chuỗi ngăn cách nhau bằng dấu phẩy (có thể hiểu nó dùng để chia nhỏ các phần tử)
- khi để ... trước arr hoặc obj sẽ bỏ đi [], {}
>var array1 = ['Js', 'Ruby', 'Php'];
>var array2 = ['NodeJs', 'Java'];
>var array3 = [...array1, ...array2] (tạo thành một mảng chứa array1 và array2)
>---> Giống kiểu nối chuỗi với dấu ...

- **Nối arr**<br>
>  let arr1 = [1, 2, 3]
  let arr2 = [4, 5, 6]
  let arr3 = [...arr1, ...arr2]
  console.log(arr3) --> [1, 2, 3, 4, 5, 6]

- **Hợp nhất obj** <br>
 > let obj1 = {name: 'tran trong nam'}<br>
  let obj2 = {age: 19}<br>
  let obj3 = {...obj1, ...obj2}<br>
  console.log(obj3) --> {name: 'tran trong nam', age: 19}

- **Truyền đối số**<br>
 > let arr = [1, 2, 3]<br>
  function tinhTong(a, b, c) {<br>
    return a + b + c<br>
  }
  
  
- **Sao chép**
>  const person1 = {
    name: 'Son',
    age: 21
  } <br>
  const person2 = {...person1}<br>
  console.log(person2) --> {name: 'Son', age: 21}

<h4>Rest Parameters</h4>

- là tạo array từ một số lượng chưa xác định
> function restTest(...args) {
  console.log(args)
}
restTest(1, 2, 3, 4, 5, 6);// [1, 2, 3, 4, 5, 6]

<h4>Tagged template literals</h4>

>function highlight(...rest) {
&emsp;&emsp;console.log(rest);
}
var brand = 'F8';
var course = 'Javascript!';
hightlight \`Hoc lap trinh ${course} tại ${brand}`

- Tham số đầu vào của `hightlight` là một mảng gồm 3 phần tử: <br>
    - phần tử 1 là mảng chứa phần không nội suy(k có dấu ${}), 
    - từ phần tử thứ 2 là các thành phần nội suy<br>
:::warning
hightlight ("Học lập trình ${course} tại ${brand}!") <=>  hightlight(['Học lập trình', 'tại', 'thật dễ hiểu'], 'Javascript', 'F8')

-    (3) [Array(3), 'JavaScript', 'F8']<br>
      0: ['Học lập trình ', ' tại ', 'thật dễ hiểu']<br>
      1: "JavaScript"<br>
      2: "F8"
:::

<h4> Optional chaining (?.)</h4>

>const adventurer = {
    name: 'Alice',
    cat: {
      &emsp;name: 'Dinah'
    }
  };

  console.log(adventurer.cat.name) --> Dinah
  console.log(adventurer.catttt.name) --> lỗi chương trình
  console.log(adventurer.catttt?.name) --> undefined

  - kiểm tra nếu có cat thì mới .name, không có trả về undefined

<h4>Yield Keyword</h4>

![](https://i.imgur.com/FpSExDF.png)

- interator = foo(0) => .next() chạy hết dòng Yield r dùng lại => .next(), chạy tiếp  dòng `index++` r quay lại vòng while
- luôn trả về { value: ?, done: ?} 
<h3>IIFE - Immediately Invoked Function Expression</h3>

- IIFE: Biểu thức hàm được gọi ngay lập tức
- Syntax:
>  (function() {
     &emsp; --code--
    })()
    
>(function() {
&emsp;&emsp;console.log('NOW NOW')
})() <br>--> Gọi ngay lập tức --> IIFE
- Các cách viết IIFE
    - khi viết toán tử phía trước IIFE thì không cần dấu ; phía trước
  ><toán_tử>(
    function(){<br>
      &emsp;--code--<br>
  })()<br>
  --> toán tử có thể là: ! + - * / ,...
   -  Cách viết IIFE thường được dùng: dùng dấu ; trước IIFE
  >;( 
    function(){
      &emsp;--code--
    } 
  )()
- IIFE là một hàm 'private'
- Sử dụng khi nào:
    - tránh tạo ra quá nhiều biến toàn cục vì trong dự án lớn sẽ dễ đặt trung tên, ghi đè nhau, tạo ra nhiều lỗi không mong muốn
  - các thư viện dùng IIFE để khi nhúng vào, tên hàm, tên biến không ảnh hưởng đến dự án
  - sử dụng khi muốn code chạy ngay, nhưng biến và hàm không ở phạm vị toàn cục

<h3>Scope</h3>

- Global - Toàn cầu
- Code block - Khối mã: let, const
- Local scope - Hàm: var, function --> Phạm vi hàm, không truy cập được bên ngoài
- Khi gọi mỗi hàm luôn có một phạm vi mới được tạo
- Các hàm có thể truy cập các biến được khai báo trong phạm vi của nó và bên ngoài nó 
- Khi nào biến bị xóa khỏi bộ nhớ
    + biến toàn cầu: khi F5, khi exit --> hạn chế sử dụng vì lãng phí bộ nhớ
    + Biến trong code block và trong hàm
    + 
-  VD:
>lỗi, tìm thấy biến gần nhất rồi, những biến đó đc gọi trước khi tạo 
 {
  &emsp;&emsp;  const num = 1
    &emsp;&emsp; {
     &emsp;&emsp;&emsp;&emsp;   {
      &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;      {
    &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;            console.log(num)
     &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;           const num = 2
   &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;         }
  &emsp;&emsp;&emsp;&emsp;      }
  &emsp;&emsp;  }
  }
  ---> Trong TH này console.log(num) sẽ lấy num trong cùng một khối nhưng do const k hoisting nên nó sẽ hiển thị lỗi
  
 - Khi biến bị xóa khỏi bộ nhớ 
    + Global (toàn cục)
         + khi thoát chương trình
    + Code block (khối mã), Local scope (hàm)
      + khi chạy xong 

   + **biến ở trong hàm được tham chiếu bởi hàm khác**
  >function makeCouter() {
      &emsp;&emsp; let couter = 0
      &emsp;&emsp; function increase() {
    &emsp;&emsp; &emsp;&emsp;       return ++couter
     &emsp;&emsp;  }
   &emsp;&emsp;    return increase
  }
  const increase1 = makeCouter()
  console.log(increase1())   //1
  console.log(increase1())   //2
  console.log(increase1())   //3
--> **biến couter chưa được xóa vì:**
**increase1 là biến toàn cục** và increase1 <=> increase mà:  increase không trực tiếp tạo ra biến couter, đang sử dụng biến couter ở phạm vi bên ngoài (trong makeCouter) nên:biến ở bên trong makeCouter vẫn được sử dụng ở một hàm khác đã được ở bên ngoài 

<h3>Closure</h3>

- hàm khai báo trong hàm
- Closure có thể tự động nhớ outer variables và có thể truy cập chúng.
- là một hàm có thể ghi nhớ nơi nó được tạo 
- truy cập được biến ở bên ngoài phạm vi của nó, 
- biến được tham chiếu (refer) trong closure sẽ không được xóa khỏi bộ nhớ khi hàm cha được thực thi xong
- Ứng dụng
    + Viết code ngắn gọn hơn
    + Biểu diễn, ứng dụng tính Private trong OOP
- Biểu thị được tính private trong OOP
- Biến được tham chiếu (refer) trong closure sẽ không được xóa khỏi bộ nhớ khi hàm cha thực thi xg, bởi vì khi gọi const A = B() thì biến trong function C ( chứa B() ) sẽ là global vì A nó là một môi trường mới
>e.g: 
	function createCounter() {
		let counter = 0
		function increase() {
			return ++counter
		}	
		return increase	
	}
	const counter1 = createCounter()  (**trả về function increase() bên trong function counter()**, **và biến counter - ngoài phạm vi, có thể truy cập được vì nó ghi nhớ nơi được tạo)**
	(**counter1 tạo ra một môi trường chứa biến counter và function increase())**
	console.log(counter1()) --> 1
	console.log(counter1()) --> 2 
	console.log(counter1()) --> 3
	const counter2 = createCounter() ---> **vì môi trường mới được tạo ra, nên gọi lại hàm counter, khởi tạo lại giá trị counter**)
	console.log(counter2()) --> 1
	console.log(counter1()) --> 2
	console.log(counter1()) --> 3


<h3>Hoisting</h3>

- là hành vi chuyển tất cả các khai báo lên đầu scope hoặc hàm, ngay cả **đứng sau return thì nó vẫn đưa lên đầu**
- `var` được chuyển lên đầu scope nhưng không được khởi tạo và nó có giá trị là `undefined`
- `let` và `const` chuyển lên đầu scope nhưng không được khởi tạo và được đưa vào **Temporal Deal Zone**
--> khối sẽ nhận biết được nó nhưng không thể sử dụng cho đến khi nó được khai báo ---> in ra lỗi: không thể truy cập
- Trong trường hợp là expression function thì hiểu đơn giản funciton ở đây là giá trị của biến thôi. 
Nó chỉ hoisted phần khai báo thôi, và tùy theo từ khóa sử dụng là gì (var, let, const). 
Tóm lại là với expression sẽ không hoisted phần hàm, chỉ hoisted phần khai báo biến như cách tạo ra 1 biến bình thường.

<h3>Từ khóa this</h3>

- Đề cập đến đối tượng mà nó thuộc về
- `this` trả về đối tượng gần nhất chứa nó
- `this` đứng một mình k thuộc func hay method thì nó trả về **window obj**
- `this` trong method trả về obj chứa method đó
- `this` trong function không có `strict mode` trả về **window obj**
- `this` trong function có `strict mode` trả về **undefined**
- Trong một  nó trả về **element** mà nó tác động
- **Arrow Function** k có giá trị `this`
- Đặc tính:
    + Trong một phương thức, `this` tham chiếu tới đối tượng truy cập phương thức (đối tượng trước dấu .)
    + Đứng ngoài phương thức, `this` tham chiếu đến đối tượng global
- `this` trong hàm tạo là đại diện cho đối tượng sẽ được tạo
- Các phương thức `bind()`, `call()`, `apply()` có thể tham chiếu `this` đến đối tượng khác

<h3>Fn.bind()</h3>

>this.firstName = 'Minh';
this.lastName = 'Thu';
const teacher = {
    &emsp;&emsp;firstName: 'Minh',
    &emsp;&emsp;lastName: 'Thao?',
    &emsp;&emsp;getFullName() {
    &emsp;&emsp;&emsp;&emsp;    return `${this.firstName} ${this.lastName}`;
  &emsp;&emsp;  }
}
*Case 1
console.log(teacher.getFullName()); (Trả về Minh Thảo)
*Case 2
const getTeacherName = teacher.getFullName;(-->Gán tham chiếu hay địa chỉ vì không có dấu () )
console.log(getTeacherName()); (Trả về Minh Thu vì `this` ở đây là window)

>Muốn Case2 trả về Minh Thảo thì ta phải ràng buộc từ khóa `this` nó với Obj, sử dụng bind(obj)
>---> const getTeacherName = teacher.getFullName.bind(teacher) ( Trả về Minh Thảo)

- Phương thức `bind()` trả về một hàm mới
- Phương thức bind() cho phép ràng buộc `this` cho một phương thức(function)
- Phương thức `bind()` sẽ trả về một hàm mới với context được bind
- Hàm được trả về từ `bind()` vẫn có thể nhận các đối số từ hàm gốc
- Nếu bind() đính kèm `arg1, arg2, ...` thì các đối số được ưu tiên hơn
- Không thực hiện gọi hàm

<h3>Fn.call()</h3>

- là phương thức trong prototype của Function constructor, phương thức này được dùng để gọi hàm và cũng có thể `bind` `this` cho hàm
- Fn.call() giúp gọi hàm và `bind` `this` tới đối tượng khác, mặc định this là window object
-  Fn.call() không trả ra hàm mới, nó gọi luôn hàm sau khi bind this (Fn.bind() thì chỉ bind this nhưng không gọi hàm)
-  Fn.call() dùng để mượn hàm - function borrowing
-  Fn.call() có thể dùng để kế thừa properties & method từ 1 Constructor khác

>const teacher = {
&emsp;&emsp; firstName: 'Minh'
&emsp;&emsp; lastName: 'Thu' 
}
const me = {
&emsp;&emsp; firstName: 'Son'
&emsp;&emsp; lastName: 'Dang'
&emsp;&emsp;showFullName() {
&emsp;&emsp;&emsp;&emsp;console.log(/`${this.firstName} ${this.lastName}`)
&emsp;&emsp;}
}
me.showFullName.call(teacher)-->Minh Thu (mượn biến từ obj teacher )


>// Ví dụ kế thừa
function Animal(name, weight) {
&emsp;&emsp;this.name = name
&emsp;&emsp;this.weight = weight
}
function Parrot() {
&emsp;&emsp;Animal.call(this, ...arguments) // Toán tử spread trải các elements của args vào làm đối số hàm Animal
    this.speak = function() {
        console.log('Go go...')
    }
}
const parrot = new Parrot('Mèo', 100)
console.log(parrot)

- Khi sử dụng toán tử arguments thì nó trả về một đối tượng arguments
>function logger {
1.&emsp;&emsp;console.log(arguments)
}
logger(1,2,3,4,5) --> Obj(Arguments): 1,2,3,4,5
---> Để sử dụng các các method reduce(), forEach(),... như array thì ta làm như sau
> 1-->Array.prototype.forEach.call(arguments, item => {
&emsp;&emsp;console.log(item)
})
---> Khi ta sử dụng cars.forEach(car) thì khi callback nó sẽ dùng toán tử this để gọi lại cars, đối chiếu với hàm trên thì this trỏ tới arguments

>C2: convert sang mảng
>const arr = Array.prototype.slice.call(arguments)

>Nên dùng để convert sang mảng: 
><strong>const arr = Arr.from(arguments</strong>
><strong>Or sử dụng EE6: const arr = [...argument]</strong>

<h3>Apply Method</h3>

- Phương thức này cho phép gọi một hàm với một `this`(bind) và truyền đối số cho hàm gốc dưới dạng mảng


<h3>Cookies</h3>

- xss lỗ hổng ở client
- k lưu ra file chống đánh cắp ở client, vì nếu set exprire and max age của cookies thì nó sẽ lưu ra file
- cách đọc cookie từ client để xem lịch sử của ng dùng ??
- client đến có một bộ nhớ riêng để lưu state, về trả về một session cho client ( save cookies trong server