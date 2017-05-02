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