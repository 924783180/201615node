PK
     WswJ�z��/_  /_     angular 2.md## angular 2

#### yarn
和npm相似的一个工具，用法基本相同
- 下载安装,会生成一个package.json的文件
```
npm install yarn -g
```
- 初始化  和npm初始化是一样的
```
yarn init
```
- 安装包 但是会自动的安装最新版本的，会覆盖指定的版本号
- yarn add [package]@[version] — 带版本号安装  这样就不会覆盖之前的版本了
```
yarn add angular 
yarn add gulp --dev 生产依赖
yarn add gulp --save-dev 项目依赖
```
- 移出某一个依赖
```
yarn remove [package]
```
在其他项目中初始化的时候把package.json/yarn.lock复制过去，就可以秒下载了

#### 
```
bootstrap栅格化布局 一共 12 列
<div class='contain'>
  <div class='row'>
      <div class='col-md-3'>
      </div>
      <div class=col-md-9>
      </div>
	  <div class='col-md-12'>
	    <table>
	    </table>
	  </div>
  </div>
</div>
前面的是去掉两边的边距，即将中间的分为12份，左边的占三份，右边的占9份
后面就是在一行中，去掉两边的边距，将中间的宽度进行划分一共分为12份
```

##### 在一个列表中进行筛选
- 例如在分数中选出符合条件的某一个数字进行变颜色
```
ng-bind-html 这里只能放字符串类型的,将数据html编译成$sce.trustAsHtml(html)

<body ng-controller="myCtrl">
<!--bootstrap栅格化布局 一共 12 列-->
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <input type="text" ng-model="query">
            <table class="table table-bordered">
                <tr>
                    <th>名字</th>
                    <th>英语</th>
                    <th>语文</th>
                    <th>数学</th>
                </tr>
                <tr ng-repeat="score in scores track by $index">
                    <!--130=><font color="red">13</font>0-->
                    <td>{{score.name}}</td>
                    <td ng-bind-html="score.english | tableRed:query | asHtml"></td>
                    <td ng-bind-html="score.chinese | tableRed:query | asHtml"></td>
                    <td ng-bind-html="score.math | tableRed:query | asHtml"></td>
                </tr>
            </table>
        </div>
    </div>
</div>
<!--ng-bind-html 这里只能放字符串类型的,将数据html编译成$sce.trustAsHtml(html)-->
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.filter('tableRed',function () {
        return function (input,param1) {
            //input是数字类型
            if(typeof param1 != 'undefined'){
                var reg = new RegExp(param1,'g');
                input = input.toString().replace(reg,function () {
                    //匹配出来的结果 是1
                    return `<font color="red" style="font-weight: bold">${arguments[0]}</font>`
                });
                //param1 = 1  130 => <font color="red">1</font>30
            }
            return input;//不需要过滤时将原值返回即可
        }
    });
    //在过滤器中使用angular自带的方法
    app.filter('asHtml',function ($sce) {
        return function (input) {
            return $sce.trustAsHtml(input.toString()); //将页面的数据转换成了html内容
        }
    });
    app.controller('myCtrl',['$scope','$sce',function ($scope,$sce) {
        $scope.html =123;
        $scope.html1 ='<h1>hello world</h1>';
        $scope.scores  =  [
            {name:'张三',english:86,chinese:77,math:23},
            {name:'李四',english:30,chinese:30,math:130},
            {name:'王五',english:88,chinese:46,math:99},
            {name:'赵六',english:95,chinese:82,math:62}
        ];
    }]);
</script>

```
#### angular中的指令
- 有四种格式
```
<!--Element E-->
<my-drag></my-drag>
<!--Attributes A-->
<div my-drag></div>
<!--Class C-->
<div class="my-drag"></div>
<!--Comment M 前后必须用空格，不建议使用-->
<!-- directive:myDrag -->

```
 指令分为两大类
- 1 自带指令
- 2 自定义指令
  装饰型指令 赋予标签一些功能  link函数
```
---angular和jquery的引用先后顺序：先引jquery在引angular

<div my-border>hello</div>
<div drag></div>

    var app = angular.module('appModule',[]);
    //param1 指令的名字
    //指令默认没有作用域
    app.directive('myBorder',function () {
        return {
            replace:true,
            restrict:'EA',
            //链接函数 链接视图和作用域的
            link:function ($scope,element,attrs) {//形参 ,在link函数中可以操作DOM元素
                // 1.代表的是当前指令所在的作用域
                // 2.element代表当前指令所在的标签，是一个jquery对象
                element.css('outline','5px dashed yellowgreen');
                //angular.element == $
                angular.element(document).on('click',function () {
                    alert('click')
                });
                // 3.代表当前指令上所有的属性集合
                console.log(attrs);
            }
        }
    });
```  
  组件式 把标签 替换成一个组件 template
```
  ```
 <!--Element E-->
<my-drag></my-drag>
<!--Attributes A-->
<div my-drag></div>
<!--Class C-->
<div class="my-drag"></div>
<!--Comment M 前后必须用空格，不建议使用-->
<!-- directive:myDrag -->

var app = angular.module('appModule',[]);
    //param1 指令的名字
    app.directive('myDrag',function () {
        return {
            replace:true, //将原指令标签替换掉，要求模板只能有一个根节点
            restrict:'EA', //限制使用范围 默认范围是EA,范围的标识必须大写
            template:`<div>
                        <div>Hello</div>
                        <div>zfpx</div>
                      </div>` //模板的根节点只能有一个
        }
    });
 ```
```
当然自定义中装饰性指令和组件性的指令是可以混合用的
组件化的开发：复用，管理统一方便
```
  ```
 <div class="container">
    <div class="row">
        <div class="col-md-12">
            <panel st="success" title="文章1">
                这是文章内容 angularjs
            </panel>
            <panel st="danger" title="文章2">
                这是第二篇文章文章内容 angularjs
            </panel>
            <panel st="warning" title="文章3">
                这是第三篇文章文章内容 angularjs
            </panel>
        </div>
    </div>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.run(function ($rootScope) {
        $rootScope.age = 'success';
    });
    app.directive('panel',function () {
        return {
            restrict:'E',
            templateUrl:`tmpl/panel.html`,//ajax获取过来的
            link:function (scope,element,attrs) {
                //自己家没有 根作用域，如果自己家有 就是自己家的作用域
                scope.name = attrs.st;
                scope.title = attrs.title;
            },
            scope:{}, //给当前指令产生一个作用域 {}  默认值是false
            // {} 不能获取父作用域的属性 相当于开辟了一个和$rootScope平级的作用域
            transclude:true,//会产生一个作用域,会将指令中夹着的部分 插入到带有ng-transclude的标签内
        };
    });
</script>

上面templateUrl:`tmpl/panel.html引入的内容：
<div class="panel panel-{{name}}">
    <div class="panel-heading">
        {{title}}
    </div>
    <div class="panel-body" ng-transclude>

    </div>
    <div class="panel-footer">
        <!--使用& 必须传入的是一个对象，名字和形参名相同-->
        <button ng-click="say({abc:title,b:'hello'})">点我say</button>
    </div>
</div>


其中 <span ng-transclude></span>是为了防止‘文章的内容是：’会被新的内容给覆盖掉
   ```
```
angular中的三个符号
```
@ name:'@st', @符引用的是字符串 名字相同可以去掉后面的
= title:'=article' = 取的是变量
& 取的是函数方法 
```

指令和指令之间的交互  手风琴
```
<!--ng-class（动态）和class不冲突（静态）
{true:'block',false:'none'}[flag]
{block:flag,none:!flag}

    ng-show/ng-hide ng-if
    频繁切换用show/hide  操作的是样式
    一次就确定下来的内容 如果值为false内部代码不执行 操作的dom, repeat经常和if连用  if会产生一个作用域
```
```
<group>
    <open title="标题1">内容1</open>
    <open title="标题2">内容2</open>
    <open title="标题3">内容3</open>
</group>
<group>
    <open title="标题1">内容1</open>
    <open title="标题2">内容2</open>
</group>
<!--将所有open的控制权交给group处理，点自己时告诉父亲 除了自己都关掉-->
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.directive('group',function () {
       return {
           restrict:'E',
           controller:function ($scope) {
               var arr = [];
               //1.group中要保存所有的open的作用域  [scope,scope,scope]
               this.collectScope = function (s) {
                   arr.push(s);
               };
               //4.拿三个scope和传过来的比,如果不一样 让其他scope下的flag为false
                this.tell = function (s) {
                    arr.forEach(function (item) { //循环判断 当前传入的和item是否相等
                        if(s!=item){
                            item.flag = false;
                        }
                    });
                };
           }
       }
    });
    app.directive('open',function () {
        return {
            restrict:'E',
            require:'^group',
            templateUrl:'tmpl/open.html',
            link:function ($scope,element,attrs,oCtrl) {
                //2.将作用域传递给父亲
                oCtrl.collectScope($scope);
                //定义自己家的方法
                $scope.flag = false;
                $scope.show = function () {
                    //3.告诉父亲当前是我在点，让父亲将其他人的作用域关闭掉 oCtrl.tell(scope)
                    $scope.flag =! $scope.flag;
                    oCtrl.tell($scope)
                }
            },
            transclude:true,
            scope:{ //接收的都是属性
                tit:'@title'
            }
        }
    });
    app.controller('myCtrl',function ($scope) {})
</script>

引入的文件
<div>
    <div class="title" ng-click="show()" >{{tit}}</div>
    <div class="content" ng-show="flag" >文章的内容是: <span ng-transclude></span></div>
</div>
```

link的第四个函数以及require  增加girl控制器的实例
```
<girl>
    <a eat train>小美丽</a>
</girl>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.directive('girl',function () {
        return {
            restrict:'E',
            controller:function ($scope) { //当前指令所在的作用域,不会产生作用域
                $scope.arr = [];
                this.collect = function (kinds) {
                    $scope.arr.push(kinds);
                }
            },
            link:function (scope,element) {
                element.on('click',function () {
                    alert(scope.arr)
                })
            }
        }
    });
    app.directive('train',function () {
        return {
            restrict:'A',
            //^自己身上找找不到向上找
            require:'^?girl',//在当前指令上找girl，找到则会在link函数的第四个参数增加girl控制器的实例
            link:function (scope,element,attrs,gCtrl) { //如果没有依赖到使用的是? 则得到的值是null，否则报错
                console.log(gCtrl);
                gCtrl.collect('火车');
            }
        }
    });

    app.directive('eat',function () {
        return {
            restrict:'A',
            require:'^girl',//在当前指令上找girl，找到则会在link函数的第四个参数增加girl控制器的实例
            link:function (scope,element,attrs,gCtrl) {
               gCtrl.collect('吃');
            }
        }
    })
</script>
```

angullar的脏值检查  
```
$watch
对比两次的变化，触发对应的回调函数,将所有数据放在一个数组中，有一个变化 所有$watchers执行一遍
```
```
触发事件

 ng-change="change()"触发的事件
            $rootScope.change = function () {
                console.log($rootScope.name);
            };
```
```
实时监控

1 $rootScope.$watch(function () {
            return $rootScope.name;
        },function (newVal,oldVal) {
            console.log(newVal,oldVal);
        })

放入的是作用域上的一个变量字符串
2 $rootScope.$watch('name',function (newVal,oldVal) {
            console.log(newVal,oldVal);
        })

两者是一样的，后者直接把函数放在里面
```
满百包邮  监控总价
```
<body>
<div ng-controller="myCtrl">
    名称：{{product.name}} <br>
    价格：{{product.price}} <br>
    数量:<input type="text" ng-model="product.count"> <br>
    邮费：{{product.free}} <br>
    总价：{{sum()+product.free}}
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('myCtrl',function ($scope) {
        $scope.product = {name:'电脑',price:20,count:2,free:30};
        //监控总价 如果总价值大于100  邮费=0;
        $scope.sum = function () {
            return $scope.product.price*$scope.product.count
        };
        $scope.$watch($scope.sum,function (newVal) {
            $scope.product.free = newVal>=100?0:30;
        });
    });
</script>
```
angular 的  内置服务5个
```
 $interval,$timeout,$sce, $http, $jsonp
``` 
 数据和视图的改变  
```
  ```
  $apply手动刷新视图

 app.controller('myCtrl',function ($scope,$interval,$timeout,$sce) { //服务
        var timer = $interval(function () { //不是angular自带的方法，数据更新不会影响视图
            //通知视图刷新
            $scope.d = Date.now();
            //$scope.$apply(); //手动刷新视图
        },1000);
        $interval.cancel(timer); //取消定时器
    })
 ```
 发送验证码
 
 <body ng-controller="myCtrl">
123123
<button ng-click="show()" ng-disabled="!flag">{{text}}</button>
{{d | date:'yyyy-dd hh:mm:ss'}}
<script src="node_modules/angular/angular.js"></script>
<script>
    //点击按钮 获取验证码， -> 等待(5) 禁用状态->  获取验证码
    var app = angular.module('appModule',[]);
    //ng-model aa-model
    app.controller('myCtrl',function ($scope,$interval) {
        $scope.text = "发送验证码";
        $scope.flag = true;
        $scope.show = function () {
            $scope.text = 5;
            $scope.flag = false;
            var timer = $interval(function () {
                $scope.text = $scope.text-1;
                if($scope.text<=0){
                    $interval.cancel(timer);
                    $scope.flag = true;
                    $scope.text = "发送验证码";
                }
            },1000);
        }
    });
```

数据和视图是相互影响的，两者谁的改变都会影响另外一个
```
<body>
<!-- M->V -->
<!-- v->M -->
<!--没有作用域的产生 指令-->
<input type="text" zf-model="aa">
{{aa}}
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.run(function ($rootScope,$interval) {
        $rootScope.name = 100;
        $rootScope.age = 200;
       /* $interval(function () {  $scope.$apply()
            $rootScope.name++;
        },1000)*/
    });
    app.directive('zfModel',function () {
        return {
            restrict:'A',
            link:function (scope,element,attrs) {
                //监控作用域上值得变化 ，将值赋予给输入框
                scope.$watch(attrs["zfModel"],function (newVal) {
                    element.val(newVal); //将新值赋予给输入框
                });
                //监控输入框输入的内容,将监控的内容 绑定到作用域上 scope[attrs["zfModel"]] = 1111
                element.on('input',function () { //手动apply
                    scope[attrs["zfModel"]] = element.val(); //改变数据 不会通知视图刷新
                    scope.$apply();
                });
            }
        }
    });// $interval $timeout $sce $http.jsonp  自定义服务
</script>
```

angular中的￥http的服务
```
$http 用法同jquery
```

```
百度的首页搜索框
<body ng-controller="searchCtrl">
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <h1 class="h1 text-center">搜索框</h1>
            <input type="text" class="form-control" ng-model="query" ng-change="search()" ng-keyup="changeIndex($event)" ng-keydown="prevent($event)"/>
            <ul class="list-group">
                <li class="list-group-item" ng-class="{active:index==$index}"  ng-repeat="a in arrs track by $index">{{a}}</li>
            </ul>
        </div>
    </div>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.controller('searchCtrl',function ($scope,$http,$sce) { //$http 用法同jquery
        $scope.index = -1; //默认哪个都不选中
        $scope.prevent = function (e) { //光标向前是 按下导致的
            if(e.keyCode == 38 ){ //阻止按下上时的事件
                e.preventDefault();
            }
        };
        $scope.changeIndex = function (e) {
            var code = e.keyCode;
            if(code == 38){
                $scope.index -=1;
                if($scope.index<=-1){
                    $scope.index = $scope.arrs.length-1;
                }
                $scope.query = $scope.arrs[$scope.index]; //上下实现赋值
            }else if(code == 40){
                $scope.index +=1;
                if($scope.index == $scope.arrs.length){
                    $scope.index = 0;
                }
                $scope.query = $scope.arrs[$scope.index]; //上下实现赋值
            }else if(code == 13){
                window.open('https://www.baidu.com/s?wd='+$scope.query); //打开新页面
            }
        };
        $scope.search = function () { //输入时触发
            //作为一个可信任的路径$sce.trustAsResourceUrl()
            //百度jsonp接口 callback名字得是cb
            //$http 返回都是promise对象 对象中有一个then方法，方法中有两个参数，第一个是成功的回调第二个是失败的回掉
            $http.jsonp(
                    $sce.trustAsResourceUrl('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd='+$scope.query)
                    ,{jsonpCallbackParam: 'cb'}
            ).then(function (res) { //成功
                $scope.arrs = res.data.s;
            },function (err) { //失败
                console.log(err);
            });
        }
    });
</script>
```
angular中的   自定义服务5个
```
    //1.服务是单例的 ，方法都是公用的
    var app = angular.module('appModule',[]);
    //2.实现复用代码 $rootScope, localStorage
    //3.angular提供了五种服务  "provider factory" service constant value
    //4.provider是最大的服务，可以进行配置,其他4中不能配置
```
```
自定义服务的配置问题
 
 <body>
<div></div>
<div ng-controller="oneCtrl">

</div>
<div ng-controller="twoCtrl">

</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    //1.服务是单例的 ，方法都是公用的
    var app = angular.module('appModule',[]);
    //2.实现复用代码 $rootScope, localStorage
    //3.angular提供了五种服务  "provider factory" service constant value
    //4.provider是最大的服务，可以进行配置,其他4中不能配置
    app.config(function (CalcProvider) { //在要配置的服务后面 增加Provider后缀
        //console.log(CalcProvider); //是provider的一个实例
        CalcProvider.currency = '£'
    });
    //运行顺序 new Provider > config > $get
    app.provider('Calc',function () { //当我们将Storage放到控制器中，会自动调用$get方法
        this.currency = '$'; //在实例上增加了属性
        this.$get = function () {
            //var that = this; //谁调用的$get(实例)
            return {
                sum: (a,b) => this.currency +(a+b)
            }
        };
    });//provider默认就会被实例化
    app.controller('oneCtrl',function ($scope,$http,Calc) {  //sum()
        console.log(Calc.sum(1,2));
    });
    app.controller('twoCtrl',function ($scope,$http) {
    });
    //$http $sce
</script>
```

```
内置缓存的 Storage


<body>
<div ng-controller="myCtrl"></div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    app.provider('Storage',function () {
        this.$get = function () {
            var obj = {
                setStorage: function (key, value) {
                    if (typeof value == 'object') {
                        value = JSON.stringify(value);
                    }
                    window.localStorage.setItem(key, value);
                },
                getStorage: function (key) {
                    // 判断是不是对象 "[{}] {}"
                    var value = window.localStorage.getItem(key);
                    if (value.startsWith('[') || value.startsWith('{')) {
                        value = JSON.parse(value);
                    }
                    return value;
                }
            };
            return obj; //这里返回的就是最后注入到控制的内容
        }
    });
    app.controller('myCtrl',function ($scope,Storage) {
        console.log(Storage.getStorage('name').name);
    });
</script>

```
```
内置服务 factory



<body>
<div ng-controller="myCtrl"></div>
<script src="node_modules/angular/angular.js"></script>
<script>
    var app = angular.module('appModule',[]);
    //factory 不具有配置能力，因为$get方法在我们的config方法后执行 配置后会被覆盖掉
    /* app.config(function (CalcProvider) { //CalcProvider  provider 的实例
        CalcProvider.currency = '%'; //未生效的
    });*/
    app.factory('Calc',function () { //factory是基于provider的 后面的函数就是provider的$get
        var currency = '$';
        return {
            "+" : (a,b)=>currency+(a+b)
        }; //这里返回的就是最后注入到控制的内容
    });
    // 最大的是provider factory是基于provider的  service 是基于factory的  value是基于 factory的
    //控制器间的交互 1.$rootScope, 服务， 事件(观察者模式) $on  $emit  $broadcast
    app.controller('myCtrl',function ($scope,Calc) {
        console.log(Calc["+"](1,2));
    });
</script>
```

angular中的事件

```
$scope.$on  $scope.$emit   $scope.$watch
```
```
买玩具

<body>
<div ng-controller="parent">
    <input type="text" ng-model="total" ng-change="tellSon()">
    <div ng-controller="child">
        玩具名称  {{toy.name}} <br>
        数量 <input type="text" ng-model="toy.count "> {{toy.count | number:0}}<br>
        价格 {{toy.price}}
    </div>
</div>
<script src="node_modules/angular/angular.js"></script>
<script>
    // $scope.$broadcast
    var app = angular.module('appModule',[]);
    app.controller('parent',function ($scope) {
        $scope.total = 10;
        //e代表的是事件源 自带的
        $scope.tellSon = function () {
            $scope.$broadcast('给你钱',$scope.total)
        };
        $scope.$on('买车',function (e,data,a) {$scope.total = data;});
    });
    app.controller('child',function ($scope) {
        $scope.$on('给你钱',function (e,data) {
            $scope.toy.count =data/ $scope.toy.price
        });
        $scope.toy = {name:'玩具车',count:1,price:10};
        $scope.$watch('toy.count',function (newVal) {
            $scope.$emit('买车',newVal*$scope.toy.price);
        });
    });
```
  


PK 
     WswJ�z��/_  /_                   angular 2.mdPK      :   Y_    