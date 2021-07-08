# 清除浮动

> 当应用于非浮动块时，它将非浮动块的[边框边界](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)移动到所有相关浮动元素[外边界](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)的下方。这个非浮动块的[垂直外边距](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)会折叠。
>
> 另一方面，两个浮动元素的垂直外边距将不会折叠。当应用于浮动元素时，它将元素的[外边界](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)移动到所有相关的浮动元素[外边框边界](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)的下方。这会影响后面浮动元素的布局，后面的浮动元素的位置无法高于它之前的元素。
>
> 要被清除的相关浮动元素指的是在相同[块级格式化上下文](https://developer.mozilla.org/en-US/docs/CSS/block_formatting_context)中的前置浮动。



* clear: right  元素右边不能有浮动元素，该元素会向下移动
* clear: left 元素左边不能有浮动元素，该元素会向下移动
* clear: both 元素左右两边都不能有浮动元素，该元素会向下移动