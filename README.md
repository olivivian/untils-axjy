- [ 导航](#head1)
- [ 使用方法](#head2)
- [ 方法类](#head3)
	- [ 根据pid生成树形结构](#head4)
	- [ 根据时间生成树形结构](#head5)
	- [ 寻找所有子节点](#head6)
	- [ 修改数组里对象的key](#head7)
	- [ 加减乘除](#head8)
	- [根据对象里的属性 重构为数组](#head9)
	- [ 根据对象的某个属性，找到数组里的对象](#head10)
	- [树结构 根据id获取name](#head11)
	- [ 字符串转对象](#head12)
	- [ 数组对象值转对象](#head13)
	- [ 二维数组全排列组合转一维数组](#head14)
	- [ 数组|对象深拷贝](#head15)
	- [ 数组对象去重](#head16)
	- [ 替换指定位置的字符串](#head17)
	- [ 数量排序](#head18)
	- [ 函数节流](#head19)
	- [ 函数防抖](#head20)
	- [ 对象字符串的值转字符串](#head21)
	- [ 去掉字符串最后的逗号](#head22)
- [ 校验类](#head23)
	- [ 判断对象指定属性是否存在](#head24)
	- [ 比较两个数据类型是否相同](#head25)
	- [ 是否为空](#head26)
	- [ 是否是字符串](#head27)
	- [ 是否是数组](#head28)
	- [ 是否是对象](#head29)
	- [ 是否是时间类型](#head30)
	- [ 是否是数字](#head31)
	- [ 是否是布尔值](#head32)
	- [ 是否是方法](#head33)
	- [ 邮箱校验](#head34)
	- [ 手机号校验](#head35)
	- [ 校验字符串是否是url](#head36)
	- [ 检验输入框大于等于0](#head37)
- [ 工具类](#head38)
	- [ PC端判断](#head39)
	- [ 数字格式化（给数字加逗号）](#head40)
	- [ downFile文件下载](#head41)
	- [ 生成随机数](#head42)
	- [ 复制](#head43)
	- [ loadScript](#head44)
	- [ 历史记录](#head45)
	- [ 防抖](#head46)
	- [ 获取地址栏参数](#head47)
	- [ base64转换为文件](#head48)
	- [ 图片转换为base64](#head49)
	- [ uuid生成唯一值](#head50)
	- [ 生成一个hash字符串](#head51)
	- [ 生成随机字符串](#head52)
	- [ 大小写转换或首字母](#head53)
	- [ 将阿拉伯数字翻译成中文的大写数字](#head54)
	- [ 将数字转换为大写金额](#head55)
	- [ 富文本代码块右上角显示复制按钮](#head56)
	- [ 图片懒加载](#head57)
	- [ 计算字符串长度（中英文）](#head58)
	- [ 判断链接是否为本地](#head59)
	- [ 获取当前url上的参数](#head60)
	- [ 处理二进制流文件](#head61)
- [ 时间类](#head62)
	- [ 获取当前周几](#head63)
	- [ 时间问候语](#head64)
	- [ Date时间转数字](#head65)
	- [ Date时间转汉字](#head66)
	- [ 获取某月有多少天](#head67)
	- [ 获取某年的第一天](#head68)
	- [ 获取某年最后一天](#head69)
	- [ 获取某个日期是当年中的第几天](#head70)
	- [ 获取某个日期在这一年的第几周](#head71)
	- [ 把秒转化为天小时分钟秒](#head72)
- [ DOM类](#head73)
	- [ 获取元素](#head74)
	- [ 跳转到某个元素](#head75)
	- [ 切换元素](#head76)
	- [ 获取兄弟节点](#head77)
	- [ 判断是否存在Class](#head78)
	- [ 添加Class](#head79)
	- [ 删除Class](#head80)
	- [ 创建a标签打开新页面](#head81)
## <span id="head1"> 导航</span>


## <span id="head2"> 使用方法</span>

```
import { 方法名称 } from "@/utils";
```

## <span id="head3"> 方法类</span>

> 包含了数组、字符串、对象的操作方法

### <span id="head4"> 根据pid生成树形结构</span>

```js
/**
 * 根据pid生成树形结构
 *  @param { object } items 后台获取的数据
 *  @param { * } id 数据中的id
 *  @param { * } link 生成树形结构的依据
 */
export const createTree = (items, id = null, link = 'pid') =>{
    items.filter(item => item[link] === id).map(item => ({ ...item, children: createTree(items, item.id) }));
};
```

### <span id="head5"> 根据时间生成树形结构</span>

```js
// 原始数据
let data = [
  {
      created_at: '"2022-01-24 18:25:43',
      title: 'title'
  }
]

// 想要的结果
let newData = [
  {
      time: '2022',
      lists: [
        {
          created_at: '"2022-01-24 18:25:43',
          title: 'title'  
        }
      ]
  }
]
```

```js
/**
 * 列表用时间生成树
 * @param data
 * @returns {{lists: *, time: string}[]}
 */
export function timeTree(data) {
	const group = data.reduce((obj, item) => {
		const year = item.created_at.substring(0, 4) // 2021-01-24 18:25:43 变为 2021
		if (!obj[year]) obj[year] = []
		obj[year].push(item)
		return obj
	}, {})
	return Object.keys(group).map(key => ({ time: key, lists: group[key] }))
}
```

### <span id="head6"> 寻找所有子节点</span>

```js
/**
 * 寻找所有子节点
 * @param id
 * @param data
 * @param pidName 父级键名
 * @param idName 子级自己的id
 * @param childrenName 包含子级的数组键名
 * @returns {[]}
 */
export function traceChildNode(id, data, pidName = 'parentId', idName = 'id', childrenName = 'children') {
    let arr = [];
    foreachTree(data, childrenName, (node) => {
        if (node[pidName] == id) {
            arr.push(node);
            arr = arr.concat(traceChildNode(node[idName], data, pidName, idName, childrenName));
        }
    });
    return arr;
}
```



### <span id="head7"> 修改数组里对象的key</span>

别名：修改数组里对象的键名

```js
data:[{"label":"标签","value":"值"}]
// 使用后
newData: [{"name":"标签","code":"值"}]
```

```js
/**
 * 修改数组里对象的Key和值
 * @param arr
 * @returns {*}
 */
export function convertKey(arr) {
    return arr.map(item => ({
        label: item.name,
        value: item.code
    }))
}
```

### <span id="head8"> 加减乘除</span>

> 解决精度丢失问题

```js
/**
 * @desc 加法函数（精度丢失问题）
 * @param { number } arg1
 * @param { number } arg2
 */
export function add(arg1, arg2) {
    let r1, r2, m;
    try { r1 = arg1.toString().split(".")[1].length } catch (e) { r1 = 0 }
    try { r2 = arg2.toString().split(".")[1].length } catch (e) { r2 = 0 }
    m = Math.pow(10, Math.max(r1, r2));
    return (arg1 * m + arg2 * m) / m
}
```

```js
/**
 * @desc 减法函数（精度丢失问题）
 * @param { number } arg1
 * @param { number } arg2
 */
export function sub(arg1, arg2) {
    let r1, r2, m, n;
    try { r1 = arg1.toString().split(".")[1].length } catch (e) { r1 = 0 }
    try { r2 = arg2.toString().split(".")[1].length } catch (e) { r2 = 0 }
    m = Math.pow(10, Math.max(r1, r2));
    n = (r1 >= r2) ? r1 : r2;
    return Number(((arg1 * m - arg2 * m) / m).toFixed(n));
}
```

```js
/**
 * @desc 乘法函数（精度丢失问题）
 * @param { number } num1
 * @param { number } num2
 */
export function mcl(num1,num2){
    let m=0,s1=num1.toString(),s2=num2.toString();
    try{m+=s1.split(".")[1].length}catch(e){}
    try{m+=s2.split(".")[1].length}catch(e){}
    return Number(s1.replace(".",""))*Number(s2.replace(".",""))/Math.pow(10,m);
}
```

```js
/**
 * @desc 除法函数（精度丢失问题）
 * @param { number } num1
 * @param { number } num2
 */
export function division(num1,num2){
    let t1,t2,r1,r2;
    try{
        t1 = num1.toString().split('.')[1].length;
    }catch(e){
        t1 = 0;
    }
    try{
        t2=num2.toString().split(".")[1].length;
    }catch(e){
        t2=0;
    }
    r1=Number(num1.toString().replace(".",""));
    r2=Number(num2.toString().replace(".",""));
    return (r1/r2)*Math.pow(10,t2-t1);
}
```



### <span id="head9">根据对象里的属性 重构为数组</span>

```

let chartData =  [{"day":"2022-11-17","num":0},{"day":"2022-11-18","num":0},{"day":"2022-11-19","num":0}]

objToArr(chartData, 'day') //['2022-11-17', '2022-11-18', '2022-11-19']
```



```
/*
* @desc 获取对象里的属性 重构为数组
* @param {Array} objArr
* @param {string} field
* @returns {Array}
*/
export const objToArr = (objArr, field) => {
  const newArr = []
  objArr.forEach((item) => {
    newArr.push(item[field])
  })
  return newArr
}
```



### <span id="head10"> 根据对象的某个属性，找到数组里的对象</span>

```js
  let objArr = [{"id":1,"name":"a"},{"id":2,"name":"b"}]
  getListObj(objArr,1,'id') //{id: 1, name: 'a'}
```



````js
  /*
  * @desc  列表根据属性值找对象
  * @param {Array} list
  * @param {string|number} val
  * @param {string} fieldName
  * @returns {Object}
  */
  export function getListObj (list, val, fieldName) {
    let result = {}

    list.forEach((item) => {
      if (item[fieldName] === val) {
        result = item
      }
    })

    return result
  }
````



### <span id="head11">树结构 根据id获取name</span>

```js
let objArr = [{"child":[{"child":[{"id":80,"name":"三级分类"}],"id":12,"name":"二级分类"}],"id":13,"name":"一级分类"},{"child":[{"child":[{"child":[{"id":72,"name":"奶茶分类"}],"id":24,"name":"咖啡分类"},{"id":2,"name":"甜品分类"}],"id":20,"name":"吃货分类"}],"id":4,"name":"甜点饮品"}]

   console.log(getTreeName(objArr,72,'id','name')) //奶茶分类
```



```js
/*
* @desc  树结构根据id找name
* @param {Array} list
* @param  val
* @param {string} valField
* @param {string} resultField
* @returns 值
*/
  export function getTreeName(list, val,valField,resultField) {
    for (let i = 0; i < list.length; i++) {
      const a = list[i]
      if (a[valField] === val) {
        return a[resultField]
      } else {
        if (a.child && a.child.length > 0) {
          const res = getTreeName(a.child, val,valField,resultField)
          if (res) {
            return res
          }
        }
      }
    }
  }
```



### <span id="head12"> 字符串转对象</span>

```
  let Str = '规格A;规格B;规格C'
  
  stringToOptionObject(Str,'label','value') 
  //[{"label":"规格A","value":"规格A"},{"label":"规格B","value":"规格B"},{"label":"规格C","value":"规格C"}]
```



```js
  /*
    * @desc  字符串转对象
    * @param {string} string
    * @param {string} labelName
    * @param {string} valueName
    * @returns {Array}
    */
  export function stringToOptionObject(str,labelName,valueName) {
    const arr = str.split(';')
    const arrObjArr = []
    arr.forEach((item) => {
      const obj = {}
      obj[labelName] = item
      obj[valueName] = item
      arrObjArr.push(obj)
    })
    return arrObjArr
  }
```



### <span id="head13"> 数组对象值转对象</span>

```js
let arr = [{"id":0,"status":"a"},{"id":155,"status":"b"},{"id":283,"status":"c"},{"id":88,"status":"d"}]

arrOptionToObject(arr,'status','id')//{"a":0,"b":155,"c":283,"d":88}
```



```js
/*
    * @desc  数组对象值转对象
    * @param {Array} arr
    * @param {string} key  新对象到的key
    * @param {string} val  新对象到的值
    * @returns {Object}
    */
  export function arrOptionToObject(arr, key, val) {
    const obj = {}
    arr.forEach((item) => {
      obj[item[key]] = item[val]
    })
    return obj
  }
```



### <span id="head14"> 二维数组全排列组合转一维数组</span>

```js
  let arr2D = [["规格:大号-U"],["颜色:红色","颜色:黄色"]]
  
  doCombination(arr2D)//["规格:大号-U,颜色:红色","规格:大号-U,颜色:黄色"]
```



```js
 /*
    * @desc  数组全排列组合
    * @param {Array} dArray:二维数组
    * @returns {Array} 一维数组
    */
   function doCombination(dArray) {
    if (dArray.length > 0) {
      return dArray.reduce((last, el) => {
        const arr = []
        // last：上次运算结果
        // el：数组中的当前元素
        last.forEach(e1 => {
          el.forEach(e2 => {
            arr.push(e1 + ',' + e2)
          })
        })
        return arr
      })
    }
    return []
  }
```



### <span id="head15"> 数组|对象深拷贝</span>

```js
/**
 * 深拷贝的简单版本，在一些边缘性情况会有bug，如果想要完美的深拷贝可以引入lodash的  _.cloneDeep
 * @param {Object} source
 * @returns {Object}
 */
export function deepClone(source) {
  if (!source && typeof source !== 'object') {
    throw new Error('error arguments', 'deepClone')
  }
  const targetObj = source.constructor === Array ? [] : {}
  Object.keys(source).forEach(keys => {
    if (source[keys] && typeof source[keys] === 'object') {
      targetObj[keys] = deepClone(source[keys])
    } else {
      targetObj[keys] = source[keys]
    }
  })
  return targetObj
}
```



### <span id="head16"> 数组对象去重</span>

```js
/*数组对象去重*/
uniqueObjArr(arr,Field) {
    const res = new Map();
    return arr.filter((a) => !res.has(a[Field]) && res.set(a[Field], 1))
}
```





### <span id="head17"> 替换指定位置的字符串</span>

```js
let str = '1300011'
changeStr(str,str.length-2,'00') //1300000  把字符串后两位替换掉
```



```js
    /*
    * 替换指定位置的字符串
    * @param {string} str 原始字符串
    * @param {number} index 开始位置
    * @param {string} changeStr 改变后的字
    * @returns {string} 
    */
    export function changeStr(str,index,changeStr){
      return str.substr(0, index) + changeStr+ str.substr(index + changeStr.length);
    }
```



### <span id="head18"> 数量排序</span>



### <span id="head19"> 函数节流</span>

```js
   demoFun:throttle(function (e) {
    //...业务逻辑
    }, 1000)
```



```js
/*
* @desc 节流器函数，避免函数在小于规定时间范围内多次执行, 减少函数的触发频率
* @param {Function} fn 目标函数
* @param {?Number} interval 规定限制时间，单位为ms，缺省值为300
* @returns {Function} 经节流器处理过的函数
*/
export const  throttle = function (fn, interval) {
  var enterTime = 0;//触发的时间
  var gapTime = interval || 300 ;//间隔时间，如果interval不传，则默认300ms
  return function() {
    var context = this;
    var backTime = new Date();//第一次函数return即触发的时间
    if (backTime - enterTime > gapTime) {
      fn.call(context,...arguments);
      enterTime = backTime;//赋值给第一次触发的时间，这样就保存了第二次触发的时间
    }
  };
}
```



### <span id="head20"> 函数防抖</span>

```js
gotoUnlock: debounce(function() {
      //...业务逻辑
}),
```



```js
/*
* @desc 防抖函数，规定函数延迟指定时间执行，如果在这段时间内再次调用。则以函数执行时间往后顺延(不管触发多少次都只执行最后一次,防止按钮重复点击)
* @param {Function} fn 目标函数
* @param {?Number} interval 规定限制时间，单位为ms，缺省值为500
* @returns {Function} 经处理过的函数
*/
export const  debounce = function debounce(fn, interval) {
  var timer;
  var gapTime = interval || 300;//间隔时间，如果interval不传，则默认1000ms
  return function() {
    clearTimeout(timer);
    var context = this;
    var args = arguments;//保存此处的arguments，因为setTimeout是全局的，arguments不是防抖函数需要的。
    timer = setTimeout(function() {
      fn.call(context,...args);
    }, gapTime);
  };
}
```



### <span id="head21"> 对象字符串的值转字符串</span>

```js
ObjStringDeal('{"颜色":"红色","规格":"大号-U"}') //红色,大号-U
```



```js
    /*
   * 对象字符串值转字符串
   * @param {string} ObjString
   * @returns {string}
   */
     const ObjStringDeal = (ObjString) =>{
      let ObjStringJson = JSON.parse(ObjString);
      let ObjValues = Object.values(ObjStringJson);
      let ObjStringSting = '';

       ObjValues.forEach((item)=>{
        ObjStringSting = ObjStringSting + item + ',';
      })
      //去掉最后的逗号
      ObjStringSting = ObjStringSting.substring(0, ObjStringSting.lastIndexOf(','));

      return ObjStringSting
    }
```



### <span id="head22"> 去掉字符串最后的逗号</span>

```js
strRemoveDot('abcd,') //abcd
```



```js
  /**
   * 去掉字符串最后的逗号
   * @param {string} str 带逗号的字符串
   * @returns {string}
   */
  function strRemoveDot (str){
    return  str.substring(0, str.lastIndexOf(','));
  }
```



## <span id="head23"> 校验类</span>

### <span id="head24"> 判断对象指定属性是否存在</span>

```js
let obj = {"id":1,"name":"1"};

hasProperty(obj,'id') //true
hasProperty(obj,'ids') //false
```



```js
  /**
   * @desc 判断一个对象是否包含某个/些属性
   * @param {Object} obj 目标对象
   * @param {String} propStr 属性表达式字符串，如"prop"、"prop1.prop2.prop3"
   * @returns {Boolean} 判断结果
   */
  function hasProperty(obj, propStr){
    if(!obj || typeof(propStr) !== 'string') return false;
    let res = false,
      tmpObj = obj;
    propStr.split(/\./g).forEach(p => {
      res = false;
      if(p in tmpObj){
        res = true;
        tmpObj = tmpObj[p];
      }
    });
    return res;
  }
```



### <span id="head25"> 比较两个数据类型是否相同</span>

```js
sameType(6,'aaa') //false
sameType(6,8) //true
```



```js
  /**
   * 比较两个数据类型是否相同
   * @param {any} arg1 待比较的第一个参数
   * @param {any} arg2 待比较的第二个参数
   */
  function sameType(arg1, arg2){
    if(arg1 === null || arg1 === undefined){
      if(arg1 === arg2) return true;
      return false;
    };
    if(arg2 === null || arg2 === undefined) return false;
    return (arg1 === arg2) || (arg1.constructor === arg2.constructor);
  }
```





### <span id="head26"> 是否为空</span>

```js
/**
 * @desc 是否为空判断
 * @param val
 * @returns {boolean}
 */
export function isNull(val) {
  if (val instanceof Array) {
    if (val.length === 0) return true
  } else if (val instanceof Object) {
    if (JSON.stringify(val) === '{}') return true
  } else {
    if (val === 'null' || val === null || val === 'undefined' || val === undefined || val === '') return true
    return false
  }
  return false
}
```



### <span id="head27"> 是否是字符串</span>

```js
/**
 * @desc 是否是字符串
 * @param str
 * @returns {boolean}
 */
export function isString(str) {
    return str instanceof String || Object.prototype.toString.call(str) === '[object String]';
}
```



### <span id="head28"> 是否是数组</span>

```js
/**
 * @desc 是否是数组
 * @param {Array} arg
 * @returns {Boolean}
 */
export function isArray(arg) {
  if (typeof Array.isArray === 'undefined') {
    return Object.prototype.toString.call(arg) === '[object Array]'
  }
  return Array.isArray(arg)
}

```



### <span id="head29"> 是否是对象</span>

```js
/**
 * @desc 是否是对象
 * @param input
 * @returns {boolean}
 */
export function isObject(input) {
    return Object.prototype.toString.call(input) === '[object Object]';
}
```



### <span id="head30"> 是否是时间类型</span>

```js
/**
 * @desc 是否是时间类型
 * @param input
 * @returns {boolean}
 */
export function isDate(input) {
    return input instanceof Date || Object.prototype.toString.call(input) === '[object Date]';
}
```



### <span id="head31"> 是否是数字</span>

```js
/**
 * @desc 是否是数字
 * @param input
 * @returns {boolean}
 */
export function isNumber(input) {
    return input instanceof Number || Object.prototype.toString.call(input) === '[object Number]';
}
```



### <span id="head32"> 是否是布尔值</span>

```js
/**
 * 是否是布尔值
 * @param input
 * @returns {boolean}
 */
export function isBoolean(input) {
    return typeof input == 'boolean';
}
```



### <span id="head33"> 是否是方法</span>

```js
/**
 * 是否是方法
 * @param input
 * @returns {boolean}
 */
export function isFunction(input) {
    return typeof input == 'function';
}
```



### <span id="head34"> 邮箱校验</span>

```js
/**
 * @desc  检测字符串是否为邮箱地址
 * @param {string} email
 * @returns {Boolean}
 */
export function validEmail(email) {
  const reg = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
  return reg.test(email)
}
```



### <span id="head35"> 手机号校验</span>

```js
/**
 * @desc 校验字符串是否为手机号码
 * @param {String} str 待判断的字符串
 * @returns {Boolean} 判断结果
*/
export function isMobileNo (str) {
    if(!str || typeof(str) !== 'string') return false;
    return /^1[3-9]\d{9}$/.test(str);
}
```



### <span id="head36"> 校验字符串是否是url</span>

```js
/**
 * @desc 校验字符串是否为url
 * @param {String} str 待判断的字符串
 * @returns {Boolean} 判断结果
*/
function isUrl (str) {
    if(!str || typeof(str) !== 'string') return false;
    return /^https?\:\/\/.+$/i.test(str);
}
```



### <span id="head37"> 检验输入框大于等于0</span>

```js
/*
* @desc 输入框大于等于0
* @param {number} val
* @param {Boolean} flag  true-可以为0
* @returns {Boolean}
*/
export const checkInput = (val, flag) => {
  val = parseFloat(val)
  if (!val || val <= 0) {
    if (flag === true && val === 0) {
      return false
    } else {
      return true
    }
  }
  return false
}
```



## <span id="head38"> 工具类</span>

### <span id="head39"> PC端判断</span>

```js
if (isPc) alert('这是PC端');
```

```js
/**
 * PC端判断
 * @returns {boolean}
 */
export function isPc() {
    let userAgentInfo = navigator.userAgent;
    let Agents = new Array("Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod");
    let flag = true;
    for (let v = 0; v < Agents.length; v++) {
        if (userAgentInfo.indexOf(Agents[v]) > 0) {
            flag = false;
            break
        }
    }
    return flag;
}
```



### <span id="head40"> 数字格式化（给数字加逗号）</span>

```js
formatDecimal(12000000.11) //12,000,000.11
formatDecimal(12000000.11,false) //12,000,000
```



```js
  /**
   * @desc 数字格式化，如：1234567.89 -> 1,234,567.89
   * @param {Number} num 待格式化的数字，小数或整数
   * @param {?Boolean} fixed 是否保留小数，缺省值为true
   * @returns {String} 格式化后的字符串
   */
  function formatDecimal (num, fixed) {
    if(typeof(num) !== 'number'){
      throw Error('parameter "num" must be a number!');
    }
    fixed = fixed === undefined ? true : fixed;
    let numStr = fixed ? (Math.round(num * 100) / 100).toFixed(2) : Math.round(num).toString();
    return numStr.replace(/(\d{1,3})(?=(\d{3})+\b)/g, '$1,');
  }
```



### <span id="head41"> downFile文件下载</span>

别名：前端下载图片

```js
let urlVal = "https://avuejs.com/images/logo-bg.jpg";
let data = {
    url: urlVal,
    width: 300,
    height: 300,
    urlName: '图片名字',
}
downLoadImgByUrl(data)
```

```js
/**
 * 下载图片到本地，可适用于移动端
 */
 const downLoadImgByUrl = ({ url, width, height, urlName }) => {
    let canvas = document.createElement('CANVAS')
    let ctx = canvas.getContext('2d')
    let img = new Image()
    img.crossOrigin = 'Anonymous'
    img.onload = function () {
        let ResWidth
        let ResHeight

        if (width && height) {
            ResWidth = width
            ResHeight = height
        } else if (width) {
            ResWidth = width
            ResHeight = img.height * (width / img.width)
        } else if (height) {
            ResHeight = height
            ResWidth = img.width * (height / img.height)
        } else {
            ResWidth = img.width
            ResHeight = img.height
        }
        canvas.width = ResWidth
        canvas.height = ResHeight
        console.log(ResWidth, ResHeight)
        ctx.drawImage(img, 0, 0, ResWidth, ResHeight)

        let saveA = document.createElement('a')
        document.body.appendChild(saveA)
        saveA.href = canvas.toDataURL('image/jpeg', 1)
        saveA.download = urlName?urlName:'mypic' + new Date().getTime()
        saveA.target = '_blank'
        saveA.click()
        canvas = null
    }
    img.src = url
}
```

### <span id="head42"> 生成随机数</span>

```js
randomString(5) // ZsAl6
```

```js
/**
 * 生成随机数
 * @param len 生成的长度
 * @returns {string}
 */
export const randomString = (len) => {
        len = len || 32;
        let chars = 'ABCDEFGHJKMNPQRSTWXYZabcdefhijkmnprstwxyz2345678';
        let maxPos = chars.length;
        let pwd = '';
        for (let i = 0; i < len; i++) {
            pwd += chars.charAt(Math.floor(Math.random() * maxPos));
        }
        return pwd;
    }
```

### <span id="head43"> 复制</span>

```js
copyToClip('复制文本')
```

```js
/**
 * 复制
 * @param content 内容
 * @param info 复制成功的文案
 */
export function copyToClip(content, info) {
    let aux = document.createElement('input');
    aux.setAttribute('value', content);
    document.body.appendChild(aux);
    aux.select();
    document.execCommand('copy');
    document.body.removeChild(aux);
    if (info == null) {
        alert('复制成功');
    } else {
        alert(info);
    }
}
```

### <span id="head44"> loadScript</span>

别名：加载脚本、加载js、加载css、加载资源

```js
loadScript('js', 'xxx.js').then(() => {
//执行后的方法
})
loadScript('css', 'xxx.css').then(() => {
//执行后的方法
})
```

```js
/**
 * loadScript
 * @param type 类型
 * @param url 资源地址
 * @returns {Promise<unknown>}
 */
export function loadScript(type = 'js', url) {
    let flag = false;
    return new Promise((resolve) => {
        const head = document.getElementsByTagName('head')[0];
        head.children.forEach(ele => {
            if ((ele.src || '').indexOf(url) !== -1) {
                flag = true;
                resolve();
            }
        });
        if (flag) return;
        let script;
        if (type === 'js') {
            script = document.createElement('script');
            script.type = 'text/javascript';
            script.src = url;
        } else if (type === 'css') {
            script = document.createElement('link');
            script.rel = 'stylesheet';
            script.type = 'text/css';
            script.href = url;
        }
        head.appendChild(script);
        script.onload = function () {
            resolve();
        };
    });
};
```

### <span id="head45"> 历史记录</span>

别名：搜索历史、最近浏览

```js
/**
 * 最近浏览
 * @param item 单个元素
 * item => {
 *   id: 40,
 *   name: '第五个元素'
 * }
 */
export function setHistoryList(item) {
    const newViewNum = 8; // 要显示几条
    let HistoryList = [];
    if (window.localStorage.getItem('HistoryList') && window.localStorage.getItem('HistoryList') !== '[""]') {
        let newHistoryList = JSON.parse(window.localStorage.getItem('HistoryList'));
        let newArr = HistoryList.concat(newHistoryList)
        if (newArr.length > 0) {
            newArr.map((vo, key) => {
                // 搜索记录单个id 和 当前点击的元素
                if (vo.id === item.id) {
                    newArr.splice(key, 1)
                }
            })
        }
        newArr.unshift(item)
        if (newArr.length > newViewNum) {
            newArr.pop();
        }
        window.localStorage.setItem('HistoryList', JSON.stringify(newArr))
    } else {
        HistoryList.push(item);
        window.localStorage.setItem('HistoryList', JSON.stringify(HistoryList))
    }
}
```

### <span id="head46"> 防抖</span>

```js
methods: {
    toggle:debounce(() => {
        console.log('防抖')
    }, 1000)
}
```

```js
/**
 * 防抖
 * @param func 方法名
 * @param wait 等待时间
 * @param immediate
 * @returns {function(...[*]=): *}
 */
export function debounce(func, wait, immediate) {
    let timeout, args, context, timestamp, result

    const later = function () {
        // 据上一次触发时间间隔
        const last = +new Date() - timestamp

        // 上次被包装函数被调用时间间隔 last 小于设定时间间隔 wait
        if (last < wait && last > 0) {
            timeout = setTimeout(later, wait - last)
        } else {
            timeout = null
            // 如果设定为immediate===true，因为开始边界已经调用过了此处无需调用
            if (!immediate) {
                result = func.apply(context, args)
                if (!timeout) context = args = null
            }
        }
    }

    return function (...args) {
        context = this
        timestamp = +new Date()
        const callNow = immediate && !timeout
        // 如果延时不存在，重新设定延时
        if (!timeout) timeout = setTimeout(later, wait)
        if (callNow) {
            result = func.apply(context, args)
            context = args = null
        }

        return result
    }
}
```

### <span id="head47"> 获取地址栏参数</span>

别名：获取问号后面的参数

```js
// http://localhost:8080/page?a=123
getQueryObject() // {a:123}
```

```js
/**
 * 获取地址栏的参数
 * @param url
 * @returns {{}}
 */
export function getQueryObject(url) {
    url = url == null ? window.location.href : url
    const search = url.substring(url.lastIndexOf('?') + 1)
    const obj = {}
    const reg = /([^?&=]+)=([^?&=]*)/g
    search.replace(reg, (rs, $1, $2) => {
        const name = decodeURIComponent($1)
        let val = decodeURIComponent($2)
        val = String(val)
        obj[name] = val
        return rs
    })
    return obj
}
```

### <span id="head48"> base64转换为文件</span>

```js
/**
 * base64转换为文件
 * @param url
 * @param filename
 * @returns {File}
 */
export function dataURLtoFile(url, filename) {
    let arr = url.split(",")
    let mime = arr[0].match(/:(.*?);/)[1]
    let bstr = atob(arr[1])
    let n = bstr.length
    let u8arr = new Uint8Array(n)
    while (n--) {
        u8arr[n] = bstr.charCodeAt(n)
    }
    return new File([u8arr], filename, {type: mime})
}
```

### <span id="head49"> 图片转换为base64</span>

```js
/**
 * 图片转换为base64
 * @param url
 * @param callback
 * @param outputFormat
 */
export function getImgToBase64(url, callback, outputFormat) {
    let canvas = document.createElement("canvas")
    let ctx = canvas.getContext("2d")
    let img = new Image()
    img.crossOrigin = "Anonymous"
    img.onload = function () {
        canvas.height = img.height
        canvas.width = img.width
        ctx.drawImage(img, 0, 0)
        let dataURL = canvas.toDataURL(outputFormat || "image/png")
        callback(dataURL)
        canvas = null
    }
    img.src = url
}
```

### <span id="head50"> uuid生成唯一值</span>

```js
console.log(uuid()) // dec79cc2-ecf7-ec28-4b34-678b60ab37da
```

```js
/**
 * 生成唯一值
 * @returns {string}
 */
export function uuid() {
    const s4 = () => {
        return Math.floor((1 + Math.random()) * 0x10000).toString(16).substring(1);
    };
    return s4() + s4() + '-' + s4() + '-' + s4() + '-' + s4() + '-' + s4() + s4() + s4();
}
```



### <span id="head51"> 生成一个hash字符串</span>

```js
getHash(6) //8cQQ80
getHash(12) //5Ia85xP0b4E6
```



```js
  /**
   * 生成一个hash字符串
   * @param {Number} length 获取的hash字符串的长度（不小于5）
   */
  export function getHash(length){
    let tempStr = '0123456789abcdefghijklmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
    let _mul = tempStr.length;
    length = (typeof(length) === 'number' && length > 5) ? length : 8;
    let str = Array(length).fill().map(v => tempStr[Math.floor(Math.random() * _mul)]);
    return str.join('');
  }
```





### <span id="head52"> 生成随机字符串</span>

```js
console.log(createUniqueString()) // njb9967469s0
```



```js
  /**
  * 生成随机字符串
   * @returns {string}
   */
   function createUniqueString() {
    const timestamp = +new Date() + ''
    const randomNum = parseInt((1 + Math.random()) * 65536) + ''
    return (+(randomNum + timestamp)).toString(32)
  }
```





### <span id="head53"> 大小写转换或首字母</span>

```js
changeBigSmallCase('Hello', 5); // hello 
```

```js
/**
 * @param str
 * @param type 1:首字母大写  2:首页母小写  3:大小写转换  4:全部大写  5:全部小写
 * @returns {string|*}
 */
export function changeBigSmallCase(str, type) {
    type = type || 4
    switch (type) {
        case 1:
            return str.replace(/\b\w+\b/g, function (word) {
                return word.substring(0, 1).toUpperCase() + word.substring(1).toLowerCase();

            });
        case 2:
            return str.replace(/\b\w+\b/g, function (word) {
                return word.substring(0, 1).toLowerCase() + word.substring(1).toUpperCase();
            });
        case 3:
            return str.split('').map(function (word) {
                if (/[a-z]/.test(word)) {
                    return word.toUpperCase();
                } else {
                    return word.toLowerCase()
                }
            }).join('')
        case 4:
            return str.toUpperCase();
        case 5:
            return str.toLowerCase();
        default:
            return str;
    }
}
```

### <span id="head54"> 将阿拉伯数字翻译成中文的大写数字</span>

```js
numberToChinese('10') // 十
```

```js
/**
 * 将阿拉伯数字翻译成中文的大写数字
 * @param num
 * @returns {string}
 */
export function numberToChinese(num) {
    let AA = new Array("零", "一", "二", "三", "四", "五", "六", "七", "八", "九", "十");
    let BB = new Array("", "十", "百", "仟", "萬", "億", "点", "");
    let a = ("" + num).replace(/(^0*)/g, "").split("."),
        k = 0,
        re = "";
    for (let i = a[0].length - 1; i >= 0; i--) {
        switch (k) {
            case 0:
                re = BB[7] + re;
                break;
            case 4:
                if (!new RegExp("0{4}//d{" + (a[0].length - i - 1) + "}$")
                    .test(a[0]))
                    re = BB[4] + re;
                break;
            case 8:
                re = BB[5] + re;
                BB[7] = BB[5];
                k = 0;
                break;
        }
        if (k % 4 == 2 && a[0].charAt(i + 2) != 0 && a[0].charAt(i + 1) == 0)
            re = AA[0] + re;
        if (a[0].charAt(i) != 0)
            re = AA[a[0].charAt(i)] + BB[k % 4] + re;
        k++;
    }

    if (a.length > 1) // 加上小数部分(如果有小数部分)
    {
        re += BB[6];
        for (let i = 0; i < a[1].length; i++)
            re += AA[a[1].charAt(i)];
    }
    if (re == '一十')
        re = "十";
    if (re.match(/^一/) && re.length == 3)
        re = re.replace("一", "");
    return re;
}
```

### <span id="head55"> 将数字转换为大写金额</span>

```js
console.log(changeToChinese('1000')) // 壹仟元整
```

```js
/**
 * 将数字转换为大写金额
 * @param Num
 * @returns {string}
 */
export function changeToChinese(Num) {
    //判断如果传递进来的不是字符的话转换为字符
    if (typeof Num == "number") {
        Num = new String(Num);
    }
    ;
    Num = Num.replace(/,/g, "") //替换tomoney()中的“,”
    Num = Num.replace(/ /g, "") //替换tomoney()中的空格
    Num = Num.replace(/￥/g, "") //替换掉可能出现的￥字符
    if (isNaN(Num)) { //验证输入的字符是否为数字
        //alert("请检查小写金额是否正确");
        return "";
    }
    ;
    //字符处理完毕后开始转换，采用前后两部分分别转换
    let part = String(Num).split(".");
    let newchar = "";
    //小数点前进行转化
    for (let i = part[0].length - 1; i >= 0; i--) {
        if (part[0].length > 10) {
            return "";
            //若数量超过拾亿单位，提示
        }
        let tmpnewchar = ""
        let perchar = part[0].charAt(i);
        switch (perchar) {
            case "0":
                tmpnewchar = "零" + tmpnewchar;
                break;
            case "1":
                tmpnewchar = "壹" + tmpnewchar;
                break;
            case "2":
                tmpnewchar = "贰" + tmpnewchar;
                break;
            case "3":
                tmpnewchar = "叁" + tmpnewchar;
                break;
            case "4":
                tmpnewchar = "肆" + tmpnewchar;
                break;
            case "5":
                tmpnewchar = "伍" + tmpnewchar;
                break;
            case "6":
                tmpnewchar = "陆" + tmpnewchar;
                break;
            case "7":
                tmpnewchar = "柒" + tmpnewchar;
                break;
            case "8":
                tmpnewchar = "捌" + tmpnewchar;
                break;
            case "9":
                tmpnewchar = "玖" + tmpnewchar;
                break;
        }
        switch (part[0].length - i - 1) {
            case 0:
                tmpnewchar = tmpnewchar + "元";
                break;
            case 1:
                if (perchar != 0) tmpnewchar = tmpnewchar + "拾";
                break;
            case 2:
                if (perchar != 0) tmpnewchar = tmpnewchar + "佰";
                break;
            case 3:
                if (perchar != 0) tmpnewchar = tmpnewchar + "仟";
                break;
            case 4:
                tmpnewchar = tmpnewchar + "万";
                break;
            case 5:
                if (perchar != 0) tmpnewchar = tmpnewchar + "拾";
                break;
            case 6:
                if (perchar != 0) tmpnewchar = tmpnewchar + "佰";
                break;
            case 7:
                if (perchar != 0) tmpnewchar = tmpnewchar + "仟";
                break;
            case 8:
                tmpnewchar = tmpnewchar + "亿";
                break;
            case 9:
                tmpnewchar = tmpnewchar + "拾";
                break;
        }
        let newchar = tmpnewchar + newchar;
    }
    //小数点之后进行转化
    if (Num.indexOf(".") != -1) {
        if (part[1].length > 2) {
            // alert("小数点之后只能保留两位,系统将自动截断");
            part[1] = part[1].substr(0, 2)
        }
        for (i = 0; i < part[1].length; i++) {
            tmpnewchar = ""
            perchar = part[1].charAt(i)
            switch (perchar) {
                case "0":
                    tmpnewchar = "零" + tmpnewchar;
                    break;
                case "1":
                    tmpnewchar = "壹" + tmpnewchar;
                    break;
                case "2":
                    tmpnewchar = "贰" + tmpnewchar;
                    break;
                case "3":
                    tmpnewchar = "叁" + tmpnewchar;
                    break;
                case "4":
                    tmpnewchar = "肆" + tmpnewchar;
                    break;
                case "5":
                    tmpnewchar = "伍" + tmpnewchar;
                    break;
                case "6":
                    tmpnewchar = "陆" + tmpnewchar;
                    break;
                case "7":
                    tmpnewchar = "柒" + tmpnewchar;
                    break;
                case "8":
                    tmpnewchar = "捌" + tmpnewchar;
                    break;
                case "9":
                    tmpnewchar = "玖" + tmpnewchar;
                    break;
            }
            if (i == 0) tmpnewchar = tmpnewchar + "角";
            if (i == 1) tmpnewchar = tmpnewchar + "分";
            newchar = newchar + tmpnewchar;
        }
    }
    //替换所有无用汉字
    while (newchar.search("零零") != -1)
        newchar = newchar.replace("零零", "零");
    newchar = newchar.replace("零亿", "亿");
    newchar = newchar.replace("亿万", "亿");
    newchar = newchar.replace("零万", "万");
    newchar = newchar.replace("零元", "元");
    newchar = newchar.replace("零角", "");
    newchar = newchar.replace("零分", "");
    if (newchar.charAt(newchar.length - 1) == "元") {
        newchar = newchar + "整"
    }
    return newchar;
}
```

### <span id="head56"> 富文本代码块右上角显示复制按钮</span>

```js
onMounted(() => {
    codeCopy() // 代码块复制
})
```

```js
const COPY = 'copy'
const COPIED = 'copied'
function codeCopy() {
    if (typeof document === 'undefined') return; // 解决 document is not defined 的bug
    const preCode = document.querySelectorAll('pre code');
    if(!preCode) return
    Array.prototype.forEach.call(preCode, function(item) {
        const spanEl = document.createElement("span");
        spanEl.className = 'code-copy';
        spanEl.innerText = COPY;
        item.parentNode.setAttribute('class','copy-box');
        item.parentNode.appendChild(spanEl)
    })
    //markdown 代码存放在pre code 标签对中
    const codeCopyHandler = document.querySelectorAll('pre.copy-box .code-copy')
    Array.prototype.forEach.call(codeCopyHandler, function(itemCopyBox) {
        itemCopyBox.onclick = function (el) {
            if(el.target.innerText === COPIED) return;
            let text = el.target.previousSibling.textContent || el.target.previousSibling.value;
            const textareaEl = document.createElement("textarea");
            textareaEl.textContent = text;
            document.body.appendChild(textareaEl)
            textareaEl.select();
            document.execCommand('Copy',true)
            textareaEl.remove()
            el.target.innerText = COPIED
            setTimeout(() => {
                el.target.innerText = COPY
            },1000)
        }
    })
}

export default codeCopy;
```

### <span id="head57"> 图片懒加载</span>

```js
import lazyLoad from "@/plugins/lazy-load-img"

onMounted(() => {
    lazyLoad() // 图片懒加载
    window.addEventListener('scroll',handleScroll,false)
})
onUnmounted(() => {
    window.removeEventListener('scroll',handleScroll,false)
})
function handleScroll() {
    lazyLoad()
}
```

```js
/**
 * 图片懒加载
 */
const lazyLoadImg = () => {
    let viewHeight = document.documentElement.clientHeight
    let limg = document.querySelectorAll("img[data-src]")
    console.log(`limg===>`, limg)
    Array.prototype.forEach.call(limg, function (item, index) {
        let rect;
        if (item.getAttribute("data-src") === "") return
        //getBoundingClientRect
        rect = item.getBoundingClientRect();
        if (rect.bottom >= 0 && rect.top < viewHeight) {
            (function () {
                let img = new Image()
                img.src = item.getAttribute("data-src")
                item.src = img.src
                item.removeAttribute('data-src')
                item.setAttribute("class","loaded")
            })()
        }
    })
}
// 定义一个防抖函数
const debounce = (fn, delay = 500) => {
    let timer = null;
    return function (...args) {
        if (timer) clearTimeout(timer);
        timer = setTimeout(() => {
            fn.apply(this, args);
        }, delay);
    };
}
const lazyLoad = debounce(lazyLoadImg)
export default lazyLoad
```



### <span id="head58"> 计算字符串长度（中英文）</span>

```js
/*
* 计算字符串长度(英文占1个字符，中文汉字占2个字符)
*/
export const strlen  = (str)  => {
  var len = 0;
  for (var i=0; i<str.length; i++) {
    var c = str.charCodeAt(i);
    //单字节加1
    if ((c >= 0x0001 && c <= 0x007e) || (0xff60<=c && c<=0xff9f)) {
      len++;
    }
    else {
      len+=2;
    }
  }
  return len;
}
```



### <span id="head59"> 判断链接是否为本地</span>

```js
export const isLocal = () => {
  const localURL = /^(192\.168\.\d{1,3}\.\d{1,3})|(localhost)|(172\.16\.\d{1,3}\.\d{1,3})$/;
  let hostname = window.location.hostname;
  return localURL.test(hostname)
}
```



### <span id="head60"> 获取当前url上的参数</span>

```js
/**
 *@desc 获取当前url上的参数
 **/
export const getRequest = () => {
  var wholeUrl = window.location.href // 获取url中"?"符后的字串
  var wholeUrlArr = wholeUrl.split('?')
  var url = wholeUrlArr[1]
  if (url) {
    var theRequest = {}
    var strs = url.split('&')
    for (var i = 0; i < strs.length; i++) {
      const value = unescape(strs[i].split('=')[1])
      const valArr = value.split('#')
      theRequest[strs[i].split('=')[0]] = valArr[0]
    }
    return theRequest
  }
  return null
}
```



### <span id="head61"> 处理二进制流文件</span>

````js
  /**
   * 二进制流处理
   * @param  { Object } res  axios返回的response
   */
  function blobDataHandle(res) {
    const blobType = res.data.type || ''

    // excel文档下载
    // if (blobType.indexOf("application/vnd.ms-excel") >= 0) {
    if (blobType.indexOf('application/vnd') >= 0) {
      const blob = new Blob([res.data], { type: 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=utf-8' }) // application/vnd.openxmlformats-officedocument.spreadsheetml.sheet这里表示xlsx类型
      let filename = 'excel.xls'
      if (res.config.exportName) {
        filename = res.config.exportName
      }
      if ('download' in document.createElement('a')) {
        const downloadElement = document.createElement('a')
        let href = ''
        if (window.URL) href = window.URL.createObjectURL(blob)
        else href = window.webkitURL.createObjectURL(blob)
        downloadElement.href = href
        downloadElement.download = filename
        document.body.appendChild(downloadElement)
        downloadElement.click()
        if (window.URL) window.URL.revokeObjectURL(href)
        else window.webkitURL.revokeObjectURL(href)
        document.body.removeChild(downloadElement)
      } else {
        navigator.msSaveBlob(blob, filename)
      }
      return
    } else if (blobType.indexOf('multipart/form-data') >= 0) {
      var blob = new Blob([res.data], { type: 'application/x-tar' })
      const linkNode = document.createElement('a')
      let filename = 'excel.xls'
      if (res.headers.filename) {
        filename = decodeURIComponent(res.headers.filename)
      }
      // linkNode.download = new Date().Format('yyyy-MM-dd hhmmss')
      // linkNode.download = fileName + '.tar'
      linkNode.download = filename + '.zip'
      linkNode.style.display = 'none'
      linkNode.href = URL.createObjectURL(blob)
      document.body.appendChild(linkNode)
      linkNode.click()
      URL.revokeObjectURL(linkNode.href)
      document.body.removeChild(linkNode)
    } else if (blobType.indexOf('application/json') >= 0) {
      throw res
    }

    const errorMessage = {
      message: '未处理的二进制：' + blobType
    }
    throw errorMessage
  }
````



## <span id="head62"> 时间类</span>

### <span id="head63"> 获取当前周几</span>

别名：今天星期几

```js
getDayText() //五
```

```js
/**
 * 获取当前日期
 * @param date
 * @returns {string}
 */
export const getDayText = (date = new Date()) => {
        if (typeof (date) === 'number') {
            date = new Date(date);
        } else if (typeof (date) === 'string') {
            date = new Date(date.replace(/-/g, '/').replace(/\./g, '/'));
        }
        return '日一二三四五六'.charAt(date.getDay());
    };
```

### <span id="head64"> 时间问候语</span>

别名：good morning

```js
/**
 * 时间问候语
 * @returns {string}
 */
export const greetTime = () => {
        const time = new Date();
        const hour = time.getHours();
        return hour < 9 ? '早上好' : hour <= 11 ? '上午好' : hour <= 13 ? '中午好' : hour < 20 ? '下午好' : '晚上好'
    }
```

### <span id="head65"> Date时间转数字</span>

别名：时间转数字格式、时间戳

```js
parseTime(new Date(), '{y}{m}{d}') // 20211221
```

```js
/**
 * 时间转数字
 * @param time new Date()
 * @param cFormat 显示的格式
 * @returns {string|null}
 */
export function parseTime(time, cFormat) {
    if (arguments.length === 0 || !time) {
        return null;
    }
    const format = cFormat || '{y}-{m}-{d} {h}:{i}:{s}';
    let date;
    if (typeof time === 'object') {
        date = time;
    } else {
        if (typeof time === 'string') {
            if (/^[0-9]+$/.test(time)) {
                time = parseInt(time);
            } else {
                // support safari
                // https://stackoverflow.com/questions/4310953/invalid-date-in-safari
                time = time.replace(new RegExp(/-/gm), '/');
            }
        }

        if (typeof time === 'number' && time.toString().length === 10) {
            time = time * 1000;
        }
        date = new Date(time);
    }
    const formatObj = {
        y: date.getFullYear(),
        m: date.getMonth() + 1,
        d: date.getDate(),
        h: date.getHours(),
        i: date.getMinutes(),
        s: date.getSeconds(),
        a: date.getDay(),
    };
    const time_str = format.replace(/{([ymdhisa])+}/g, (result, key) => {
        const value = formatObj[key];
        // Note: getDay() returns 0 on Sunday
        if (key === 'a') {
            return ['日', '一', '二', '三', '四', '五', '六'][value];
        }
        return value.toString().padStart(2, '0');
    });
    return time_str;
}
```

### <span id="head66"> Date时间转汉字</span>

别名：时间转汉字、时间戳

```js
formatTime(new Date()) // 刚刚
```

```js
/**
 * 时间转汉字
 * @param  {number} time
 * @param  {string} option
 * @returns {string}
 */
export function formatTime(time, option) {
    if (('' + time).length === 10) {
        time = parseInt(time) * 1000
    } else {
        time = +time
    }
    const d = new Date(time)
    const now = Date.now()

    const diff = (now - d) / 1000

    if (diff < 30) {
        return '刚刚'
    } else if (diff < 3600) {
        // less 1 hour
        return Math.ceil(diff / 60) + '分钟前'
    } else if (diff < 3600 * 24) {
        return Math.ceil(diff / 3600) + '小时前'
    } else if (diff < 3600 * 24 * 2) {
        return '1天前'
    }
    if (option) {
        // 必须使用 [ parseTime ] 这个函数
        // return parseTime(time, option)
    } else {
        return (
            d.getMonth() +
            1 +
            '月' +
            d.getDate() +
            '日' +
            d.getHours() +
            '时' +
            d.getMinutes() +
            '分'
        )
    }
}
```

### <span id="head67"> 获取某月有多少天</span>

```js
console.log(getMonthOfDay('2021-2')) // 28
console.log(getMonthOfDay()) // 当前为2021-12月，即得到的是30
```

```js
/**
 * 获取某月有多少天
 * @param time
 * @returns {number}
 */
export function getMonthOfDay(time) {
    let date = new Date(time);
    let year = date.getFullYear();
    let mouth = date.getMonth() + 1;
    let days;

    let timeArr = [1, 3, 5, 7, 8, 9, 10, 12]
    //当月份为二月时，根据闰年还是非闰年判断天数
    if (mouth === 2) {
        days = year % 4 === 0 ? 29 : 28
    } else if (timeArr.includes(mouth)) {
        //月份为：1,3,5,7,8,10,12 时，为大月.则天数为31；
        days = 31
    } else {
        //其他月份，天数为：30.
        days = 30
    }
    return days
}
```

### <span id="head68"> 获取某年的第一天</span>

```js
console.log(getFirstDayOfYear('2021')) // 2021-01-01 00:00:00
```

```js
/**
 * 获取某年的第一天
 * @param time
 * @returns {string}
 */
export function getFirstDayOfYear(time) {
    let year = new Date(time).getFullYear();
    return year + "-01-01 00:00:00";
}
```

### <span id="head69"> 获取某年最后一天</span>

```js
console.log(getLastDayOfYear('2021')) // 2021-12-31 23:59:59
```

```js
/**
 * 获取某年最后一天
 * @param time
 * @returns {string}
 */
export function getLastDayOfYear(time) {
    let year = new Date(time).getFullYear();
    let dateString = year + "-12-01 00:00:00";
    // 要配合 [ getMonthOfDay ] 这个方法使用
    let endDay = getMonthOfDay(dateString);
    return year + "-12-" + endDay + " 23:59:59";
}
```

### <span id="head70"> 获取某个日期是当年中的第几天</span>

```js
console.log(getDayOfYear('2021-12-13')) // 347
```

```js
/**
 * 获取某个日期是当年中的第几天
 * @param time
 * @returns {number}
 */
export function getDayOfYear(time) {
    // 要配合 [ getFirstDayOfYear ] 这个方法使用
    let firstDayYear = getFirstDayOfYear(time);
    let numSecond = (new Date(time).getTime() - new Date(firstDayYear).getTime()) / 1000;
    return Math.ceil(numSecond / (24 * 3600));
}
```

### <span id="head71"> 获取某个日期在这一年的第几周</span>

```js
console.log(getDayOfYearWeek('2021-12-31')) // 53
```

```js
/**
 * 获取某个日期在这一年的第几周
 * @param time
 * @returns {number}
 */
export function getDayOfYearWeek(time) {
    // 要配合 [ getDayOfYear ] 这个方法使用
    let numDays = getDayOfYear(time);
    return Math.ceil(numDays / 7);
}
```

### <span id="head72"> 把秒转化为天小时分钟秒</span>

```js
sToHs(60) // 1分钟0秒
```

```js
/**
 * 把秒转化为天小时分钟秒
 * @param seconds
 * @returns {string}
 */
export function sToHs(seconds){
  let daySec = 24 *  60 * 60;
  let hourSec=  60 * 60;
  let minuteSec = 60;
  let dd = Math.floor(seconds / daySec);
  let hh = Math.floor((seconds % daySec) / hourSec);
  let mm = Math.floor((seconds % hourSec) / minuteSec);
  let ss = seconds%minuteSec;
  if(dd > 0) {
    return dd + "天" + hh + "小时" + mm + "分钟"+ss+"秒";
  } else if (hh > 0) {
    return hh + "小时" + mm + "分钟"+ss+"秒";
  } else if (mm > 0) {
    return mm + "分钟"+ss+"秒";
  } else {
    return ss+"秒";
  }
}
```


## <span id="head73"> DOM类</span>

### <span id="head74"> 获取元素</span>

```js
export function $(selector) {
    let type = selector.substring(0, 1);
    if (type === '#') {
        if (document.querySelecotor) return document.querySelector(selector)
        return document.getElementById(selector.substring(1))
    } else if (type === '.') {
        if (document.querySelecotorAll) return document.querySelectorAll(selector)
        return document.getElementsByClassName(selector.substring(1))
    } else {
        return document['querySelectorAll' ? 'querySelectorAll' : 'getElementsByTagName'](selector)
    }
}
```

### <span id="head75"> 跳转到某个元素</span>

别名：滚动到某个div

```js
goTo()
{
    goToDom(document.querySelector('#contentTop'), 0);
}
```

```js
/**
 * 跳转到某个元素
 * @param toEl
 * @param n
 */
export function goToDom(toEl, n) {
    let bridge = toEl;
    let body = document.body;
    let height = 0;
    do {
        height += bridge.offsetTop;
        bridge = bridge.offsetParent;
    } while (bridge !== body)
    // 增加动画
    window.scrollTo({
        top: height - n,
        behavior: 'smooth'
    })
}
```

### <span id="head76"> 切换元素</span>

别名：切换class

```js
toggle()
{
    toggleClass(document.body, 'dark')
}
```

```js
/**
 * 切换Class
 * @param element
 * @param className
 */
export function toggleClass(element, className) {
    if (!element || !className) {
        return
    }
    let classString = element.className
    const nameIndex = classString.indexOf(className)
    if (nameIndex === -1) {
        classString += '' + className
    } else {
        classString =
            classString.substr(0, nameIndex) +
            classString.substr(nameIndex + className.length)
    }
    element.className = classString
}
```

### <span id="head77"> 获取兄弟节点</span>

```js
/**
 * 获取兄弟节点
 * @param ele
 * @returns {[]}
 */
export function siblings(ele) {
    let chid = ele.parentNode.children, eleMatch = [];
    for (let i = 0, len = chid.length; i < len; i++) {
        if (chid[i] != ele) {
            eleMatch.push(chid[i]);
        }
    }
    return eleMatch;
}
```

### <span id="head78"> 判断是否存在Class</span>

别名：class是否存在

```js
/**
 * 判断是否存在某个Class
 * @param ele 元素
 * @param cls class名
 * @returns {boolean}
 */
export function hasClass(ele, cls) {
    return !!ele.className.match(new RegExp('(\\s|^)' + cls + '(\\s|$)'))
}
```

### <span id="head79"> 添加Class</span>

```js
/**
 * 添加class
 * @param ele 元素
 * @param cls class名
 */
export function addClass(ele, cls) {
    if (!hasClass(ele, cls)) ele.className += ' ' + cls
}
```

### <span id="head80"> 删除Class</span>

```js
/**
 * 删除Class
 * @param ele 元素
 * @param cls class名
 */
export function removeClass(ele, cls) {
    if (hasClass(ele, cls)) {
        const reg = new RegExp('(\\s|^)' + cls + '(\\s|$)')
        ele.className = ele.className.replace(reg, ' ')
    }
}
```

### <span id="head81"> 创建a标签打开新页面</span>

别名：window.open被浏览器拦截

```js
/**
 * window.open(url)打开链接被浏览器拦截解决方案
 * @param url
 */
export function addBlankPath(url) {
    const a = document.createElement('a')
    a.href = url  // 窗口的链接
    a.target = '_blank'
    document.body.appendChild(a)
    a.click()
    document.body.removeChild(a)
}
```
