### let
> let语句声明一个块级作用域的本地变量，并且可选的赋予初始值。
> 
> let允许你声明一个作用域被限制在块级中的变量、语句或者表达式。与var关键字不同的是，var声明的变量只能是全局或者整个函数块的。

#### 作用域规则
> let只在其声明的块或子块可用。var声明的变量的作用域是整个封闭函数。

	//举个例子

	function var1 (){
		var a = 1;
		if( a ){
			var a = 2;
			console.log(a); //2
		}		
		console.log(a);     //2
	}
	
	function let1 (){
		let a = 1;
		if( a ){
			let a = 2;
			console.log(a); //2
		}
		console.log(a);     //1 
	}
	
#### 简化函数，干掉闭包

	//举个例子
	var list = document.getElementById('list');
	
	for( let i = 0 ; i <=5 ; i++){
		var item = document.createElement('li');
		item.appendChild(document.createTextNode("item",i));
		
		//获取每次循环的i,j是块级域
		let j = i;
		item.onclick = function(){
			console.log("item"+j+"clicked");
		};
		
		list.appendChild(item);
	}
	
#### let不属于window
> 在程序或者函数的顶层，let并不会像var一样在全局对象上创造一个属性

	var x = 1;
	let y = 1;
	console.log(window.x);	//1
	console.log(window.y); //undefined
	
#### 暂存死区（从块的开始到声明之前）
	
	function doing (){
		console.log(a);
		let a = 1;
	}
	//ReferenceError(引用错误) 对象表明一个不存在的变量被引用
	
#### 常见报错

> 同一块级作用域，声明同名变量

	if (x) {
		let a = 1;
		let a = 2; //Identifier 'a' has already been declared
	}
	
> switch语句报错，该语句只有一个作用块 

	switch (x) {
		case 0 :
			let a;
			break;
			
		case 1:
			let a;
			break;
			
		default : 
			let a;
			break;
	}
	//Identifier 'a' has already been declared
	
> let后跟一个函数传递的参数时将导致循环内部报错。

	function a (obj){
		for( let obj of obj.a){ //obj is not defined
			console.log(obj);
		}
	}
	a({a:[1,2,3]});
	
#### 块级作用域
> let或者const可以隐式的声明块级作用域
> 
> 块级作用域用花括号区分层级 {}
> 
> 块级作用域可以多层嵌套，互不干扰 {{{{...}}}}

#### 块级作用域与函数声明
ES5 ：函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数。

ES6 ：引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。

	//ES6推荐写法
	{
		let a = 1;
		let alertA = function(a){
			alert(a);
		}
	}
	
#### const
定义常量，初始化的时候必须赋值，且不能被改变。其他特性和let一致.

