##div之间的间隔

div之间之所以有间隔，有时候不是div的问题，而是div内部标签的问题。

- 1. div内部有img/h2/h3...等标签，这些标签在div的第一个子元素位置或者最后一个子元素位置时，要清除这些元素的margin为0： margin: 0;或者设置img图片的font-size=0或者display:block也可以。
- 2. div 有时候设置了height值，而且也用了背景图片，这个时候也可能会出现间隔。因为背景图片如果只是repeat-x而没有 repeat-y 的话，也会出现两个div之间有间隔，因为图片高度没有设置的height大所以也会出现间隔。这个时候可以行当减少height的值。


