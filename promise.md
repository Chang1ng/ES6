### promise

### 是什么？
> ES6提供的异步编程操作解决方案，promise对象。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。


### 特点

> 1.对象的状态不受外界影响。promise的三种状态分别是pending（进行中），resolved（已完成），rejected（已失败）。只有异步操作的结果可以决定是哪一种状态，其他操作无法改变状态。
> 
> 2.一旦状态改变，就不会再变，任何时候都可以得到这个结果。
>
> 状态的改变路径只有两种:
>
> pending->resolved
> pending->rejected
> 状态改变后，无法再次改变。有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。

### 缺点
1 无法取消（一旦创建了promise对象就会执行）

2 如果不设置回调函数，promise内部错误不会反应到外部 

3 pending状态时不知道具体阶段（刚开始？快结束？）

### 基本用法
	//1.新建一个Promise对象
	//Promise对象接受一个fn作为参数
	//resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
	var promise = new Promise(function(resolve,reject){
		//do something
		if(/*成功*/){
			resolve(value);
		}else{
			reject(error);
		}
	});
	
	//2.指定回调函数
	//error回调函数可选
	promise.then(function(value){
		//success
	},function(error){
		//error
	});
	
简单的例子1 （最基本用法）

	//makeP方法返回一个Promise实例，表示一段时间以后才会发生的结果。过了指定的时间（time参数）以后，Promise实例的状态变为Resolved，就会触发then方法绑定的回调函数。
	function makeP(time){
		return new Promise((resolve,reject) => {
			setTimeout(resolve,time,'done');
		});
	}
	makeP(100).then(function(value){
		console.log(value);
	});
	
简单的例子2 （执行顺序）

	//Promise新建后立即执行，所以首先输出的是“promised”。然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以“resolved”最后输出。
	let promise = new Promise(function(resolve,reject){
		console.log('promised');
		resolve();
	})
	
	promise.then(function(){
		console.log('resolved');
	});
	
	console.log('lalaland');

简单的例子3 （异步加载图片）
	
	function loadImg(url){
		return new Promise(function(resolve,reject){
			
			var img = new Image();
			img.onload = function(){
				resolve(img);
			}
			img.onerror = function(){
				reject( new Error('加载图片失败' + url) );
			}
			
			img.src = url;
			
		});
	}
	
简单的例子4 （异步ajax Promise）

	function ajax (url){
		var promise = new Promise(resolve,reject){
			//创建xmlhttp对象
			var client = new XMLHttpRequest();
			client.open("GET",url);
			
			client.onreadystatechange = handler;
			
			client.responseType = json;
			
			client.setRequestHeader("Accept","application/json");
			
			client.send();
			
			function handler(){
				if(this.readyState !== 4){
					return;
				}
				if(this.status === 200){
					resolve(this.response);	
				}else{
					reject(new Error(this.statusText));
				}
			}
			
		};
		
		return promise;
	}
	
	ajax('xxx.php').then(function(json){
		console.log(json);
	},function(error){
		console.log(error);
	});


	