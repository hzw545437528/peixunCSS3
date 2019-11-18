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

- OUTDOORS和下面的IS WHERE LIFE HAPPENS 字母与字母之间的间隔可以使用 **letter-spacing** 属性。

- 背景色使用的是 **background-image** 的 **linear-gradient** ，可百度，颜色代码为：

  ```css
  rgba(85, 197, 122, 0.8), rgba(40, 180, 133, 0.8)
  ```

- 刚进入页面时，标题，副标题，以及按钮会有一个进入的动画效果，可以看看 **@keyframes** 的使用方法，配合   **animation** 使用。

- 鼠标移到白色按钮上面，会有一个动画效果，移出时还有另一个动画效果，这个可以配合 **::after** 伪类元素来使用，提示：放大效果用**transform:scale**，消失用**opacity：0**实现。另外鼠标悬停在按钮上时，会有一个小的向上位移的动画，并且出现阴影。

- 右上角的小球始终固定在右上角，上下浏览页面也是。

- 右上角的小球里面的横线指定比较小的 **height** 和 **width**，**background-color **为黑色即可。中间的横线写好后，用伪类元素 **::before** 和 **::after** 写出上面和下面的黑线。指针移上去，上面和下面的黑线会分别平移向上向下平移。

- 背景的剪裁可以使用clip-path实现。

- 点击右上角小球，弹出菜单栏，三条横线变成‘X’，先说下变成‘X'的实现方法：

  首先要有一个checkbox选择框作为触发器，这个checkbox当然是要不可见的。

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

  同样用checkbox触发的方式，通过选择器选中绿球，并将他放大一个很大的倍数，比如80倍(transform: scale(80))，再指定一下transition参数，就可以了。

  菜单从左往右的弹出的实现：

  先给整个列表的宽度设为0，可见度为0(opacity)。

  点击白球后，宽度为100%，可见度为1。指定transition参数后，动画效果就实现了。

  参数：

  ```css
  transition: all .8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  ```

   ↓↓↓这个网址可以根据不同动画效果获得cubic-bezier的参数↓↓↓

​			 http://www.roblaplaca.com/examples/bezierBuilder/

  ![image](https://github.com/caiduncheng/peixun1/blob/master/img/image-20191113100206427.png?raw=true)

- 背景色代码:

  ```scss
  #f7f7f7
  ```

- 标题的颜色一样用linear-gradient，颜色代码：

  ```scss
  #7ed56f, #28b485
  ```

- 鼠标移动到标题上的效果可以用 **transform: skewX skewY scale** 来实现，阴影用text-shadow，参数：

  ```scss
  $color-black: #000000;
  text-shadow: .5rem 1rem 2rem rgba($color-black, .2);
  ```

- 鼠标放在图片上后，该图片会放大，其他图片会缩小。提示：可以用not选择器

- 标题下面的文本，可以去 https://lipsum.com/ 生成。

- Learn more 鼠标移上去的样式变化，比较简单。颜色

  ```scss
  #55c57a
  ```

![image](https://github.com/caiduncheng/peixun1/blob/master/img/%E6%8D%95%E8%8E%B7.PNG?raw=true)

- 阴影代码

  ```scss
  box-shadow: 0 1.5rem 4rem rgba(0, 0, 0, 0.15)
  ```

- 四个小卡片的背景色不是全白，有点透明，可以用rgba的第四个参数调整。

- 四个图标可以从iconfonts下载，使用方法可以看看官网。四个图标搜索关键字：world compass map heart

- 背景的变形效果用到了 **transform** 的 **skewY** ，没有用到clip-path。

- 鼠标放上去有向上平移和放大的效果

- 图标的背景色填充效果要用到 **-webkit-background-clip: text**

  ```scss
  background-image: linear-gradient(to right, #7ed56f, #28b485);
  color: transparent;
  -webkit-background-clip: text; /* 一定要放在后面 */
  ```

  ![image](https://github.com/caiduncheng/peixun1/blob/master/img/tours.PNG?raw=true)

- 背景色

  ```scss
  #f7f7f7
  ```

- 三张卡片的颜色代码：

  ```scss
  /* 第一张正面和反面背景色 */
  #ffb900, #ff7730
  /* 第一张 THE SEA EXPLORER 背景色 */
  rgba(255, 185, 0, 0.85), rgba(255, 119, 48, 0.85)
  /* 第二张正面反面背景色 */
  #7ed56f, #28b485
  /* 第二张 THE FOREST HIKER */
  rgba(126, 213, 111, 0.85), rgba(40, 180, 133, 0.85)
  /* 第三张 正反面*/
  #2998ff, #5643fa
  /* 第三张 THE SNOW ADVENTURE */
  rgba(41, 152, 255, 0.85), rgba(86, 67, 250, 0.85)
  ```

- 卡片正面上面的背景滤镜效果用到了css3的属性 **background-blend-mode: screen**

- 卡片的翻转效果用到了 **transform: rotateY** 以及新的属性 **perspective** ，也就是可以让元素获得透视效果，具体的值可以自己多改改，多观察它的变化。

- 点击BUY，背景色会逐渐变黑，这个可以用transition实现，然后弹出一个弹出框，这个弹出框的动画是等待背景开始变化后的一段时间才开始，这个可以用到transition-delay，动画延迟。该弹出框由小变大的动画效果是利用scale，让它的初始值变小，在显示时恢复它的大小，淡入的效果用opacity的变化。

- 弹出框的背景和窗口的隐藏是一开始将整个元素设为

  ```scss
  opacity: 0;
  visibility: hidden
  ```

  buy按钮设置为a标签，href属性设为弹出框父元素的id。

  给该父元素的伪类target设置样式，就能实现点击按钮显示弹出框。

  ```scss
  button:target {
      opacity: 1;
      visibility: visible;
  }
  ```

  ![image-20191118114601473](https://github.com/caiduncheng/peixun1/blob/master/img/image-20191118114601473.png?raw=true)

- 两个形状是通过 **transform: skewX** 实现。外部变形后，内部要反过来变形，否则内容会倾斜。

- 照片的圆形通过 **clip-path** 实现。

- 模糊效果用的是 

  ```scss
  filter: brightness(80%) blur(3px);
  ```

- 文字的浮现效果，用 **opacity** 和 **transition** 实现

- 外层阴影代码：

  ```scss
  box-shadow: 0 3rem 6rem rgba(0, 0, 0, 0.5)
  ```

![image](https://github.com/caiduncheng/peixun1/blob/master/img/form.PNG?raw=true)

- 绿色背景颜色代码

  ```scss
   #7ed56f, #28b485
  ```

- 内层表单实际上使用了两个背景，最下层的风景图，还有白色的梯形背景，样式比较长，我这里直接贴出来，可以调整一下各个参数的数值观察每个参数的作用

  ```scss
  background-image: linear-gradient(105deg, rgba(255, 255, 255, 0.9) 0%, rgba(255, 255, 255, 0.9) 50%, transparent 41%), url(../img/nat-10.jpg);
  ```

- 外部阴影代码

  ```scss
  0 1.5rem 4rem rgba(0, 0, 0, 0.5)
  ```

- 输入表单时，如果表单不符合规定，会出现红色的底部边框，如果正确则变为绿色。实现方法是：首先预先设置一个透明色的底部边框，否则输入时，底部边框突然出现会导致整体元素的位移。

  用 **:focus** 伪类来改变底部边框上色

  input标签有 **required** 属性，表示必填，**type=email** 则表示邮箱格式的检验。

  css有 **:invalid** 伪类元素，会选中不符合表单规则的元素

  表单相关颜色代码

  ```scss
  /* 绿色底部边框 */ 
  #55c57a
  /* 红色底部边框 */
  #ff7730
  /* 表单获取焦点的阴影 */ 
  0 1rem 2rem rgba($color-black, .1);
  ```

- 输入表单表单不为空时，表单的placeholder内容会偏移上去，这是用input的 **placeholder** 属性和另一个有同样内容的元素配合。初始状态先通过 **:placeholder-shown** 伪类和 **+** 选择相邻的文本元素，使它隐藏。

  >  :*placeholder-shown* CSS 伪类 在 input 或 textarea 元素显示 placeholder text 时生效. 

  ```scss
   &__input:placeholder-shown + &__label {
         opacity: 0;
         visibility: hidden;
         transform: translateY(-4rem);
      } 
  /* 注意，label自身的样式不能写在上面，否则输入文字后会失效。 */
  ```

  当输入文字后，该伪类选择器失效，再去写，在label自身的样式中加上

  ```scss
  transition: all .3s;
  transform: translateY(-9rem);
  ```

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