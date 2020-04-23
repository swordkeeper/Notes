# Class

JavaScript 中的class

#### 声明

```javascript
class A{
    
}
```



#### 构造函数

````javascript
class Car{
    constructor(){
        console.log("构造函数，会在类一开始时，执行")
    }
}
let my_car = new Car()
````



#### 静态属性/方法

```javascript
class Car{
    constructor(){}
    static year = "1999";
	static repair(car){
        console.log("car",  car);
    }
}

Car.repair("我的车");
console.log(Car.year);
```



#### get/set

```javascript
const obj = {
    _name:'',
    set name(val){  // set 为关键字 
        this._name = val;
    }
    get name(){
        return this._name;
    }
}
// 或者
Object.defineProperty(obj, 'name', {  
    enumerable:true,
    get: () => {
    	return this._name;
    },
    set: (val) => {
        this._name = val;
    }
})

// 或者   类类型的get/set
class Persion{
    constructor(){
        this._name="";
    }
    get name(){
        return this._name;
    }
    set name(val){
        this._name = val;
    }
}
```



#### 继承

```javascript
class Human{
    constructor(name, age , gender, hobby){
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.hobby = hobby;
    }
    eat(){
        console.log(`${this.name} are eating`);
    }
}

class Engineer extends Human{
    constructor(name, age , gender, hobby, skill, salary){
        super();
        this.skill = skill;
        this.salary = salary;
    }
}
let myFather = new Engineer("xxx",11,"male", "fighting", "building","20K");
```

