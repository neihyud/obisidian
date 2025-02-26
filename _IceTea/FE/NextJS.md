- [rendering on web](https://web.dev/articles/rendering-on-the-web)
![[Pasted image 20250110145316.png]]
- [z-index](https://web.dev/learn/css/z-index)
# Terms
### Code Splitting
- split app -> smaller bundles -> downloaded and executed -> decrease amount of data transfered
### Prefetching
- Preload route in the backgroud before user visits 
- Sử dụng <Link /> hoặc **`router.prefetch()`** 
### Cache
- has an **in-memory client-side cache** called the [Router Cache](https://nextjs.org/docs/app/building-your-application/caching#router-cache).
### Partial Rendering
- [sourse](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating)
- only router segments change on navigation re-render on the client , and any shared segments are preserved.
## Overview
- Để sử dụng hook, sử dụng "use client"
	- - `"use client"` - This is a Client Component, which means you can use event listeners and hooks.
- Default: sử dụng **React Server Component**
- `server client`: được fetch và render ở server -> không tồn tại trong **client-side bundle**
	- 
- `use client`: thêm tương tác (onclick, input) => các child component được sử dụng ở compnent import `use client`
	- khi được đánh dấu `use client` => F12 thấy file ở webpack bundle
	- sử dụng khi sử dụng hook, hoặc có tương tác
## File Convenient
![[file_convenient.png]]
- **layout**: not render when switch page 
- **template**: render when switch page
- not-found: được gọi khi `notFound()` của NextJS được gọi
## Component Hierarchy
![[Pasted image 20240730105753.png]]
![[Pasted image 20240730105802.png]]
![[Structure_Layout.png]]

## Layout
- A layout is UI that is **shared** between multiple routes
![[Pasted image 20240730112428.png]]
```javascript
export default function DashboardLayout({
  children, // will be a page or nested layout
}) {
  return (
    <section>
      {/* Include shared UI here e.g. a header or sidebar */}
      <nav></nav>
 
      {children}
    </section>
  )
}
```
- When a `layout.js` and `page.js` file are defined in the same folder, the layout will wrap the page.
	- [[Structure_Layout.png]]
![[Parallel Routes.png]]
[Parallel Routes](https://nextjs.org/docs/app/building-your-application/routing/parallel-routes#slots)
### Root Layout
- layout.tsx - **Required**
- must contain `html` and `body` tags,
- Any UI add in rootlayout -> share UI across **multiple pages**
```javascript
export default function RootLayout({children}) {
  return (
    <html lang="en">
      <body>
        {/* Layout UI */}
        <main>{children}</main>
      </body>
    </html>
  )
}
```
## Handle Error
[handle - error](https://nextjs.org/docs/app/building-your-application/routing/error-handling#recovering-from-errors)
### Handle Error in Layout
- Xử lý error trong segment -> file **error.tsx**
```javascript
'use client'

export default function Error({error,reset,}: {
	error: Error & { digest?: string }
	reset: () => void
}) {
return (
		<div>
			<h2>Something went wrong!</h2>
			<button onClick={() => reset()}>Try again</button>
		</div>
	)
}
```
### Handle error in Root Layout or Template
- tạo file **global-error.tsx**
- tg tự như root **layout**
## Rendering
![[Pasted image 20240730151212.png]]
## Cache
![[Pasted image 20240730151603.png]]
## Css Style
- [css - styling](https://nextjs.org/learn/dashboard-app/css-styling)
- Global style: `global.css` -> import vào file `app/layout.tsx`
- Css Module: tạo một css duy nhất cho component
- **Clsx**: toggle class name 
## Optimizing Fonts and Images 
### Font
- **Tự động tối ưu font** -> down font at build time
- **Add a primary font**: 
	- create new file `fonts.ts` in `/app/ui`
```javascript=
import { Inter } from 'next/font/google';
 
export const inter = Inter({ subsets: ['latin'] });
```

Áp font vào body trong file **layout.tsx** ( - ***root layout - layout required*** - )
```html=
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({children}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

	- antialiased: làm mịn font
### Optimizing Image
[optomizing - image](https://nextjs.org/docs/app/building-your-application/optimizing/images)
- Từ động tối ưu image: sử sụng **<Image />**
## Opitmizing Navigation
- Sử dụng <**Link** /> -> avoid reload page when navigation ( sử dụng prefetching)
	- Next.js automatically code splits your app by **router segments**
		- pages become isolated -> if 1 page error, other page still work
- Sử dụng `usePathname` -> check current path
## Static and Dynamic Rendering
- **Static Rendering**: data fetching and rendering happens on the server at build time
	- useful for UI with **no data** or **data that is shared across users**
- Dynamic Rendering: content render on the server for each user at **request time** (when the user visits the page)
## Streaming
- is a data transfer technique -> break down a route in to smaller "chunks" 
### Streaming a whole page with `loading.tsx`
- Create file `loading.tsx` trong folder page (ex: /dashboard) ->  it allows you to create fallback UI to show as a replacement while page content loads.
	- Nếu chỉ muốn áp dụng loading cho dashboard page thì ta nhóm folder
		![[Pasted image 20240730094343.png]]
		=> `/dashboard/(overview)/page.tsx` -> `/dashboard`
### Streaming component
- Sử dụng React Suspense
	- cho phép defer rendering parts của app cho đến khi condition thoảm ãn
- In general, what is considered good practice when working with Suspense and data fetching? 
	=> Move data fetches down to the components that need it

## Add Search And Pagination
- Update URL với search params
```javascript=
'use client';
 
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline';
import { useSearchParams, usePathname, useRouter } from 'next/navigation';
 
export default function Search() {
  const searchParams = useSearchParams();
  const pathname = usePathname();
  const { replace } = useRouter();
 
  function handleSearch(term: string) {
    const params = new URLSearchParams(searchParams);
    if (term) {
      params.set('query', term);
    } else {
      params.delete('query');
    }
    replace(`${pathname}?${params.toString()}`);
  }
}
```

# Code Spliting with dynamic import
https://web.dev/articles/code-splitting-with-dynamic-imports-in-nextjs#route-based-and-component-based-code-splitting

# Rendering
## (Client-Side Rendering) CSR

![[csr.png]]
![[csr2.png]]

##  (Server-Side Rendering) SSR
https://github.com/reactwg/react-18/discussions/37
![[ssr.png]]![[ssr_.png]]
- khi JS chưa được tải về page chưa thể tương tác được
SSR in React always happens in several steps:
- On the server, fetch data for the entire app.
- Then, on the server, render the entire app to HTML and send it in the response.
- Then, on the client, load the JavaScript code for the entire app.
- Then, on the client, connect the JavaScript logic to the server-generated HTML for the entire app (this is “hydration”).
The key part is that **each step had to finish for the entire app at once before the next step could start.** This is not efficient if some parts of your app are slower than others, as is the case in pretty much every non-trivial app.

React 18 lets you use `<Suspense>` to **break down your app into smaller independent units** which will go through these steps independently from each other and won’t block the rest of the app. As a result, your app’s users will see the content sooner and be able to start interacting with it much faster. The slowest part of your app won’t drag down the parts that are fast. These improvements are automatic, and you don’t need to write any special coordination code for them to work.

When both React and your application code loads, you want to make this HTML interactive. You tell React: “Here’s the `App` component that generated this HTML on the server. Attach event handlers to that HTML!” React will render your component tree in memory, but instead of generating DOM nodes for it, it will attach all the logic to the existing HTML.

**This process of rendering your components and attaching event handlers is known as “hydration”.** It’s like watering the “dry” HTML with the “water” of interactivity and event handlers. (Or at least, that’s how I explain this term to myself.)


## Suspense
- Sử dụng `Suspense` để ssr không cần chờ tạo hết HTML để gửi về cho Client, mà sẽ gửi về sau khi nó được tạo
- Để hydrating được thì nó phải chờ toàn bộ code được tải về -> không mong muốn 
=> sử dụng lazy
### Hydrating the page before all the code has loaded

We can send the initial HTML earlier, but we still have a problem. Until the JavaScript code for the comments widget loads, we can’t start hydrating our app on the client. If the code size is large, this can take a while.

To avoid large bundles, you would usually use "code splitting": you would specify that a piece of code doesn't need to load synchronously, and your bundler will split it off into a separate `<script>` tag.

You can use code splitting with `React.lazy` to split off the comments code from the main bundle:


---

# CSR
### **Client-Side Rendering (CSR) – Render phía client**

File `bundle.js` này mới là phần quan trọng, chứa toàn bộ thư viện React, code ứng dụng của bạn và mọi thứ cần thiết để ứng dụng chạy. Trình duyệt sẽ tải xuống file này ngay sau khi xử lý HTML.

Khi JavaScript được tải xuống và thực thi, nó sẽ tạo HTML ngay trên máy tính của bạn và chèn nó vào DOM bên trong thẻ `<div id="root">`. Đó là lúc giao diện người dùng của bạn cuối cùng mới xuất hiện.

Bạn có thể kiểm tra điều này bằng cách mở **trình kiểm tra phần tử (DOM Inspector)**. Bạn sẽ thấy tiêu đề `<header>`, hình ảnh `<img>`, thẻ `<h1>` và đoạn văn `<p>`. Nhưng nếu bạn kiểm tra **"View Page Source"**, bạn sẽ chỉ thấy HTML ban đầu mà server gửi về – một trang trống rỗng!

Toàn bộ phương pháp này, nơi trình duyệt (client) chịu trách nhiệm biến các component React thành giao diện trên màn hình, được gọi là **Client-Side Rendering (CSR)**. CSR đã trở nên cực kỳ phổ biến cho các ứng dụng single-page, nhưng nó cũng có những nhược điểm đáng kể.
#### **Hai vấn đề chính của CSR**
1. **SEO kém:**
    - Công cụ tìm kiếm chủ yếu dựa vào HTML để index nội dung trang web. Nhưng với CSR, HTML ban đầu chỉ là một thẻ `<div>` trống, khiến công cụ tìm kiếm khó nhận biết nội dung trang.
    - Nếu trang web có quá nhiều component lồng nhau gọi API, nội dung quan trọng có thể tải quá chậm, làm giảm khả năng SEO.
2. **Hiệu suất & Trải nghiệm người dùng:**
    - Trình duyệt phải làm tất cả mọi thứ: lấy dữ liệu, dựng UI, thêm tương tác… Điều này khiến người dùng phải nhìn màn hình trống hoặc **loading spinner** trong thời gian dài.
    - Mỗi khi bạn thêm tính năng mới, kích thước file JavaScript (`bundle.js`) tăng lên, khiến thời gian tải trang lâu hơn, đặc biệt là với kết nối mạng chậm.

Mặc dù CSR mang lại một cuộc cách mạng cho web, các nhà phát triển nhanh chóng nhận ra rằng cần có giải pháp tốt hơn để tối ưu **SEO** và **hiệu suất**.

### **Server-Side Rendering (SSR) – Render phía server**

Để khắc phục những hạn chế của CSR, các framework React hiện đại như **Next.js** và **Gatsby** chuyển sang giải pháp render phía server.

Khi có yêu cầu từ client, thay vì gửi một file HTML rỗng như CSR, **server sẽ render toàn bộ trang HTML** rồi gửi về trình duyệt.

💡 **Lợi ích của SSR:**  
✔ **Tối ưu SEO** → Nội dung được render sẵn trên server, giúp công cụ tìm kiếm dễ index hơn.  
✔ **Trải nghiệm tốt hơn** → Người dùng thấy nội dung ngay lập tức thay vì đợi JavaScript tải và chạy.

### **Nhược điểm của SSR?**

1. **Tương tác bị trì hoãn**
    - Mặc dù HTML được tải nhanh hơn, trang vẫn chưa thể **tương tác** được ngay lập tức.
    - Người dùng phải đợi **toàn bộ JavaScript bundle** tải xong thì React mới có thể "kích hoạt" trang.
    - Quá trình này gọi là **hydration** – nơi React "gắn kết" logic JavaScript vào HTML tĩnh đã được render trên server.
### **Hydration trong React**

Hydration là quá trình React **tái tạo lại cây component (component tree) trong bộ nhớ** dựa trên HTML server-rendered, sau đó **gắn kết các sự kiện và logic tương tác**.

💡 **Tóm tắt quá trình SSR + Hydration:**

1. **Server render HTML** và gửi đến trình duyệt.
2. **Trình duyệt hiển thị HTML tĩnh** ngay lập tức.
3. **React tải JavaScript bundle** để "kích hoạt" trang web.
4. **Hydration diễn ra**, giúp trang trở thành **tương tác**.

**Vấn đề với hydration:**

- React phải **hydrate toàn bộ trang một lần** → không thể tương tác với bất kỳ phần nào trước khi hydration hoàn thành.
- Toàn bộ JavaScript phải được tải trước khi React có thể bắt đầu hydration.
- Khi có quá nhiều component, quá trình hydration trở nên **chậm và không hiệu quả**.
### **Static Site Generation (SSG) & SSR**

Có hai phương pháp chính trong server-side rendering:

1. **SSG (Static Site Generation)**
    
    - Trang web được render **trong quá trình build**, sau đó lưu trữ trên CDN.
    - **Lý tưởng cho nội dung tĩnh**, như blog, trang giới thiệu sản phẩm…
    - Load cực nhanh vì không cần chờ server render.
2. **SSR (Server-Side Rendering)**
    
    - Trang web được render **khi có request từ user**.
    - **Lý tưởng cho nội dung động**, như dashboard, feed mạng xã hội…
    - Chậm hơn SSG vì phải fetch dữ liệu và render **mỗi lần có request**.

🔹 **Cả hai phương pháp trên đều được gọi chung là SSR, nhưng thực tế chúng khác nhau!**

### **React 18 và Suspense SSR – Cách cải tiến SSR hiện đại**

React 18 giới thiệu **Suspense SSR**, giúp giải quyết những vấn đề lớn của SSR truyền thống:  
✔ **HTML Streaming** → Render từng phần HTML dần dần thay vì đợi tất cả dữ liệu tải xong.  
✔ **Selective Hydration** → Hydrate từng phần trang khi sẵn sàng, thay vì toàn bộ trang một lúc.

💡 **Suspense giúp gì?**

- **Streaming HTML**: React có thể gửi từng phần của trang khi dữ liệu sẵn sàng.
- **Hydration chọn lọc**: React có thể hydrate từng phần trang độc lập, không cần đợi toàn bộ JavaScript tải xong.

Ví dụ:

- Nội dung chính (`<MainContent />`) có thể hiển thị sớm trong khi dữ liệu API vẫn đang tải.
- Người dùng có thể **tương tác với phần đã hydrate**, trong khi các phần khác tiếp tục tải nền.

🚀 **Kết quả:**

- Không cần fetch **toàn bộ dữ liệu** trước khi render.
- Không cần tải **toàn bộ JavaScript** trước khi bắt đầu hydration.
- **Trải nghiệm nhanh hơn, mượt hơn** cho người dùng.

**Tải xuống mã JavaScript theo từng phần** là một cải tiến lớn vì nó có nghĩa là một đoạn mã JavaScript nặng sẽ không làm chậm phần còn lại của trang khi trở nên tương tác. Nhưng mọi thứ còn tốt hơn nữa!

**Hydration có chọn lọc** cũng giải quyết vấn đề thứ ba của chúng ta: yêu cầu phải hoàn tất toàn bộ quá trình hydration trước khi có thể tương tác với bất kỳ phần nào. React bắt đầu quá trình hydration ngay khi có thể, điều đó có nghĩa là người dùng có thể tương tác với các thành phần như **header** và **thanh điều hướng bên** mà không cần chờ nội dung chính tải xong. Và điều tuyệt vời nhất là **React xử lý tất cả điều này một cách tự động**.

Ngoài ra, React có một cơ chế rất thông minh để xử lý quá trình hydration trong trường hợp có nhiều thành phần đang chờ được hydrate. **React ưu tiên hydration dựa trên hành động của người dùng**. Ví dụ, nếu React sắp hydrate thanh điều hướng bên, nhưng người dùng lại bấm vào nội dung chính, React **sẽ ngay lập tức chuyển hướng** và ưu tiên hydrate thành phần mà người dùng đã tương tác trước, trong giai đoạn bắt sự kiện nhấp chuột. Điều này giúp đảm bảo **phản hồi nhanh chóng**, còn thanh điều hướng bên sẽ được hydrate sau.

Vậy là, **kiến trúc Suspense SSR mới của React đã giải quyết hiệu quả ba hạn chế chính của SSR truyền thống**. Nhưng chúng ta vẫn chưa hoàn toàn xong!

Mặc dù SSR đã có những cải tiến lớn, vẫn còn một số thách thức cần được xem xét:

1. **Kích thước mã JavaScript tải xuống**: Mặc dù chúng ta có thể truyền mã JavaScript theo từng phần, nhưng cuối cùng người dùng vẫn phải **tải toàn bộ mã của trang web**. Khi chúng ta tiếp tục thêm tính năng vào ứng dụng, mã này ngày càng phình to. **Câu hỏi đặt ra:** **Người dùng có thực sự cần tải xuống tất cả dữ liệu này không?**
    
2. **Hydration không cần thiết**: Hiện tại, **tất cả các thành phần React đều được hydrate trên client**, ngay cả khi chúng không cần đến tính năng tương tác. Điều này **lãng phí tài nguyên và làm chậm tốc độ tải trang**, ảnh hưởng đến **thời gian tương tác (TTI - Time to Interactive)**. **Câu hỏi đặt ra:** **Chúng ta có nên hydrate tất cả các thành phần không, ngay cả khi chúng chỉ là nội dung tĩnh?**
    
3. **Gánh nặng xử lý trên thiết bị người dùng**: **Mặc dù máy chủ có khả năng xử lý mạnh mẽ hơn, chúng ta vẫn đang đẩy phần lớn công việc xử lý JavaScript sang thiết bị của người dùng**. Điều này có thể làm chậm hiệu suất, đặc biệt là trên **các thiết bị yếu**. **Câu hỏi đặt ra:** **Chúng ta có nên tận dụng sức mạnh của máy chủ nhiều hơn không?**
    

Những thách thức này chỉ ra một vấn đề lớn hơn: **Chúng ta cần những cách thông minh hơn để xây dựng các ứng dụng nhanh hơn, vượt xa các phương pháp render truyền thống.**

**Giải pháp là gì?** Đó chính là điều chúng ta sẽ khám phá tiếp theo. 🚀


**Suspense với SSR** đã giúp chúng ta tiến gần hơn đến trải nghiệm render mượt mà, nhưng vẫn gặp phải một số vấn đề như:

- **Kích thước bundle lớn**, dẫn đến việc tải xuống quá nhiều dữ liệu cho người dùng.
- **Hydration không cần thiết**, gây trì hoãn sự tương tác.
- **Xử lý quá nặng phía client**, làm giảm hiệu suất tổng thể.
### React Server Components
![[rsc.png]]
#### **Client Components là gì?**

**Client Components** là các component React quen thuộc mà chúng ta đã sử dụng trong các kỹ thuật render trước đây. Chúng thường được render **phía client**, nhưng cũng có thể được **render thành HTML một lần trên server**, giúp người dùng thấy nội dung trang ngay lập tức thay vì một màn hình trống.

Mặc dù nghe có vẻ khó hiểu khi nói **"Client Components có thể render trên server"**, nhưng hãy hiểu điều này như một **chiến lược tối ưu hóa**. Chúng **chủ yếu hoạt động trên client**, nhưng vẫn có thể (và nên) **chạy một lần trên server** để tăng hiệu suất.

**Đặc điểm của Client Components:**

- **Có quyền truy cập đầy đủ vào môi trường client** (trình duyệt), cho phép sử dụng **state, effect và event listener** để xử lý tương tác.
- **Có thể sử dụng các API độc quyền của trình duyệt** như **geolocation, local storage** để xây dựng UI phục vụ các trường hợp sử dụng cụ thể.
- **Được dùng từ trước đến nay trong React**, nhưng bây giờ được phân biệt rõ hơn với Server Components.

---

#### **Server Components là gì?**

Ngược lại với Client Components, **Server Components là một loại component mới** được thiết kế **chỉ chạy trên server**. Khác với Client Components, **mã của Server Components không bao giờ được tải xuống client**.

**Tại sao lại chọn cách thiết kế này?** Điều này mang lại **nhiều lợi ích quan trọng** cho ứng dụng React:

1. **Kích thước bundle nhỏ hơn**
    
    - Vì **Server Components chỉ chạy trên server**, nên **tất cả dependencies của chúng cũng ở trên server**.
    - Điều này cực kỳ có lợi cho người dùng có **kết nối mạng chậm hoặc thiết bị yếu**, vì họ **không cần tải, parse hoặc thực thi lượng lớn JavaScript**.
    - **Không cần hydration**, giúp ứng dụng tải nhanh hơn và tương tác sớm hơn.
2. **Truy cập trực tiếp vào tài nguyên server**
    
    - Server Components có thể **kết nối trực tiếp với cơ sở dữ liệu và hệ thống tệp**, giúp việc **fetch dữ liệu trở nên cực kỳ hiệu quả**.
    - **Không cần xử lý dữ liệu phía client**, giảm tải công việc cho trình duyệt.
    - **Sử dụng sức mạnh của server** để thực hiện các tác vụ render nặng.
3. **Cải thiện bảo mật**
    
    - Vì **Server Components chỉ chạy trên server**, nên **các thông tin nhạy cảm như API key và token không bao giờ rời khỏi server**.
    - Điều này giúp **giảm nguy cơ rò rỉ dữ liệu** và tăng cường bảo mật.
4. **Tối ưu hóa việc fetch dữ liệu**
    
    - Server Components giúp **di chuyển quá trình fetch dữ liệu về gần nguồn dữ liệu hơn**.
    - Điều này giúp **giảm thời gian lấy dữ liệu**, **giảm số lượng request từ client** và **tăng hiệu suất render**.