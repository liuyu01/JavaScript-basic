- ##### JS对象属性

  JS有三种不同的属性：数据属性、访问器属性和内部属性。

  ###### 数据属性（properties）

  对象的普通属性将字符串名称映射到值。例如，下面对象obj有一个数据属性，名称为prop，对应的值为123

  ```
  var obj = {
  	prop : 123
  };
  ```

  可以用以下方式读取属性的值：

  ```
  consoe.log(obj.prop);//123
  console.log(obj['prop']);//123
  ```

  当然也可以用以下方式来设置属性的值：

  ```
  obj.prop = 'abc';
  obj['prop'] = 'abc';
  ```

  ###### 访问器属性

  l另外，可以通过函数处理获取和设置属性值。这些函数称为访问器函数。处理获取的函数称为getter。处理设置的函数称为setter。

  ```
  var obj = {
  	get prop (){
  		return 'Getter';
  	},
  	set prop (value){
  		console.log('Setter: ' + value);
  	}
  }
  ```

  访问obj属性

  ```
  obj.prop  //'Geter'
  obj.prop 123; //Setter: 123
  ```

  ###### 内部属性

  一些属性只是用于规范，这些属于‘内部’的内部，因为它们不能直接访问，但是它们确实影响对象的行为。内部属性有特殊的名称都写在两个方括号，如：

  - 内部属性[[Prototype]]指向对象的原型。它可以通过Object.getPrototypeof()读取。它的值只能通过创建具有给定原型的新对象来设置，例如通过object.create()或__ proto __ 。
  - 内部属性[[Extensible]]决定是否可以向对象添加属性。可以通过Object.isExtensibe()方法判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。可以通过Object.preventExtensions()方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。

  ##### 属性特性

  属性的所有状态，包括数据和元数据，都存储在特性（attribute）中。它们是属性具有的字段，就像对象具有属性一样。特性（attribute）键通常用双括号编写：

  ###### 以下特性是属于数据属性：

  [[Value]]：该属性的属性值，默认为undefined

  [[Writable]]：是一个布尔值，表示属性值（value）是否可改变（即是否可写），默认为true。

  ###### 以下特性是属于访问器属性：

  [[Get]]：是一个函数，表示该属性的取值函数（getter），默认为undefined

  [[Set]] ：是一个函数，表示该属性的存值函数（setter）,默认为udefined

  ###### 所有的属性都具有以下特性：

  [[Enumerable]]：是一个布尔值，表示该属性是否可遍历，默认为true。如果设为false，会使得某些操作（比如for...in循环、Object.keys()）跳过该属性。

  [[Configurable]]：是一个布尔值，表示可配置性，默认为true。如果设为false，将阻止某些操作改写该属性，比如无法删除该属性，也不得改变该属性的属性描述对象（value属性除外）。也就是说，configurable属性控制了属性描述对象的可写性。

