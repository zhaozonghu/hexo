---
title: Gulp 自动化构建工具-初探
categories: web
tags: node
---

![gulp](http://or49syh5a.bkt.clouddn.com/525072a07f737c14623sfefs1.png)

<!--more-->

##### 入门

###### 全局安装 gulp

	$ npm install --global gulp
	
###### 在项目中运用（git barsh）

	$ npm install --save-dev gulp
	
###### 在项目根目录下创建 gulpfile.js 文件

	var gulp = require('gulp');
	
	gulp.task('auto',function(){
		//默认执行的任务代码放在这里
	})

然后使用下面命令即可：

	$ gulp auto

###### 删除模块安装

	$ npm install --save-dev gulp del
	
使用：

	gulp.task('clean',function(cb){
		del(['./dist/**/*'],cb);
	});
	
##### 常用方法整理（gulpfile.js）：

	var gulp=require('gulp');
	var concat = require('gulp-concat');//文件合并
	var uglify = require('gulp-uglify');//文件压缩
	var jshint = require('gulp-jshint');//jshint检查
	var cleanCSS = require('gulp-clean-css');//压缩 CSS 文件
	var htmlmin = require('gulp-htmlmin');//压缩 html 文件
	var imagemin = require('gulp-imagemin');//压缩图片文件
	var rename = require('gulp-rename');//重命名
	var del = require('del');//清除文件
	var rev = require('gulp-rev');//- 对文件名加MD5后缀
	var revCollector = require('gulp-rev-collector');//- 路径替换
	var babel = require('gulp-babel');//ES6编译为ES2015
	var less = require('gulp-less');//less编译
	var plumber = require('gulp-plumber');//异常处理
	var notify = require('gulp-notify');
	var autoprefixer = require('gulp-autoprefixer');//css3前缀自动补全

	//清除文件
	gulp.task('clean',function(cb){
		del(['./dist/**/*','./rev/**/*'],cb);
	});
	gulp.task('cleanCss',function(cb){
		del(['./dist/css/*'],cb);
	});
	//压缩、合并js
	gulp.task('js',function(){
		return gulp.src('./js/*.js')
			.pipe(jshint())
			.pipe(uglify())
			.pipe(concat('all'))
			.pipe(rename(function(path){
				path.basename +='.min';
				path.extname ='.js'
			}))
			.pipe(rev())
			.pipe(gulp.dest('./dist/js/'))
			.pipe(rev.manifest())
			.pipe(gulp.dest('./rev/js'));
	});
	//less编译
	gulp.task('less2css',function(){
		return gulp.src('./less/common.less')
			.pipe(plumber({errorHandler: notify.onError('Error: <%= error.message %>')}))  //异常处理
			.pipe(less())
			.pipe(gulp.dest('./css/'))
	});
	//压缩、合并css
	gulp.task('css',['less2css'],function(){
		return gulp.src('./css/*.css')
			.pipe(cleanCSS())
			.pipe(concat('all'))
			.pipe(rename(function(path){
				path.basename +='.min';
				path.extname ='.css'
			}))
			.pipe(rev())
			.pipe(gulp.dest('./dist/css/'))
			.pipe(rev.manifest())
			.pipe(gulp.dest('./rev/css'));
	});
	//css3前缀补全
	gulp.task('autoFx',['css'],function(){
	   return gulp.src('./dist/css/*.css')
			.pipe(autoprefixer({
				browsers: ['last 5 versions']
			}))
			.pipe(gulp.dest('./dist/css'));
	});
	//压缩html
	gulp.task('html', function() {
		var options = {
			removeComments: true,  //清除HTML注释
			collapseWhitespace: true,  //压缩HTML
			collapseBooleanAttributes: true,  //省略布尔属性的值 <input checked="true"/> ==> <input checked />
			removeEmptyAttributes: true,  //删除所有空格作属性值 <input id="" /> ==> <input />
			removeScriptTypeAttributes: true,  //删除<script>的type="text/javascript"
			removeStyleLinkTypeAttributes: true,  //删除<style>和<link>的type="text/css"
			minifyJS: true,  //压缩页面JS
			minifyCSS: true  //压缩页面CSS
		};
		return gulp.src('./*.html')
			.pipe(htmlmin(options))
			.pipe(gulp.dest('./dist/'));
	});
	//压缩图片
	gulp.task('img', function(){
		gulp.src('./images/*')
			.pipe(imagemin({
				optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
				progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
				interlaced: true, //类型：Boolean 默认：false 隔行扫描gif进行渲染
				multipass: true //类型：Boolean 默认：false 多次优化svg直到完全优化
			}))
			.pipe(gulp.dest('dist/images'));
	});

	//文件路径替换
	gulp.task('rev',['autoFx','js'],function(){
		return gulp.src(['./rev/**/*.json','./dist/*.html'])
			.pipe(revCollector({replaceReved: true}))
			.pipe(gulp.dest('./dist/'));
	});

	//监听less修改
	gulp.task('lessWatch', function () {
		gulp.watch('./less/*.less', ['cleanCss','rev']);
	});

	gulp.task('tigger',['css','js','img','html']);

	
##### 插件地址

1. [gulp-rename](https://www.npmjs.com/package/gulp-rename)： 重命名
2. [gulp-uglify](https://www.npmjs.com/package/gulp-uglify)：文件压缩
3. [gulp-concat](https://www.npmjs.com/package/gulp-concat)：文件合并
4. [gulp-less](https://www.npmjs.com/package/gulp-less)：编译 less
5. [gulp-sass](https://www.npmjs.com/package/gulp-sass)：编译 sass
6. [gulp-clean-css](https://www.npmjs.com/package/gulp-clean-css)：压缩 CSS 文件
7. [gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin)：压缩 HTML 文件
8. [gulp-babel](https://www.npmjs.com/package/gulp-babel): 使用 babel 编译 JS 文件
9. [gulp-jshint](https://www.npmjs.com/package/gulp-jshint)：jshint 检查
10. [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)：压缩jpg、png、gif等图片
11. [gulp-rev](https://www.npmjs.com/package/gulp-rev/)：版本控制
12. [gulp-rev-collector](https://www.npmjs.com/package/gulp-rev-collector/)：文件名称替换（rev）
13. [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer)：css3前缀补全

> [gulp中文教程](http://www.gulpjs.com.cn/)




