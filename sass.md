$fontSize: 12px !default; //默认

$color: red !global; //全局变量

$maps: (color: red, borderColor: blue)
color: map-get($maps, color);

$className: main
.#{$className}{
  width: 50px;
}

$paddings: 3px, 10px, 5px, 10px;
padding: nth($padding, 1);
padding: 3px;

导入
@import 'css.css';
@import "http://ss/xx"
@import url()

@import "part1.scss"

### 继承和嵌套

属性嵌套
footer {
  background: {
    color: red;
size: 100% 50%;
  }
}