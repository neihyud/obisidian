# Check Type Image
![[magic_number.png]]
```html
<script>
  function check(headers) {
    return (buffers, options = { offset: 0 }) => headers.every((header, index) => header === buffers[options.offset + index]);
  }
  
  function readBuffer(file, start = 0, end = 2) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = () => {
        resolve(reader.result);
      };
      reader.onerror = reject;
      reader.readAsArrayBuffer(file.slice(start, end));
    });
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

```
# RedLock
```javascript

const crypto = require('crypto') // have in module nodejs

/**
 * Create checksum
 * 
 * @param {string} file Absolute file path 
 * @returns {Promise<string | null} Hash file
 */
const _checksumFile = async (file) => {
    if (!file) return null
  
    return new Promise((resolve, reject) => {
        Fs.readFile(file, function (err, data) {
            if (err) return reject(err)
            const md5 = crypto.createHmac('md5', 'secret').update(data).digest('hex')
            return resolve(md5)
        })
    })
}

/**
 * @param {FileObject} file 
 */
const _lockFile = async (file) => {
    if (!file || !file.path) {
        throw new Error('File is empty or invalid.')
    }
    const redlock = createRedlockConnect([clients], { 
        driftFactor: 0.01,
        retryCount: 0,
        retryDelay: 200, 
        retryJitter: 200
    })
    
    const _hashingMd5 = await _checksumFile(file.path)

    const resource = `locks:submitCashback:${_hashingMd5}`
    
    try {
        await redlock.lock(resource, TTL_LOCK)
    } catch (error) {
        throw new Error(`Don't submit duplicate cashback files`)
    }
}
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
