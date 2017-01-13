# Opções de Referência

### Diferença do uso entre Webpack 1 e 2

Para Webpack 1.x: Adicione um bloco \`vue\` na raiz nas suas configurações Webpack:

``` js
module.exports = {
  // ...
  vue: {
    // vue-loader options
  }
}
```

Para Webpack 2 (^2.1.0-beta.25)

``` js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          // vue-loader options
        }
      }
    ]
  }
}
```

### loaders

- type: `Object`
  
  Um objeto que especifica carregadores de Webpack para usar para blocos de linguagem dentro de arquivos `*.vue`. A chave corresponde ao atributo `lang` para o bloco de linguagem, se especificado. O padrão `lang` para cada tipo é:
  
  - `<template>`: `html`
  - `<script>`:`js`
  - `style`: `css`
  
  Por exemplo, para usar `babel-loader` e `eslint-loader` para processar todos os blocos `<script>`: 
  
  ``` js
  // ...
  vue: {
    loaders: {
      js: 'babel!eslint'
    }
  }
  ```
  
### postcss

- type: `Array` ou `Function` ou `Object`
- Formato `Object` suportado apenas em ^8.5.0

  Especifica plugins PostCSS customizados para serem aplicado a CSS dentro de arquivos `*.vue`. Se estiver usando uma função, a função irá ser chamada usando o mesmo contexto do loader e deve retornar uma Array de plugins. 

  ``` js
  // ...
  vue: {
    // note: do not nest the `postcss` option under `loaders`
    postcss: [require('postcss-cssnext')()],
    loaders: {
      // ...
    }
  }
  ```
  
  Está opção também pode ser um objeto que contém opções para ser passada para o processador PostCSS. Isto é útil quando você está usando projetos PostCSS que entrega um parser/stringifiers personalizado:
  
  ``` js
  postcss: {
    plugins: [...], // list of plugins
    options: {
      parser: sugarss // use sugarss parser
    }
  }
  ```
  
### cssSourceMap

- type: `Boolean`
- default: `true`

  Se deve habilitar mapas de origem para CSS. Desativar isso pode evitar alguns erros relacionado a caminho relativo em `css-loader` e fazer a construção um pouco mais rápida.
  
  Observe que isso é automaticamente definida para `false` se a opção `devtoo` não estiver presente as configurações principal de Webpack.
  
### esModule

- type: `Boolean`
- default: `undefined`

  Se deve emitir código compatível esModule. Por padrão vue-loader irá emitir default export em formato commonjs como `module.exports = ....`. Quando `esModule` estiver definido como true, default export irá ser transpilado em `exports.__esModule = ture; exports = ....`. Útil para interoperação com transpiladores diferente de Babel, como TypeScript.
  
### preserveWhitespace

- type: `Boolean`
- default: `true`

  Se definido para `false`, os espaços em branco entre as tags HTML em templates serão ignorados.

### transformToRequire

- type: `{ [tag: string]: string | Array<string> }`
- default: `{ img: 'src' }`

  Durante a compilação do template, o compilador pode transformar certos atributos, tais como URLs `src`, em chamadas `require`, para que o recurso de destino possa ser manipulado pelo Webpack. A configuração padrão transforma o atributo `src` em tags `<img>`. 
  
### buble

- type: `Object`
- default: `{}`
  
  Opções de configuração para o `buble-loader` (Se estiver presente), E a compilação buble passa para o template as funções de renderização.
  
  > Nota de versão: Na versão 9.x, as expressões do template são configuradas separadamente através da opção `templateBudle` agora removida.
  
  A função de renderização do template suporta uma transformação especial `stripWith` (habilitada por padrão), que remove o `with` usado em funções de renderização geradas para torná-las compatíveis com modelo estrito.
  
  Exemplo de configuração:
  
  ``` js
  // webpack 1
  vue: {
    buble: {
      // enable object spread operator
      // NOTE: you need to provide Object.assign polyfill yourself!
      objectAssign: 'Object.assign',

      // turn off the `with` removal
      transforms: {
        stripWith: false
      }
    }
  }

  // webpack 2
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          buble: {
            // same options
          }
        }
      }
    ]
  }
  ```












