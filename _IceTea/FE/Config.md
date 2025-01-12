- [UI color](https://uicolors.app/create) - Get level color
- [UI color contrast](https://coolors.co/contrast-checker/112a46-acc8e5) - Get color background and text
- [Svg convert](https://www.svgviewer.dev/) - Convert to svg component
- [Paste image to download](https://www.paste.photos/)
- [It tools](https://it-tools.tech/date-converter)
- [All tool in one](https://truedevtools.com/)
- [Check page speed](https://pagespeed.web.dev/)
- [Place hold space for image](https://placehold.co/)
- [Database diagram](https://dbdiagram.io/d)

- Be Vietnam pro: font VN
- **Databinhdan**: learn data
- [Get meta for page ](https://tools.j2team.dev/)


## Build project
- `lint-staged`:  install dev-save and insert into file package.json
```
"lint-staged": {
    "*.ts": [
      "npm run lint",
      "npm run format",
      "git add ." => after commit, add file in staged
    ]
}
```
- `husky`: 
```
npm install --save-dev husky

npx husky init

echo "npx --no -- commitlint --edit \$1" > .husky/commit-msg

echo "format:check" > .husky/pre-commit => to run code pre-commit, add "format:check" into script in package.json
```

```
 "scripts": {
    "lint": "biome lint .",
    "lint:fix": "biome lint . --write",
    "format:check": "biome format . ",
    "format:fix": "biome format .  --write",
    "check-types": "tsc --noEmit",
    "postinstall": "husky",
  }
```
- `CommitLint`: [commit-lint](https://commitlint.js.org/guides/getting-started.html)
```
npm install --save-dev @commitlint/{cli,config-conventional}

echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js

```

- `biome`: [[biome]] - file biome config
- add folder `.vscode`: [[vscode]] - file vscode

=>Remember: change icon, and title