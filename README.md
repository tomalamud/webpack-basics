## Basic usage
webpack solamente entiende JavaScript y JSON. Los loaders nos permite procesar archivos de otros tipos para convertirnos en módulos válidos que serán consumidos por nuestras aplicaciones y agregadas como dependencias.

En alto nivel, los **loaders** poseen 2 configuraciones principales:
- test - propiedad que identifica cuáles archivos deberán ser transformados
- use - propiedad que identifica el loader que será usado para transformar a dichos archivos

**Plugins:**
Mientras los loaders transforman ciertos tipos de módulos, los plugins _son utilizados para extender tareas específicas, como la optimización de paquetes, la gestión de activos y la inyección de variables de entorno.

Una vez importado el plugin, podemos desear el personalizarlos a través de opciones.

### instalation
1) npm i webpack webpack-cli -D
2) npx webpack --mode production
esto crea una carpeta ./dist.

### config de entrada y salida
3) webpack.config.js
- punto de entrada
- salida
- extensiones

```js
const path = require('path');

module.exports = {
	entry: './src/index.js',
	output: {
		path: path.resolve(__dirname, 'dist'), // dónde se encuentra nuestro proyecto.
		filename: 'bundle.js',
	},
	resolve: {
		extensions: ['.js*']
	}
}
```
Si ya tenemos el archivo de config, podemos ejecutar
4) npx webpack --mode production --config webpack.config.js
Este comándo se lo suele agregar como build en el package.json.
### Babel loader
Compatibilidad con todos los browsers
5) npm i babel-l
oader @babel/core @babel/preset-env @babel/plugin-transform-run
time -D
6) crear .babelrc
```js
{
	"presets": [	
		"@babel/preset-env"	
	],
	"plugins": [
		"@babel/plugin-transform-runtime"
	]
}
```
7) añadir a webpack
```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'), // dónde se encuentra nuestro proyecto.
    filename: 'bundle.js',
  },
  resolve: {
    extensions: ['.js']
  },
  module: {
    rules:[
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ] 
  }
}
```
### Html loader
8) npm i html-webpack-plugin -D
9) add
```js
plugins: [
  new HtmlWebpackPlugin({
    inject: true,
    template: './public/index.html',
    filename: './index.html'
  })
]
```
### Css y preprocesadores
10) npm i mini-css-extract-plugin css-loader -D
11) webpack.config:
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'), // dónde se encuentra nuestro proyecto.
    filename: 'main.js',
  },
  resolve: {
    extensions: ['.js']
  },
  module: {
    rules:[
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      },
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, 'css-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      inject: true,
      template: './public/index.html',
      filename: './index.html'
    }),
    new MiniCssExtractPlugin(),
  ]
```
12) npm i stylus stylus-loader -D

### loaders para fuentes
incorporarlas al proyecto para evitar el llamado al cdn
