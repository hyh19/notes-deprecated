# [Vue 实例](https://cn.vuejs.org/v2/guide/instance.html)

## 数据与方法

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue"></script>
</head>
<body>
    <script type="text/javascript">
        var data = { a: 1 }
        var vm = new Vue({
            data: data
        })
        // 他们引用相同的对象！
        console.log(vm.a === data.a)

        // 设置属性也会影响到原始数据
        vm.a = 2
        console.log(data.a)

        // ... 反之亦然
        data.a = 3
        console.log(vm.a)
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue"></script>
</head>
<body>
    <div id="example"></div>
    <script type="text/javascript">
        var data = { a: 1 }
        var vm = new Vue({
            el: '#example',
            data: data
        })

        console.log(vm.$data === data)
        console.log(vm.$el === document.getElementById('example'))

        vm.$watch('a', function (newValue, oldValue) {
            console.log('newValue: ' + newValue + ' oldValue: ' + oldValue) 
        })
    </script>
</body>
</html>
```

## 实例生命周期

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue"></script>
</head>
<body>
    <script type="text/javascript">
        new Vue({
            data: {
                a: 1
            },
            created: function () {
                console.log('a is: ' + this.a)
            }
        })
    </script>
</body>
</html>
```


