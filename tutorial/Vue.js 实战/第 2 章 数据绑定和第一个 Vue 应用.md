# 第 2 章 数据绑定和第一个 Vue 应用

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <input type="text" v-model="name" placeholder="你的名字">
        <h1>你好，{{ name }}</h1>
    </div>
</body>
<script type="text/javascript">
    new Vue({
        el: "#app",
        data: {
            name: ""
        }
    })
</script>
</html>
```

## 2.1 Vue 实例与数据绑定

### 2.1.1 实例与数据

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app"></div>
</body>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            a: 2
        }
    })
    console.log(app.a)
</script>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app"></div>
</body>
<script type="text/javascript">
    var myData = {
        a: 1
    }
    var app = new Vue({
        el: "#app",
        data: myData
    })
    console.log(app.a)
    app.a = 2
    console.log(myData.a)
    myData.a = 3
    console.log(app.a)
</script>
</html>
```

### 2.1.2 生命周期

https://vuejs.org/v2/api/#created

https://vuejs.org/v2/api/#mounted

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app"></div>
</body>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            a: 2
        },
        created: function () {
            console.log(this.a)
        },
        mounted: function () {
            console.log(this.$el)
        }
    })
</script>
</html>
```

### 2.1.3 插值与表达式

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{ book }}
    </div>
</body>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            book: "《Vue.js 实战》"
        }
    })
</script>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{ date }}
    </div>
</body>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            date: new Date()
        },
        mounted: function () {
            var _this = this
            this.timer = setInterval(function() {
                _this.date = new Date()
            }, 1000)
        },
        beforeDestroy: function () {
            if (this.timer) {
                clearInterval(this.timer)
            }
        }
    })
</script>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <span v-html="link"></span>
    </div>
</body>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            link: '<a href="#">这是一个连接</a>'
        }
    })
</script>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <span v-pre>{{ 这里的内容是不会被编译的 }}</span>
    </div>
</body>
<script type="text/javascript">
    var app = new Vue({
        el: "#app"
    })
</script>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{ number / 10 }}<br>
        {{ isOK ? '确定' : '取消' }}<br>
        {{ text.split(',').reverse().join(',') }}
    </div>
</body>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            number: 100,
            isOK: false,
            text: '123,456'
        }
    })
</script>
</html>
```

### 2.1.4 过滤器

https://vuejs.org/v2/api/#filters

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{ date | formatDate }}
    </div>

    <script type="text/javascript">
        var padDate = function (value) {
            return value < 10 ? '0' + value : value
        }

        var app = new Vue({
            el: "#app",
            data: {
                date: new Date()
            },
            filters: {
                formatDate: function (value) {
                    var date = new Date(value)
                    var year = date.getFullYear()
                    var month = padDate(date.getMonth() + 1)
                    var day = padDate(date.getDate())
                    var hours = padDate(date.getHours())
                    var minutes = padDate(date.getMinutes())
                    var seconds = padDate(date.getSeconds())
                    return year + '-' + month + '-' + day + ' ' + hours + ':' + minutes + ':' + seconds
                }
            },
            mounted: function () {
                var _this = this
                this.timer = setInterval(function() {
                    _this.date = new Date()
                }, 1000)
            },
            beforeDestroy: function () {
                if (this.timer) {
                    clearInterval(this.timer)
                }
            }
        })
    </script>
</body>
</html>
```

## 2.2 指令与事件

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <p v-if="show">显示这段文本</p>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                show: true
            }
        })
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <a v-bind:href="url">链接</a><br>
        <img v-bind:src="imgUrl">
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                url: 'https://www.github.com',
                imgUrl: 'https://vuejs.org/images/logo.png'
            }
        })
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <p v-if="show">这是一段文本</p>
        <button v-on:click="handleClose">点击隐藏</button>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                show: true
            },
            methods: {
                handleClose: function () {
                    this.show = false
                }
            }
        })
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <p v-if="show">这是一段文本</p>
        <button v-on:click="show = false">点击隐藏</button>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                show: true
            }
        })
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <p v-if="show">这是一段文本</p>
        <button v-on:click="handleClose">点击隐藏</button>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                show: true
            },
            methods: {
                handleClose: function () {
                    this.close()
                },
                close: function () {
                    this.show = false
                }
            }
        })
    </script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            methods: {
                init: function (text) {
                    console.log(text)
                }
            },
            mounted: function () {
                this.init('在初始化时调用')
            }
        })
        app.init('通过外部调用')
    </script>
</body>
</html>
```

## 2.3 语法糖

```html
<a v-bind:href="url">链接</a><br>
<img v-bind:src="imgUrl">
<!-- 缩写为 -->
<a :href="url">链接</a><br>
<img :src="imgUrl">

<button v-on:click="handleClose">点击隐藏</button>
<!-- 缩写为 -->
<button @click="handleClose">点击隐藏</button>
```