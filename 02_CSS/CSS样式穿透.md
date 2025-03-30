# CSS样式穿透

通常在引入第三方UI组件库时，需要自定义组件样式，由于组件使用`scoped`属性（避免组件间样式相互影响），导致无法直接覆盖原组件样式，这时使用样式穿透。

## 1.使用`>>>`符号

```css
.container >>> .el-input__inner {
    width: 160px;
}
```

适用的样式语法：`css`、`stylus`

## 2.使用`/deep/`

```css
.container /deep/ .el-input__inner {
    width: 160px;
}
```

该方法适用的样式语法：`sass`、`scss`、`less`

## 3.使用`::deep`

```css
::v-deep .el-input__inner {
    width: 160px;
}
```

该方法对所有样式语法通用，即适用于 `css`、`stylus`、`sass`、`scss` 和` less`

其中`/deep`和`::v-deep`属于深度选择器。