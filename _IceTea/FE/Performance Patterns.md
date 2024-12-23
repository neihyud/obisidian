- Server send response contain HTML -> browser parse HTML to DOM, init request when find external resource , if request block, parse HTML will pause until resource import handled
- -> after build DOM -> build CSSOM

```
Performance tips include 
1) minimizing the number of critical resources by deferring non-critical ones' download, marking them as async, or eliminating them altogether, 
2) optimizing the number of requests required along with the file size of each request 
3) optimizing the order in which critical resources are loaded by prioritizing the downloading of critical assets, thereby shortening the critical path length.
```
# Bundle splitting
- when build a modern web, bundler take souce and bundle this together into one or more bundles -> user visits a website, the bundle is **requested** and **loaded** in order to display the data to the user’s screen.
- The UI only becomes interactive after the bundle has been loaded and executed

- **First Contentful Paint:**  the time it takes before the first component has been rendered to the screen
- **Largest Contentful Paint**: the time it takes before the largest component has been rendered to the screen
 - **Time To Interactive:** The time it takes before all content has been painted to the screen and has been made interactive, is called the.
 
![[bundle.mp4]]

 => The `EmojiPicker` isn’t directly visible ( even click on the `Emoji`) -> unnecessarily added the `EmojiPicker` module to our initial bundle, which potentially increased the loading time
 => user import dynamic

---
- Load component at a more opportune moment:
	- When the user clicks to interact with that component for the first time
	- Scrolls the component into view
	- Eager - load resource right away (the normal way of loading scripts)
	- Lazy ([Route-based](https://web.dev/code-splitting-with-dynamic-imports-in-nextjs/#route-based-and-component-based-code-splitting)) - load when a user navigates to a route or component
	- Lazy (On interaction) - load when the user clicks UI (e.g Show Chat)
	- Lazy (In viewport) - load when the user scrolls towards the component
	- [Prefetch](https://web.dev/link-prefetch/) - load prior to needed, but after critical resources are loaded
	- [Preload](https://web.dev/preload-critical-assets/) - eagerly, with a greater level of urgency