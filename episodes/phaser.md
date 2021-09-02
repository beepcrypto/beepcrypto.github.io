---
layout: episode
title: 如何用 Phaser 开发一个简单的网页游戏
---

# 如何用 Phaser 制作一款简单的网页游戏

我们估计 NFT 游戏接下来会迎来一个爆发期，这个视频里给大家演示一下如何快速上手 [phaser](https://phaser.io/) 。Phaser 是一套 HTML5 游戏开发框架。注意，它的特点就是可以跟 [Moralis](https://moralis.io/) 和 [Aavegotchi](https://aavegotchi.com/) 无缝配合使用。Moralis 是一个区块链开发平台，让前端开发者可以用 JS 接口直接调用用户的区块链信息。Aavegotchi 是 NFT 数字收藏品，一个很可爱的幽灵形象，可以直接出现在游戏里。不过这个视频里面我们先不碰 Moralis 和 Aavegochi ，只是先上手 Phaser 。

看完这个视频大家可以动手做出这样的一个游戏场景。有一个小幽灵站在一个平台上，我们可以用键盘控制它左右移动，可以让它跳起来，如果不小心它也会从平台上掉下来。

好下面来进入动手部分。首先说明一下，咱们这里演示的所有的代码，官方教程里面都有，到 [Phaser 官网](https://phaser.io)，点学习，然后可以看到官方教程了，这里有更详细的讲解，可以作为进一步的学习资料。

创建一个文件夹 `mkdir demo` ，用自己的编辑器打开这个文件夹，我用的是 vscode ，创建 index.html 文件。构建出 html 文件的基本骨架。

```html
<html>
  <head></head>
  <body>
    
  </body>
</html>
```

导入 Phaser 有多种方式，最简单的一种是使用 [CDN](https://phaser.io/download/stable) ，

```html
<script src="//cdn.jsdelivr.net/npm/phaser@3.55.2/dist/phaser.min.js"></script>
```

拷贝这一行到 html 文件的 head 标签之内。




属性type可以是 Phaser.CANVAS，或者 Phaser.WEBGL，或者 Phaser.AUTO。这是你要给你的游戏使用的渲染环境（context）。推荐值是Phaser.AUTO，它将自动尝试使用WebGL，如果浏览器或设备不支持，它将回退为Canvas

可以从 https://www.gameart2d.com/free-graveyard-platformer-tileset.html 下载一些游戏中需要的图片。

```js
  this.add.image(400, 300, 'background');
```

这样我们就把背景图片放到了画布的中心，因为整个画布是800乘以600的，而函数的参数定位的是背景图中心的坐标。
