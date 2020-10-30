# svelte + typescript + scss(autoprefixer) + pug (rollup)
기본 환경 세팅

## Rollup - 기초 세팅 순서

```
폴더 생성
cd 폴더
npx degit sveltejs/template
node scripts/setupTypeScript.js
npm install
```

### svelte.config.js 생성
```
npm i -D svelte-preprocess
```

```js
// svelte.config.js
const sveltePreprocess = require('svelte-preprocess')
const production = !process.env.ROLLUP_WATCH;

module.exports = {
    // enable run-time checks when not in production
    dev: !production,
    // we'll extract any component CSS out into
    // a separate file - better for performance
        css: css => {
        css.write('public/build/bundle.css');
    },
    preprocess: sveltePreprocess(),
}
```
```js
// rollup.config.js
//...
export default {
  //...
  plugins: [
    svelte(require('./svelte.config')),
    //...
  ],
  //...
};
```

### typescript 설정
```js
// svelte.config.js
//...
module.exports = {
    //...
    preprocess: sveltePreprocess({
        sourceMap: !production
    }),
}
```

### scss 설정
```
npm i -D sass rollup-plugin-scss
```

공통 css 추가
```js
// rollup.config.js
//...
import scss from 'rollup-plugin-scss';

export default {
  plugins: [
    //...
    scss({
      output: 'public/global.css'
    }),
    //...
  ]
};
```

main.ts 에 import
```js
// src/main.ts
import './assets/scss/global.scss';
import App from './App.svelte';

const app = new App({
	target: document.body,
	props: {
		name: 'world'
	}
});

export default app;
```

SCSS 변수 import 안하고 사용
```js
// svelte.config.js
//...
module.exports = {
    //...
    preprocess: sveltePreprocess({
    //...
    scss: {
            prependData: `@import "src/assets/scss/variables.scss";`
        }
    }),
}
```

```scss
/* src/assets/scss/variables.scss */
$primary-color: #ff3e00;
```

### postCss 설정
```
npm i -D postcss autoprefixer
```

autoprefixer 설정
```js
// svelte.config.js
const autoprefixer = require('autoprefixer');

//...
module.exports = {
    //...
    preprocess: sveltePreprocess({
        //...
        postcss: {
        plugins: [autoprefixer()]
        },
    }),
}
```

### pug 추가
```
npm i -D pug
```

### Alias 설정
```
npm i -D @rollup/plugin-alias
```

```js
// rollup.config.js
//...
import alias from '@rollup/plugin-alias';
import path from 'path';
//...

export default {
  //...
  plugins: [
    //...
    alias({
      entries: [
        { find: '@', replacement: path.resolve(__dirname, 'src') }
      ]
    }),
    //...
};
```

typescript import 위해서 tsconfig.json 수정
```js
// tsconfig.json
{
  //...
  "compilerOptions": {
    "baseUrl": "./",
    "paths": { "@/*": ["src/*"] }
  }
}
```

---

## 추가 모듈

### svelte
- <a href="https://www.npmjs.com/package/svelte-preprocess" target="_blank">svelte-preprocess</a>

### scss
- <a href="https://www.npmjs.com/package/sass" target="_blank">sass</a>
- <a href="https://www.npmjs.com/package/rollup-plugin-scss" target="_blank">rollup-plugin-scss</a>
- <a href="https://www.npmjs.com/package/postcss" target="_blank">postcss</a>
- <a href="https://www.npmjs.com/package/autoprefixer" target="_blank">autoprefixer</a>

### pug
- <a href="https://www.npmjs.com/package/pug" target="_blank">pug</a>

### alias
- <a href="https://www.npmjs.com/package/@rollup/plugin-alias" target="_blank">@rollup/plugin-alias</a>
- <a href="https://www.npmjs.com/package/path" target="_blank">path</a>

---
