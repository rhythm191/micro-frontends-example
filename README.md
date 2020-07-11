# micro-frontends-example

ひとつの Web アプリケーションを複数のリポジトリで管理するサンプルです。
やっていることは単純で、ベースとなるアプリケーションを Nuxt.js で SSG し、
その生成物のサブディレクトリをサブアプリケーションの生成物でオーバーライドします。

## 仕組み解説

base となる Nuxt.js アプリケーションを SSG した場合、生成物は次のようになっています。

```
dist
├── 200.html
├── README.md
├── _nuxt/...
├── favicon.ico
└── index.html
```

その生成物に対して、別の Nuxt.js アプリケーションを SSG してサブディレクトリに配置することで、
サブディレクトリ配下を別のリポジトリで管理することができるようになります。

```
dist
├── 200.html
├── README.md
├── _nuxt/...
├── favicon.ico
├── hoge
│   ├── 200.html
│   ├── README.md
│   ├── _nuxt/...
│   ├── favicon.ico
│   └── index.html
└── index.html
```

この時、nuxt.config.json は全て、SSG を行う設定をする必要があります。

```
export default {
  /*
  ** Nuxt rendering mode
  ** See https://nuxtjs.org/api/configuration-mode
  */
  mode: 'universal',
  /*
  ** Nuxt target
  ** See https://nuxtjs.org/api/configuration-target
  */
  target: 'static',

```

また、サブアプリケーションはどのサブディレクトリに配置するかを nuxt.config.js に記述する必要があります。

```
  /*
   ** Routings
   */
  router: {
    base: "/hoge/"
  },
```

あとは、 `nuxt build && nuxt export` した結果を生成物ディレクトリに適切に配置します。
このサンプルではひとつのリポジトリ内に全て集約しましたが、生成物をコピーするだけなので、
デプロイ時にマージすることでサブアプリケーションは別のリポジトリに分割することが可能です。

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run all
$ npm run start
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).
