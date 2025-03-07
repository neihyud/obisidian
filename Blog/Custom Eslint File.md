[custom-rule-eslint](https://eslint.org/docs/latest/extend/custom-rule-tutorial)
- **b1: Tạo folder ở root project**
```shell
mkdir eslint-plugin-custom-rules
cd eslint-plugin-custom-rules
```
- **b2: Tạo file index.js**
```js
module.exports = {
  rules: {
    'no-plain-text': {
      create: function (context) {
        return {
          Literal(node) {
            if (typeof node.value === 'string' && node.value.trim() !== '') {
              context.report({
                node,
                message: 'Do not use plain text. Use translation function instead.',
              });
            }
          },
          JSXText(node) {
            if (node.value.trim() !== '') {
              context.report({
                node,
                message: 'Do not use plain text. Use translation function instead.',
              });
            }
          },
        };
      },
    },
  },
};
```
- **b3: Đặt tên cho module eslint**: **ex - *eslint-plugin-custom-rules***
- **b4: Cấu hình** ***package.json*** -> **yarn**
```js
{
  "name": "your-project-name",
  "version": "1.0.0",
  "dependencies": {
    ...
  },
  "devDependencies": {
    "eslint": "^8.57.0",
    "eslint-plugin-custom-rules": "file:./eslint-plugin-custom-rules"
  }
}

```
- **b5: Cấu hình *.eslintrc.js***
```js
module.exports = {
  parser: '@babel/eslint-parser',
  parserOptions: {
    ecmaVersion: 2020,
    sourceType: 'module',
  },
  env: {
    browser: true,
    es6: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
  ],
  plugins: ['custom-rules'],
  rules: {
    'custom-rules/no-plain-text': 'error',
  },
};

```
- **b6: Run lại để config**
	**npx eslint .**
