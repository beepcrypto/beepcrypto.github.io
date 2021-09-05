---
layout: episode
title: 如何用 Phaser 开发一个简单的网页游戏
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/yOxpMg176S4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

我们估计 NFT 游戏接下来会迎来一个爆发期，这个视频里给大家演示一下如何快速上手 [Phaser](https://phaser.io/) 。Phaser 是一套 HTML5 游戏开发框架。注意，它的优势是可以跟 [Moralis](https://moralis.io/) 和 [Aavegotchi](https://aavegotchi.com/) 无缝配合。Moralis 是一个区块链开发平台，让前端开发者可以用 JS 接口直接调用用户的区块链信息。Aavegotchi 是 NFT 数字收藏品，一个很可爱的幽灵形象，可以直接出现在游戏里。不过这个视频里面我们先不碰 Moralis 和 Aavegochi ，只是先上手 Phaser 。

看完这个视频大家可以动手做出这样的一个游戏场景。有一个小幽灵站在一个平台上，我们可以用键盘控制它左右移动，可以让它跳起来，如果不小心它也会从平台上掉下来。首先说明一下，咱们这里演示的所有的代码，官方教程里面都有，到 [Phaser 官网](https://phaser.io)，点学习，然后可以看到官方教程了，这里有更详细的讲解，可以作为进一步的学习资料。


## 基本配置

好下面来进入动手部分。先把项目的基本骨架搭建起来。

创建一个文件夹 

```
mkdir demo
```

用自己的编辑器打开这个文件夹，我用的是 vscode ，创建 index.html 文件。构建出 html 文件的基本骨架。

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

```
serve .
```

然后启动本地服务器，用 Chrome 浏览器打开本地链接，在开发者工具的 console 中输入 `Phaser` 回车，可以看到 Phaser 已经加载成功了。

下面对 Phaser 进行一下基本配置。在 html 页面的 body 标签下添加 script 标签，我们的主体代码会写到这里面。

```js
var config = {
  type: Phaser.AUTO,
  width: 800,
  height: 600,
  scene: {
    preload: preload,
    create: create,
    update: update
  }
};
```

属性 type 可以是 Phaser.CANVAS，或者 Phaser.WEBGL，或者 Phaser.AUTO。这是要给你的游戏指定渲染环境。推荐值是 Phaser.AUTO ，它会自动尝试使用 WebGL，如果浏览器或设备不支持，它将回退为 Canvas 。然后画布的大小设置为800x600像素。然后给场景设置三个函数。

```js
var game = new Phaser.Game(config);

function preload()
{
}

function create()
{
}

function update()
{
}
```

接下来在初始化 Phaser 的时候加载这个配置，然后把刚才那三个函数的空壳子先放到这里。稍后我们会逐一介绍它们的作用。

## 添加图片

看看如何让游戏显示一张图片呢。

可以从 [gameart2d](https://www.gameart2d.com/free-graveyard-platformer-tileset.html) 下载一些游戏中需要的图片。解压后放到项目的 `assets` 文件夹下。做完之后，我们的项目有这样的树形结构。

显示图片需要用到 preload 和 create 这两个函数。preload 顾名思义就是在程序启动之前先去加载一下资源，例如音频或者图片。而 create 就是要构建游戏场景的画面了，所以到底图片如何显示，显示到什么位置，都是在 create 函数中去执行的。注意，这两个函数都是在游戏启动时会被环境自动执行的，不需要我们去调用。

先来加载图片

```js
function preload ()
{
  this.load.image('background', 'assets/bg.png');
}
```

两个参数，一个是给这个图片起个名字，方便后面调用，这里叫 `background` ，第二个参数就是图片路径了，是刚才我下载的一张大背景图。

那到底显示到什么位置呢？

```js
this.add.image(400, 300, 'background');
```

这就要到 create 函数中去写了，三个参数，前两个是图片中心点的横纵坐标，最后一个是图片的名字。浏览器中可以看到，背景图片放到了画布的中心，因为整个画布是800乘以600的。400和300刚好是中心位置。

这样，我们的游戏中就显示出一张图片了。我们也算完成了一项肉眼可以的进展，是不是很有成就感呢？

## 设置动态人物

但是接下来要做的事情就更酷了。游戏中人物肯定是要动的，那么怎么去添加一个可以动起来的游戏人物呢？

```js
physics: {
  default: 'arcade',
  arcade: {
      gravity: { y: 300 },
      debug: false
  }
}
```

一个游戏人物是要处于一个物理场景下的。到配置部分添加一个名为 arcade 的物理状态。重力设置 y 方向，值为正整数，那么人物就会被来自下方的引力所吸引，很显然，这里也允许配置让引力来自上方或者水平方向，你自己可以试试把这里的 `y` 改成 x ，或者把后面的数字改成负值，去试试看。debug 这一项的作用稍后会看到，暂时设置成 false ，没有开启调试模式。

Arcade 这个物理场景下有两种类型的物体，一种是静态的，另外一种是动态的。静态的物体是不受外力影响的，总是静静的原地不动。动态的物体，可以有速度，会受到重力影响，可以跟其他物体发生碰撞。

现在添加一个静态物体。

```js
var platforms;
```

添加一个全局变量来存放静态物体。

```js
this.load.image('ground', 'assets/tile.png');
```

跟刚才添加普通图片的流程有些类似，到 preload 函数中，先加载图片。

```js
platforms = this.physics.add.staticGroup();
platforms.create(470, 400, 'ground');
```

然后到 create 函数中，来添加一个静态物体，设置合适的坐标摆放一下。

浏览器可以看到，图片有点太大了。

```js
platforms.create(470, 400, 'ground').setScale(0.5)
```

所以添加 `setScale` 把大小变成原来的50%。


```js
 debug: true
```

但是现在隐含了一个问题。把 debug 模式打开，可以看到虽然图片缩小了，但是原有的区域，还是被占据着，所以如果未来有动态的人物站在上面，那么人物就是悬空的。


```js
platforms.create(470, 400, 'ground').setScale(0.5).refreshBody();
```

可以添加 `refreshBody` 进来，这样，实际区域就和图片吻合了。

```js
platforms.create(470, 400, 'ground').setScale(0.5).refreshBody();
platforms.create(533, 400, 'ground').setScale(0.5).refreshBody();
platforms.create(594, 400, 'ground').setScale(0.5).refreshBody();
platforms.create(657, 400, 'ground').setScale(0.5).refreshBody();
```

接下来，设置合适的坐标值，用这张图片摆成一排，形成一个平台。

好，静态物体添加好了，下面来添加动态的人物。

```js
var player;
```

声明一个全局变量。

```js
this.load.image('player', 'assets/player.png');
```

preload 中加载图片。

```js
player = this.physics.add.sprite(500, 250, 'player').setScale(0.3).refreshBody();
```

用 `add.sprite` 来添加一个动态物体。和静态物体一样，也设置坐标，缩放，然后 refreshBody 。

浏览器中看一下，我们希望的效果肯定是人物能站到平台上，但是实际却会掉下来。

```js
this.physics.add.collider(player, platforms);
```

需要做的是到 create 函数中添加这一句，这样，这两个物体之间就会碰撞，而不是直接忽视。这样，人物就可以站到平台上了。

但是如何让这个人物动起来呢？这里的主角就是 update 函数了。一个游戏之所以人眼看起来图像可以平滑移动，就是因为场景会在一秒之内更新很多次。update 函数会在游戏启动后，每秒被执行60次。正好可以用来存放一些对游戏进行动态控制的语句。

```js
var cursors;
```

首先，定义一个全局变量 cursors 。

```js
cursors = this.input.keyboard.createCursorKeys();
```

接下来， create 函数里面添加这一条，让 cursors 获取到键盘状态。


```js
if (cursors.left.isDown)
{
  player.setVelocityX(-160);
}
else if (cursors.right.isDown)
{
  player.setVelocityX(160);
}
else {
  player.setVelocityX(0);
}

```

最后要在 update 中添加的语句就很直观了。如果左方向键被按下，那就让人物获得一向左的速度，如果右键按下那就速度向右，抬起左右键的时候，让速度归零。

还有一种情况就是我们希望人物能跳起来。

```js
player.setBounce(0.4);
```

create 函数中添加一下落地反弹效果，0.4 是反弹力度，这是一个动画了，一会儿浏览器中就会看到。

```js
if (cursors.up.isDown && player.body.touching.down)
{
    player.setVelocityY(-330);
}
```

我们希望当上方向键按下去的时候，人物获得一个向上的速度。由于重力的作用，这个速度最终会归零，然后为负，也就是说人物最终会重新落地。这是一个典型的跳跃了，但是我们可能不希望的是，人物的脚已经离地了，还能继续踩着空气往上跳。而添加 `player.body.touching.down` 就可以保证，只有当人物的下方是跟其他物体接触的时候，换句话说，就是人物的脚是踩在平台上的时候，按上方向键才会有向上的速度。我们用到的具体的接口介绍可以参考官网的 [API 文档](https://newdocs.phaser.io/docs/3.55.2/Phaser.Physics.Arcade.Body#touching)。


## 总结

现在，把 `debug` 改成 false ，这样浏览器中可以看到，我们可以正常控制人物了，达成了这集视频我们想要达成的效果。我们学到了，如何给游戏添加图片，设置静态和动态物体，并且让这两类物体发生碰撞关系，学会了用键盘控制人物的运动。但是我们还没有碰的是，如何让游戏可以读取区块链信息，或者在游戏中价值一些 NFT 资源。这些任务我们在后续的视频中会有介绍。我是 Peter ，下个视频我们再见，拜拜。