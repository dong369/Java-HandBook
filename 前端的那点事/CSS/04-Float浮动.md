# 1. 认识浮动

浮动会让父元素的高度塌陷！浮动原本的作用是实现文字的环绕效果！因为浮动会使得文档中的元素脱离文档流，继而不占用文档流的空间。

## 1.1 包裹

收缩、坚挺、隔绝，江湖人称BFC-块级格式化上下文。（display、position、overflow）

## 1.2 破坏

（display、position）

# 2. 清除浮动

## 2.1 clear

底部插入clear:both;

clea通常应用飛式

html block水平元素底部走起<dv….></div>
CSS after伪元素底部生成
clearfix： after f

## 2.2 BFC/haslayout

BFC/ haslayout通常声明
float: left/right
position: absolute/fixed
overflow: hidden/scroll(IE7+
display: inline-block/table-cell(IE8+)
width/height/zoom: 1/.(IE6/IE7)

.clearfix应用在包含浮动子元素的父级元素上。

乱入的 haslayout往往会让IE6/E7做出出格的事情！
浮动也会触发 haslayout，所以，浮动在E67下更
显魔性！
但浮动普遍滥用了！

直教浮动生死相许
1.元素bock块状化（砖头化）
2.破坏性造成的紧密排列特性（去空格化）

文字环绕衍生一单侧固定