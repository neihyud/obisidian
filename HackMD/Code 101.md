---
title: Code 101

---

# Code 101

## Eslint - config
```javascript=
module.exports = {
  root: true,
  env: { browser: true, es2020: true },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs'],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    parser: '@babel/eslint-parser',
    babelOptions: {
      parserOpts: {
        plugins: ["jsx"],
      },
    }
  },
  settings: { 
    react: { 
      version: '18.2' 
    }
  },
  plugins: [
    'react-refresh',
    'react',
  ],
  rules: {
    "array-bracket-spacing": ["error", "never"],
    "brace-style": ["error", "1tbs"],
    "comma-spacing": "error",
    "comma-style": "error",
    "computed-property-spacing":  ["error", "never"],
    "constructor-super": "error",
    "dot-notation": "error",
    "eol-last": "error",
    "func-call-spacing": "error",
    "indent": [ "error", "tab", { "SwitchCase": 1 } ],
    "key-spacing": "error",
    "keyword-spacing": "error",
    "no-alert": "error",
    "no-bitwise": "error",
    "no-console": "error",
    "no-const-assign": "error",
    "no-debugger": "error",
    "no-dupe-args": "error",
    "no-dupe-class-members": "error",
    "no-dupe-keys": "error",
    "no-duplicate-case": "error",
    "no-duplicate-imports": "error",
    "no-else-return": "error",
    "no-eval": "error",
    "no-extra-semi": "error",
    "no-fallthrough": "error",
    "no-lonely-if": "error",
    "no-mixed-operators": "error",
    "no-mixed-spaces-and-tabs": "error",
    "no-multiple-empty-lines": [ "error", { "max": 1 } ],
    "no-multi-spaces": "error",
    "no-negated-in-lhs": "error",
    "no-nested-ternary": "error",
    "no-redeclare": "error",
    "no-shadow": "error",
    "no-undef-init": "error",
    "no-unreachable": "error",
    "no-unsafe-negation": "error",
    "no-use-before-define": [ "error", "nofunc" ],
    "no-useless-computed-key": "error",
    "no-useless-constructor": "error",
    "no-useless-return": "error",
    "no-var": "error",
    "no-whitespace-before-property": "error",
    "object-curly-spacing": [ "error", "always" ],
    "one-var": "off",
    "prefer-const": "error",
    "semi": ["error","never"],
    "semi-spacing": "error",
    "space-before-blocks": [ "error", "always" ],
    "space-before-function-paren": [ "error", "never" ],
    "space-in-parens": [ "error", "never" ],
    "space-infix-ops": [ "error", { "int32Hint": false } ],
    "space-unary-ops": [ "error"],
    "template-curly-spacing": [ "error", "never" ],
    "valid-jsdoc": [ "error", { "requireReturn": false } ],
    "valid-typeof": "error",
    "yoda": [0],
    "linebreak-style": [
        2,
        "unix"
    ],
    "quotes": [
        2,
        "single"
    ],
    "curly": [
        2,
        "all"
    ],
    "eqeqeq": [
        2,
        "smart"
    ],
    "one-var-declaration-per-line": [
        2,
        "always"
    ],
    "new-cap": 2,
    "no-case-declarations": 0,
  }
}

```

## Check isPng

![](https://i.imgur.com/vHKeUI6.png)


```javascript=
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>File Type Detect Demo</title>
    </head>
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
</html>
```