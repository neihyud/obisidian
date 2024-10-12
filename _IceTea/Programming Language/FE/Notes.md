![[Pasted image 20240828113706.png]]
## Show Less and Show More
```md
Show less, show more base on height

useLayoutEffect(() => {
if (ref.current.clientHeight < ref.current.scrollHeight) {
	-> show more
}

but its not correct with image because image not load -> dimension incorrect
=> use event onLoad

}, [])

=> Solution
useLayoutEffect(() => {

	const element = ref?.current;
	const handleImageLoad = () => {
		if (element?.clientHeight && element?.scrollHeight) {
			if (element?.clientHeight < element.scrollHeight) {
				setIsVisible(true);
			}

		}
};

const images = element?.querySelectorAll("img");
	if (images && images.length > 0) {
		images.forEach((img) => {
			img.addEventListener("load", handleImageLoad);
		}
	);
	} else {
	
		if (element && element.clientHeight < element.scrollHeight) {
			setIsVisible(true);
		}
	}

  

return () => {
	if (images && images.length > 0) {
		images.forEach((img) => {
			img.removeEventListener("load", handleImageLoad);	
		});
	}
};

}, [ref]);
```


## React hook form
- currently not active correct with Controller => use **reset(defaultValues)**
## React Select 
```react=
<Select 
    // other props
    menuPortalTarget={document.body} 
    styles={{ menuPortal: base => ({ ...base, zIndex: 9999 }) }}
/>
```
=> fix **z-index**: focus-within:z-20
![[Pasted image 20240919195349.png]]
