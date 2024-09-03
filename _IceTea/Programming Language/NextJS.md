s ## Terms
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
![[Pasted image 20240730105603.png]]
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