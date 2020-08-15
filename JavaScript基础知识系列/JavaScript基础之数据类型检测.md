## 数据类型

> 最新的ECMAScript标准定义了**8**种数据类型，包括7种原始类型和Object。

> 7种原始类型：Number，String，Boolean，Null，Undefined，Symbol，BigInt

## 检测数据类型的四种方法

### typeof 操作符

>  typeof 操作符返回一个字符串，表示未经计算的操作数的类型

   	var types1 = [
	  new Number,
	  1,
	  new String,
	  'str',
	  1n,
	  Symbol('id'),
	  undefined,
	  null,
	  {},
	  /a/,
	  []
	];
	for(let i = 0; i < types1.length; i++ ){
 	 	types1[i] = [
						'typeof',
						Object.prototype.toString.call(types1[i]), 
						'返回',
						typeof types1[i]
					].join('  ');
	}
	console.log(types1.join('\n'))

> 打印结果：
 
	typeof  [object Number]  返回  object
	typeof  [object Number]  返回  number
	typeof  [object String]  返回  object
	typeof  [object String]  返回  string
	typeof  [object BigInt]  返回  bigint
	typeof  [object Symbol]  返回  symbol
	typeof  [object Undefined]  返回  undefined
	typeof  [object Null]  返回  object
	typeof  [object Object]  返回  object
	typeof  [object RegExp]  返回  object
	typeof  [object Array]  返回  object

> 总结：typeof只能检测出原始类型字面量值的数据类型，其他检查对象都返回 object。

### instanceof 运算符
   
> instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。
	
	
	console.log( new String instanceof String );	// true
	console.log( '' instanceof String); 			// false，检查原型链会找到 undefined
	console.log( new Number instanceof Number);		// true
	console.log( 1 instanceof Number);  			// false
	console.log( new Boolean instanceof Boolean); 	// true
	console.log( true instanceof Boolean); 	 		// false
	console.log( 1n instanceof BigInt); 			// false
	console.log( Symbol('i') instanceof Symbol);	// false
	console.log( [] instanceof Array);  			// true
	console.log( {} instanceof Object); 			// true
	console.log( /ab+c/ instanceof RegExp);			// true
	// console.log( null instanceof Null);  		// Null is not defined;
	// console.log( undefined instanceof Undefined);// Undefined is not defined

> 总结：
> 
> 1. instanceof 不能检测原始类型字面量值的数据类型
> 2. 若对象的构造函数的 prototype 属性指向改变 则 检测结果不准

### constructor 属性
> 返回创建实例对象的 Object 构造函数的引用。
> 注意，此属性的值是对函数本身的引用，而不是一个包含函数名称的字符串。
> 对原始类型来说，如1，true和"test"，该值只可读。
	
	var types2 = [
	  new Number,
	  1,
	  new String,
	  'str',
	  1n,
	  Symbol('id'),
	// undefined,
	// null,
	  {},
	  /a/,
	  []
	];
	for(let i = 0; i < types2.length; i++ ){
 	 	types2[i] = [
						Object.prototype.toString.call(types2[i]), 
     			 		'.constructor',
						 '  返回  ',
						 types2[i].constructor.name
					].join('');
	}
	console.log(types2.join('\n'))
	
> 打印结果：

	[object Number].constructor  返回  Number
	[object Number].constructor  返回  Number
	[object String].constructor  返回  String
	[object String].constructor  返回  String
	[object BigInt].constructor  返回  BigInt
	[object Symbol].constructor  返回  Symbol
	[object Object].constructor  返回  Object
	[object RegExp].constructor  返回  RegExp
	[object Array].constructor  返回  Array

> 对象的 constructor 属性是可以改的，改了之后结果如下：

	// 自定义一个构造函数
	function Type() { };
	var types2 = [
	  new Number,
	  1,
	  new String,
	  'str',
	  1n,
	  Symbol('id'),
	// undefined,
	// null,
	  {},
	  /a/,
	  []
	];
	for(let i = 0; i < types2.length; i++ ){
		// 改变检测数据的constructor指向
    	types2[i].constructor = Type;
 		types2[i] = [
				Object.prototype.toString.call(types2[i]), 
				'instanceof Type 返回',
			 	types2[i] instanceof Type,
				'construcor 指向',
				 types2[i].constructor.name
			].join('  ');
	}
	console.log(types2.join('\n'));

> 打印结果：

	[object Number]  instanceof Type 返回  false  construcor 指向  Type
	[object Number]  instanceof Type 返回  false  construcor 指向  Number
	[object String]  instanceof Type 返回  false  construcor 指向  Type
	[object String]  instanceof Type 返回  false  construcor 指向  String
	[object BigInt]  instanceof Type 返回  false  construcor 指向  BigInt
	[object Symbol]  instanceof Type 返回  false  construcor 指向  Symbol
	[object Object]  instanceof Type 返回  false  construcor 指向  Type
	[object RegExp]  instanceof Type 返回  false  construcor 指向  Type
	[object Array]  instanceof Type 返回  false  construcor 指向  Type

> 总结： 
> 
> 1. 只有原始类型不受影响；
> 2. 即使更改了对象的 constructor 指向为 新构造函数，新构造函数 的 prototype 属性也不在该对象的原型链上；
> 3. 依靠 constructor 属性判断数据类型不靠谱。


### Object.prototype.toString.call()

> 上文中为了让打印结果更加明朗已经用过该方法

	var types3 = [
	  new Number,
	  1,
	  new String,
	  'str',
	  1n,
	  Symbol('id'),
	  undefined,
	  null,
	  {},
	  /a/,
	  []
	];
	for(let i = 0; i < types3.length; i++ ){
 	 	types3[i] = [
      					'返回',
						Object.prototype.toString.call(types3[i]), 
					].join('  ');
	}
	console.log(types3.join('\n'))

> 打印结果：

	返回  [object Number]
	返回  [object Number]
	返回  [object String]
	返回  [object String]
	返回  [object BigInt]
	返回  [object Symbol]
	返回  [object Undefined]
	返回  [object Null]
	返回  [object Object]
	返回  [object RegExp]
	返回  [object Array]

> 若改动构造函数的 prototype：

	function Fun(){}
	Fun.prototype.old = 'old'

	var f1 = new Fun();

	console.log(f1 instanceof Fun);						// true
	console.log(f1 instanceof Array);					// false
	console.log(Object.prototype.toString.call(Fun))	// [object Function]
	console.log(Object.prototype.toString.call(f1))		// [object Object]
	
	Fun.prototype = Array.prototype;
	
	console.log(f1 instanceof Fun);						// false
	console.log(f1 instanceof Array);					// false
	console.log(Object.prototype.toString.call(Fun))	// [object Function]
	console.log(Object.prototype.toString.call(f1))		// [object Object]

	var f2 = new Fun();
	
	console.log(f1.old)			// old
 	console.log(f2.old)			// undefined
	console.log(f2 instanceof Fun);						// true
	console.log(f2 instanceof Array);					// true
	console.log(Object.prototype.toString.call(Fun))	// [object Function]
	console.log(Object.prototype.toString.call(f2))		// [object Object]


> 若改动实例对象的 constructor 属性：

	var str = new String('str');
  	str.constructor = function Type2(){};
  	console.log(Object.prototype.toString.call(str));	// [object String]
  	console.log(str.constructor.name);	// Type2

> 总结：Object.prototype.toString.call()检测结果不会受影响
	
