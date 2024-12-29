![[Pasted image 20241226102522.png]]
```
document.querySelectorAll('*').length;
=> show page có bao nhiêu DOM (một page nên có tối đa 1400 DOM)
```

## Show Less and Show More
![[Pasted image 20240828113706.png]]
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


## Svg
### Viewbox
https://www.digitalocean.com/community/tutorials/svg-svg-viewbox
-  **`viewport`** as the window to our image and the `viewBox` as the tool we use to scale and position the image.

![[Pasted image 20241120163112.png]]
- **x** - specify the minimum x coordinate
- **y** - specify the minimum y coordinate
- **width** - width in user coordinates/px units
- **height** - height in user coordinates/px units

https://www.digitalocean.com/community/tutorials/svg-preserve-aspect-ratio
- **preserveAspectRatio**: use to scale svg when **viewBox** and **viewPort** difference
	- ex: 'svg viewBox="0 0 100 100" preserveAspectRatio="xMidYMid meet"'
		- xMidYMid - center the 'viewBox' region within the 'viewPort' region
		- meet - scale our graphic until it 'meets' the height and width of our 'viewPort'


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


# Trick
## Border radius
![[Pasted image 20241121102443.png]]