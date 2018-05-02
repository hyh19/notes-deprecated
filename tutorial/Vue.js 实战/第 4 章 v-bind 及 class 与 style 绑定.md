# 第 4 章 `v-bind` 及 `class` 与 `style` 绑定

## 4.1 了解 `v-bind` 指令

略

## 4.2 绑定 `class` 的几种方式

### 4.2.1 对象语法

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <div :class="{ 'active': isActive }"></div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                isActive: true
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
        <div class="static" :class="{ 'active': isActive, 'error': hasError }"></div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                isActive: true,
                hasError: false
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
        <div :class="classes"></div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                isActive: true,
                error: null
            },
            computed: {
                classes: function () {
                    return {
                        active: this.isActive && !this.error,
                        'text-fail': this.error && this.error.type === 'fail'
                    }
                }
            }
        })
    </script>
</body>
</html>
```

### 4.2.2 数组语法

```
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <div :class="[activeCls, errorCls]"></div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                activeCls: 'active',
                errorCls: 'error'
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
        <div :class="[isActive ? activeCls : '', errorCls]"></div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                isActive: true,
                activeCls: 'active',
                errorCls: 'error'
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
        <div :class="[{ 'active': isActive }, errorCls]"></div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                isActive: true,
                errorCls: 'error'
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
        <div :class="classes"></div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                size: 'large',
                disabled: true
            },
            computed: {
                classes: function () {
                    return [
                        'btn',
                        {
                            ['btn-' + this.size]: this.size !== '',
                            ['btn-disabled']: this.disabled
                        }
                    ]
                }
            }
        })
    </script>
</body>
</html>
```

### 4.2.3 在组件上使用

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <my-component :class="{'active': isActive}"></my-component>
    </div>

    <script type="text/javascript">
        Vue.component('my-component', {
            template: '<p class="article">一些文本</p>'
        })
        var app = new Vue({
            el: "#app",
            data: {
                isActive: true
            }
        })
    </script>
</body>
</html>
```

## 4.3 绑定内联样式

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <div :style="{ 'color': color, 'fontSize': fontSize + 'px' }">文本</div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                color: 'red',
                fontSize: 14
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
        <div :style="styles">文本</div>
    </div>

    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            data: {
                styles: {
                    color: 'blue',
                    fontSize: 14
                }
            }
        })
    </script>
</body>
</html>
```
