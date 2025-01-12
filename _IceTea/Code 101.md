---
title: Code 101

---
![[Pasted image 20241201225503.png]]
# Check isPng

```javascript=
    <body>
        <div>
            <input type="file" id="inputFile" accept="image/*" onchange="handleChange(event)"/>
            <p id="realFileType"></p>
        </div>
        <script>
            function check(headers) {
                return (buffers,options={
                    offset: 0
                })=>headers.every((header,index)=>header === buffers[options.offset + index]);
            }

            function readBuffer(file, start=0, end=2) {
                return new Promise((resolve,reject)=>{
                    const reader = new FileReader();
                    reader.onload = ()=>{
                        resolve(reader.result);
                    }
                    ;
                    reader.onerror = reject;
                    reader.readAsArrayBuffer(file.slice(start, end));
                }
                );
            }

            const isPNG = check([0x89, 0x50, 0x4e, 0x47, 0x0d, 0x0a, 0x1a, 0x0a]);
            const realFileElement = document.querySelector("#realFileType");

            async function handleChange(event) {
                const file = event.target.files[0];
                const buffers = await readBuffer(file, 0, 8);
                const uint8Array = new Uint8Array(buffers);
                realFileElement.innerText = `The type of ${file.name} is：${isPNG(uint8Array) ? "image/png" : file.type}`;
            }
        </script>
    </body>
```

# Format and Convert Number
```javascript
function formatAndConvertToNumber(num) {
    // Định dạng số với tối đa 10 chữ số thập phân
    const formatter = new Intl.NumberFormat('en-US', {
        minimumFractionDigits: 0,
        maximumFractionDigits: 10
    });
    // Định dạng số
    const formatted = formatter.format(num);
    // Chuyển đổi chuỗi định dạng trở lại thành số
    return parseFloat(formatted);
}

// Ví dụ
const number1 = 3.14159265358979; // Có nhiều hơn 10 chữ số thập phân
const number2 = 3.1415926535;     // Có ít hơn 10 chữ số thập phân

console.log(formatAndConvertToNumber(number1)); // 3.1415926536
console.log(formatAndConvertToNumber(number2)); // 3.1415926535

```
# Transaction to Height Auto
```html
.wrapper {
  display: flex;
}

.inner {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.5s ease-out;
}

.wrapper.is-open .inner {
  max-height: 100%;
}


--- 

<div class="wrapper">
  <div>
    <div class="inner">Expandable content</div>
  </div>
</div>

```
