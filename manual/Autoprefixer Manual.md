# Autoprefixer Manual

https://github.com/postcss/autoprefixer

https://www.npmjs.com/package/autoprefixer

## [Usage](https://github.com/postcss/autoprefixer#usage)

`webpack.config.js`
```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ["style-loader", "css-loader", "postcss-loader"]
            }
        ]
    }
}
```

`postcss.config.js`
```javascript
module.exports = {
  plugins: [
    require('autoprefixer')
  ]
}
```