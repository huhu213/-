###WebPack

WebPack是一个模块加载器兼打包工具，分析项目结构并打包为合适的格式供浏览器使用。

####webpack chunk

webpack诞生的目的就是Code Spliting，并保证静态资源的模块化无缝集成。Code Spliting可以实现按需加载，减少首次加载时间。webpack通过code spliting将代码分离成chunk，chunk可以按需加载。

code spliting的具体做法是生成一个分离点，在分离点中依赖的模块会被打包到一起，可以异步加载。一个分离点会产生一个打包文件。

通过给定的主文件入口，找到项目所有的依赖文件，使用不同的loaders处理（在webpack的config文件里指定对应的loader），最终打包为一个浏览器可识别的JS文件。

配置文件webpack.config.js
>      module.exports = {
> 			devtool: "eval-source-map",//生成编译后文件和源文件的对应映射
>			entry: _dirname + "/app/main.js";//项目入口
>			output: {
>				path: _dirname + "/public",//打包后文件存放的地方
>				filename: "bundle.js"
>			}
>		}

**_dirname是nodejs中的一个全局变量，指向当前执行脚本所在的目录**

webpack命令执行方式: node_modules/.bin/webpack

npm配置文件中，可以通过配置scripts属性，来指定npm start命令指向webpack

>     {
> 			"name": "webpack-sample-project",
> 			"version": "1.0.0",
> 			"description": "Sample webpack project",
> 			"scripts": {
> 				"start": "webpack",
> 				"build": "webpack-build"
> 			},
> 			...
> 		}
>  npm的start是一个特殊的脚本名称，执行npm start相当于执行npm run start，对应于其他的script，执行npm run {script name}即可。

####webpack loader
loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个

>       module.exports = {
> 			devtool: "eval-source-map",//生成编译后文件和源文件的对应映射
>			entry: _dirname + "/app/main.js";//项目入口
>			output: {
>				path: _dirname + "/public",//打包后文件存放的地方
>				filename: "bundle.js"
>			},
>			modules: {
>				loaders: [{
>					test: /\.json$/,//当前loader匹配的待处理的文件拓展名的正则表达式，必须
>					loader: "json"//loader的名字，必须
>					include/exclude: //手动添加需要处理或不用处理的文件
>					query: //为loader提供额外的配置选项
>				}]
>			}
>		}

**autoprefixer为文件自动添加前缀，用于确定文件是否改变过**

####webpack plugin
plugin和loader不一样，插件并不直接操作单个文件，它直接对整个构建过程其作用。

####webpack 缓存

缓存无处不在，使用缓存的最好方法是保证你的文件名和文件内容是匹配的（内容改变，名称相应改变）。webpack将hash值添加到文件名中。

#### 优点
* webpack将所有文件都看作模块，均可以通过require进行引入
* webpack以 CommonJS 的形式来书写脚本，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移。
* 开发便捷，能替代部分 grunt/gulp 的工作，比如打包、压缩混淆、图片转base64等。

#### 项目相关

智慧校园项目是一个基于Backbone.js的SPA，路由定义如下：

>      require.ensure([], function(require) {   			var pay = require('./pay/index').default; 			var PayRoute = Backbone.Router.extend({
        		routes: {
            		'pay': 'pay'
        		},
       		pay
   			});
  			new PayRoute();
		});
		Backbone.history.start();


require.ensure([deoendencies], callback, [chunkName]) 在需要的时候才下载依赖的模块，当参数指定的模块都下载下来了（下载下来的模块还没执行，只有回调函数中调用对应的模块时，才被执行），便执行参数指定的回调函数。require.ensure会创建一个chunk，且可以指定该chunk的名称，如果这个chunk名已经存在了，则将本次依赖的模块合并到已经存在的chunk中，最后这个chunk在webpack构建的时候会单独生成一个文件。

使用webpack进行文件压缩，打包等。

seajs基于CMD，webpack基于CommonJS

>     //CMD
> 		define(function(require, exports, module) {
> 			//...
> 			module.eports = ...;
> 		})
> 		//CommonJS
> 		var webpack = require('webpack');

CommonJS规范同步加载模块，当当前文件require的所有文件被加载完之后，才能执行后续的操作。AMD中模块加载则是异步加载。


