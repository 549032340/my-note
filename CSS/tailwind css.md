# tailwind css

### 安装 tailwind css 

#### vue+webpack 使用 tailwind css
1. 从头开始，首先初始化一个项目
 `vue create tailwind-emo`省略创建过程
2. 安装tailwind所需依赖
 
 - ~~`npm install tailwindcss postcss autoprefixer`~~  会报`PostCSS plugin tailwindcss requires PostCSS 8`错误
 - 安装低版本包 `npm install tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9`
3. 使用`npx tailwind init` 创建`tailwind.config.js`
4. 创建`postcss.config.js`文件

     ```js
	 const purgecss = require('@fullhuman/postcss-purgecss')({
		content: [
		  './src/**/*.html',
		  './src/**/*.vue',
		  './src/**/*.jsx',
		],

	// Include any special characters you're using in this regular expression
		defaultExtractor: content => content.match(/[\w-/:]+(?<!:)/g) || []
	  })
	   module.exports = {
		plugins: [
		  require('tailwindcss'),
		  require('autoprefixer'),
		  ...process.env.NODE_ENV === 'production'
		  ? [purgecss]
		  : []
		]
	  }
	```
  
	 
    
 5. 在`src/assets/css`下创建`tailwind.css`

     ```js
     @tailwind base;
     @tailwind components;
     @tailwind utilities;
     ```
 6. 在`main.js`中引入创建的css文件
      `import "./assets/css/tailwind.css"`
  7. 在`app.vue`粘贴以下代码，然后`npm run serve`启动服务查看效果
   ```vue
   	<template>
	  <div class="flex items-center justify-center align-middle">
		<div class="flex h-screen">
		  <div class="m-auto">
			<div class="md:flex">
			  <div class="md:flex-shrink-0">
				<img
				  class="rounded-lg md:w-56"
				  src="https://images.unsplash.com/photo-1556740738-b6a63e27c4df?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2550&q=80"
				  alt="Woman paying for a purchase"
				/>
			  </div>
			  <div class="mt-4 md:mt-0 md:ml-6">
				<div class="uppercase tracking-wide text-sm text-indigo-600 font-bold">Marketing</div>
				<a
				  href="#"
				  class="block mt-1 text-lg leading-tight font-semibold text-gray-900 hover:underline"
				>Finding customers for your new business</a>
				<p
				  class="mt-2 text-gray-600"
				>Getting a new business off the ground is a lot of hard work. Here are five ideas you can use to find your first customers.</p>
			  </div>
			</div>
		  </div>
		</div>
	  </div>
	</template>

	<style>
	</style>
   ```
#### 普通html页面使用wailwind css

1. 需要安装 tailwindcss、 postcss-cli、autoprefixer

   `npm install tailwindcss postcss-cli autofixer`

 2. 使用`npx tailwind init` 创建`tailwind.config.js`

3. 创建`postcss.config.js`文件

     ```js
     module.exports = {
       plugins:[
         require('tailwindcss'),
         require('autoprefixer')
       ]
     }
     ```

 4. 创建`css/tailwind.css`

     ```js
     @tailwind base;
     @tailwind components;
     @tailwind utilities;
     ```

 5. 在`package.json`文件中的`scripts`对象中添加打包命令

     ```js
     "scripts": {
         "build": "postcss css/tailwind.css -o public/build/tailwind.css"
       },
     ```

6. 通过`npm run build`命令生成`tailwind.css`文件

7. 在html页面中通过link标签引用`build/tailwind.css`文件

     ```
     <link rel="stylesheet" href="/build/tailwind.css">
     ```

 8. 然后就可以愉快的使用tailwind了

### 使用hover foucs active

- 需要在tailwind.config.js配置文件中的variants中配置用到的属性

  ```js
  variants: {
      backgroundColor: ['responsive', 'hover', 'focus', 'active'],
    },
  ```

  

### vue 使用 tailwindcss 报错 `PostCSS plugin tailwindcss requires PostCSS 8.`

- 删除原来安装的包 `npm uninstall tailwindcss postcss autoprefixer`
- 安装低版本包 `npm install tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9`