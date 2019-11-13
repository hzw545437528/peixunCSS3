# 培训任务一：静态页面制作

### 简介

用html5 + css3 + sass 制作单个静态页面，本任务不涉及js。

### 目的

- 熟悉CSS3各种新属性
- CSS动画的运用
- 学习sass使用方法以及编译成css
- 学习npm
- 学习基本的布局方式
- 学习CSS命名(可以看看BEM命名)
- 简单的响应式布局
- CSS选择器
- iconfont的使用

### 效果预览

打开项目里的example/index.html查看（提供效果预览，尽量不要照着样式写）

### 文件结构（参考）

├── img        //图片（要用到的图片都放在里面了）

├── node_modules      //依赖

├── sass       //sass样式（编写的样式都写在这里）

​		 ├──  _animation.scss   //动画

​		 ├──  _variable.scss   //变量（颜色等）

​		 ├──  _base.scss	//全局样式

​		 ├──  _mixin.scss   //主要放@mixin代码

​		 ├──  _main.scss   //主要的样式文件，可以将其他样式文件的样式import进来，方便编译。

​		 ├──  components  //可根据需要再细分样式文件

​		 └──   layout  //可根据需要再细分样式文件

├── css          //编译后的css，由node自行创建

└── index.html	主页

### 详解

全局样式：

```css
body {
    box-sizing: border-box;
    padding: 3rem;
}

*,
*::after,
*::before  {
    margin: 0;
    padding: 0;
    box-sizing: inherit;
}

html {
    font-size: 62.5%; // 1rem = 10px, 10/16 = 62.5 %

}
```

建议使用rem作为单位，对之后响应式的设计有帮助。此时1rem = 10px



![image-20191112155542455](https://github.com/caiduncheng/peixun1/blob/master/img/image-20191112155542455.png?raw=true)

头部header有以下要点：

- OUTDOORS和下面的IS WHERE LIFE HAPPENS 字母与字母之间的间隔可以使用letter-spacing属性。

- 背景色使用的是background-image的linear-gradient，可百度，颜色代码为：

  ```css
  rgba(85, 197, 122, 0.8), rgba(40, 180, 133, 0.8)
  ```

- 刚进入页面时，标题，副标题，以及按钮会有一个进入的动画效果，可以看看 **@keyframes** 的使用方法，配合   **animation** 使用。

- 鼠标移到白色按钮上面，会有一个动画效果，移出时还有另一个动画效果，这个可以配合 **::after** 伪类元素来使用，提示：放大效果用**transform:scale**，消失用**opacity：0**实现。

- 右上角的小球始终固定在右上角，上下浏览页面也是。

- 右上角的小球里面的横线指定比较小的height和width，background-color为黑色即可。中间的横线写好后，用伪类元素 **::before** 和 **::after** 写出上面和下面的黑线。鼠标移上去，上面和下面的黑线会分别平移向上向下平移。

- 背景的剪裁可以使用clip-path实现。

- 点击右上角小球，弹出菜单栏，三条横线变成‘X’，先说下变成‘X'的实现方法：

  首先要有一个checkbox选择框作为触发器。

  ```html
  <input type="checkbox" class="navigation__checkbox" id="navi-toggle">
  <label for="navi-toggle" class="navigation__button">
  	<span class="navigation__icon">&nbsp;</span>
  </label>
  ```

  点击白球，也就是上面的label，checkbox会变为选中状态

  通过选择器选择选中状态的checkbox的兄弟元素的子元素。

  ```scss
  .navigation {
  	&__checkbox:checked + &__button &__icon {
          //...
      }

      &__checkbox:checked + &__button &__icon::before {
          //...
      }

      &__checkbox:checked + &__button &__icon::after {
         //...
      }
  }
  ```

  在选中状态下，将中间的黑线隐藏(可以使用background: transparent)。

  上下两条黑线旋转，使用transform: rotate

  再来说说点击后背景色逐渐放大，并遮住主页面的效果。

  首先写出一个绿色的球，尺寸略小于白球，将它放在白球下面。

  绿球颜色代码（用background-image的radial-gradient）：

  ```scss
  #7ed56f, #28b485
  ```

  ![image-20191113085122759](https://github.com/caiduncheng/peixun1/blob/master/img/image-20191113085122759.png?raw=true)

  同样用checkbox触发的方式，通过选择器选中绿球，并将他放大一个很大的倍数，比如80倍(transform: scale(80))。

  菜单从左往右的弹出的实现：

  先给整个列表的宽度设为0，可见度为0(opacity)。

  点击白球后，宽度为100%，可见度为1。指定transition参数后，动画效果就实现了。

  参数：

  ```css
  transition: all .8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  ```



  ![image](https://github.com/caiduncheng/peixun1/blob/master/img/image-20191113100206427.png?raw=true)

要点：

- 标题的颜色一样用linear-gradient
- 鼠标移动到标题上的效果可以用 **transform: skewX skewY scale** 来实现，阴影用text-shadow
- 鼠标放在图片上后，该图片会放大，其他图片会缩小。提示：可以用not选择器
-

### 运行

```
npm install
```

我已经将编译的命令写好

```json
 "scripts": {
    "start": "node-sass sass/main.scss css/style.css -w"
  }
```

运行

```
npm run start
```

会把main.scss编译为style.css，后面的参数-w表示运行start后，程序会监听你的文件的保存行为，每保存一次就会自动编译，省去了每次写好手动编译的麻烦。可以配合vscode的liveserver来使用，这样不需要每次编译后都刷新。
