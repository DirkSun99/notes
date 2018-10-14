## 搭建Vue的开发环境

> https://cn.vuejs.org/v2/guide/installation.html

1. 必须要安装nodejs
2. 搭建Vue的开发环境，安装Vue的脚手架工具(官方命令行工具)
	
	`npm install --global vue-cli`

3. 创建项目
	
	```shell
	vue init webpack vue-demo01
	cd vue-demo01

	# 如果创建项目的时候没有报错，这一步可以省略；如果报错了，cd到项目里面执行
	cnpm install / npm install

	npm run dev
	```

4. 如果npm速度慢的话，使用cnpm

	```shell
	http://npm.taobao.org/

	npm install -g cnpm --registry=https://registry.npm.taobao.org
	```

5. **另一种创建项目的方式(推荐)**
	
	```shell
	vue init webpack-simple vue-demo02
	cd vue-demo02

	# 如果创建项目的时候没有报错，这一步可以省略；如果报错了，cd到项目里面执行
	cnpm install / npm install

	npm run dev
	```