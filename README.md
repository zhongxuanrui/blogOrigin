# creater_notes
项目日常笔记

---
### 一些坑
#### 语言组织比较口水
* v2.1.0
* 1.编辑器缓存，在多人维护项目情况下更新资源编辑器缓存，不能在第一时间获取最新资源：解决方法：重启编辑器，有必要情况删掉本地配置文件。

* 2.UUID：资源对应的唯一id  同一资源在多处复用meta不能复制。

* 3.音频：音乐和音效的声音大小归于setVolume()，但是在setVolume()设置为0 的情况下，仍然可以获取到cc.audioEngine.getEffect()不为0；

* 4.label font 右边被裁减 动态在string 解决方法： 后面加空

* 5.label font 设置字体大小后顶部被裁减，解决方法：将lineHeight  设置为大于字高几个像素

* 6.drawCall 父级下面不同节点引用统一个纹理资源，在中间有引用其他纹理资源的精灵的情况下，因为drawcall 切换 导致引用统一纹理资源的精灵不一样。解决方法：将引用同一纹理的精灵放置在相邻节点，一方面可以节省darwcall

* 7.scrollView 在执行scrollTop()方法的时候，如果上一个动作还在滑动中，执行无效

* 8.同一个node节点，替换尺寸不一样的纹理之后记得修改宽高，编辑器不会自己更新

* 9.从缓存次读取的节点，如果在一开始创建的时候需要清空事件，不然会出现多个事件

* 10.内存释放问题 举个例子：loadRes(“aaaa”); loadRes(“bbb”) releaseRes(“aaa”)，如果bbb这个预制体或者其他复合资源，引用到了aaa或者aaa里面的某个依赖资源，那么在释放aaa后，bbb就会直接出错

*  11.效率优化：告别系统字，用bmfont代替。

* 12.告别使用shrink。造成卡顿，不断渲染造成。

* 13.scrollview 1.分帧创建item2.不可见的隐藏起来。3.根据需要创建其他item.

* 14.对于大量的预制体，使用分帧创建。在顺畅体验和及时体验（卡顿一会全部东西出来）中做权衡取舍

* 15.优化渲染效率，禁用在屏幕外不确定可见的render组件。如果代码是视图与逻辑分离的，将active设置为false

---
### 优化
#### 设置优化策略
* 在v1.10.0中加入了’优化策略‘选项，能优化所选预制的实例化时间，也就是执行cc.instantiate所需的时间，可设置的值有：
** 自动调整
<设为这个选项后，引擎将根据创建次数自动调整优化策略，初次创建实例时，等于“优化单次创建性能”,多次创建后将自动动“优化多次创建性能”>
** 优化单次创建性能
<该选项会跳过针对这个预制的代码生成优化操作>
** 优化多次创建性能
<该选项会启用针对这个预制的代码生成优化操作>
* 如果这个预制需要反复执行 cc.instantiate 请选择优化“多次创建性能”,否则保持默认的“自动调整”

---
#### 性能优化注意事项 
* 使用单独存在的Texture 作为 sprite 资源，在预览和发布游戏时，将无法对这些 sprite 进行批量渲染优化的操作。目前编辑器不支持转换原有的单张 texture 引用到 atlas 里的 spriteframe 引用，所以在开发正式项目时，应该尽早把需要使用的图片合成 atlas（图集），并通过 atlas 里的 spriteframe 引用使用，

---
## creater
### 网络
#### 多线程的使用 
* 资源的加载或者下载。网络消息的手法

---

#### 如何判断断网还是弱网
* 心跳检测

---

#### CCImage 和 CCTexture 的区别？
* CCImage 保存的是内存数据
* CCTexture 保存的是显存数据

---

#### CCNode 中的 visit 和 draw 方法的区别？
* visit 渲染前准备
* draw 具体绘制

---

#### 2.x和  3.x 渲染上最大的变化是什么？
* 场景树到渲染队列

---

#### A-B 场景切换的步骤是什么，怎样避免内存峰值？
* - B-init   B-Enter  A-Exit  A-Clean
* - 过度场景

---

#### 如何知道当前的网络类型是 wifi 还是 4G？
* 分平台实现 NetworkInfo

---

#### 如何进行设备同一标识，避免重复注册？
* 利用 IMEI、MAC、UUID 生成用户标识

---

#### clippingNode 的图形学原理？
* 模板缓冲

### CocosCreator 和 cocos2d-x 的区别
* 实现不同，Creator 引擎是采用 cocos2d-x-lite + jsb 和 js engine 分别实现原生和h5平台
* cocos2d-x-lite 是基于cocos2d-x 3.9 的简化版，2d-x基本属于代码驱动
* Creator优化了相关工具链，提高了开发效率
* cocos creator是一套包含编辑器在内的开发框架，其内部引擎使用了cocos2d-x js的精简修改版本
* cocos2d-x 资源使用了引用计数进行管理，Creator 因为 js 的自动垃圾回收无法使用引用计数

---

### CocosCreator 如何实现多平台发布的
* 原生平台使用 cocos2d-x-lite + jsb ，h5平台使用js engine运行于浏览器环境

---

### Creator 中需要动态载入的资源，放在工程的哪个子目录中 
* resources

---

### 资源动态加载和非动态加载的区别
* 使用 cc.loader 加载的属于动态加载，直接在场景上拖上去的属于非动态加载
* 非动态加载资源参与场景销毁时的资源自动释放，动态加载不会，需要手动释放资源
* 需要动态载入的资源，必须放在工程的resources目录中 

---

### 释放资源时要注意哪些问题
* 需要注意被释放的资源是否被引用，否则会渲染出错

---

### 生命周期回调有哪些，触发的时机
*  onLoad   组件首次激活时触发
*  start    组件第一次执行 update 之前触发
*  update     每一帧渲染前调用
*  lateUpdate 所有组件 update 调用后调用
*  onDestroy  组件或所在节点调用了 destroy() 时调用，并在当前帧结束时统一回收组件
*  onEnable  组件的enabled属性从 false 变为 true 时
*  onDisable  组件的 enabled 属性从 true 变为 false 时

---

### Creator 对齐 UI 使用什么组件
* widget 组件

---

### Creator UI 屏幕适配有哪些心得
* widget 组件能有效帮助屏幕适配
* 非异型屏幕的适配只需要合理使用 widget 组件将 UI 元素贴边摆放
* 异型屏幕的适配要点在于获取适配偏移值 offset ，然后使用 offset 动态的设置 widget 组件的 up 、down 、 left 、 right 的值，然后执行对其操作即可

---

### 游戏性能指标有哪些
* 内存占用
* CPU占用
* Draw Call
* FPS

---

### draw call 如何优化，如何合并
* 合并 draw call 的规则是这样的：
  * render command 相邻，大致可以理解为节点树中顺序相邻
  * texture 一致
  * blend function 一致
  * shader program 一致
  * label 会打断drawcall合并，所以尽量使可以合并的节点相邻，避免 label 在中间打断，或使用 BitMap 字体

---

### spine动画能否合并 draw call
* 不能

---

### 被Mask组件裁切不在显示区域内的节点会占用 draw call 吗
* 会

---

### 如何优化 ScrollView
* 隐藏显示区域外的节点，active 为 false 的节点不占用 draw call
* 复用列表项，根据 content 移动的位置动态的更新内容
* 使用对象池管理列表项
* 参照 draw call 的合并规则，合理规划列表项中的渲染节点

---

### 如何进行资源和内存优化
* 图片压缩有利于减小包体大小，但不能减小内存占用
* 缩小图片尺寸能减少内存占用
* 合理使用图集能够合并 draw call
  * 引擎提供的动态图集能够在运行时实时合图集
  * 编辑器提供自动图集功能，但合图的算法不如 TexturePacker 好
* 动画方面权衡序列帧和骨骼动画的使用
  * 序列帧动画有利于 draw call 合并但内存不经济
  * 骨骼动画内存经济且美术效果好，但不能合并 draw call
* 使用合理的策略进行资源的加载和释放
  * 资源规划时必须各个模块使用的资源隔离开，避免释放了被引用的资源，公共部分放在一起
  * 将不需要用到的资源释放
* 合理使用过渡场景避免内存峰值

---

### 一般而言造成FPS低的直接原因是什么，如何优化 FPS
* 原因是主循环中游戏逻辑占用太多时间
  * 比如大量频繁的创建和销毁节点
  * 大量频繁的加载和释放资源
  * 过多的逻辑操作运算等
* 优化方法是压缩运算量、采用分帧操作避免卡帧，使用对象池

---

### 使用全局单例要注意哪些问题
* 需要格外注意释放

---

### 逻辑开发如何解耦
* 合理使用事件系统有助于逻辑解耦

---

### a 节点和 b 节点非父子关系，如何获取 a 相对 b 节点的坐标

```
b.convertToNodeSpaceAR(a.convertToWorldSpaceAR(cc.Vec2.ZERO))
```

---

### 如何使代码块在一帧执行
* `scheduleOnce(CallBackFunction, 0);`

---

#### Creator meta 文件的作用，编辑项目的时候 关于 meta 文件需要注意什么
* meta 文件是 Creator 编辑器在导入资源文件（asset）时创建的，用于记录引擎使用该资源时所需的数据
* 每个资源文件/文件夹都在成功导入到 assets 中之后，都会生成一个对应的 meta 文件
* 不同类型的资源，meta 文件的内容是不同的。但是每个 meta 都一定会有的两个属性
  * uuid：该资源的唯一标识符，meta 文件创建时随机生成
  * ver：meta 文件的版本号，由资源对应的 meta 类定义，主要用于判断资源在不同版本的 Creator 编辑器中是否需要重新导入
* 其他的属性都是由资源类型对应的 meta 类定义的
* 请不要随意的直接在系统的文件夹中删除 meta 文件，删除资源务必在编辑器中操作，主要原因是：
  * meta 文件中的 uuid 是创建时随机生成的，而且 uuid 是资源的唯一标识符
  * 资源文件之间的引用都是依赖于 uuid 的，例如场景中使用了一张图片资源，那么在这个场景的 fire 文件中，会记录这个图片资源的 uuid
  * 一旦删除了 meta 文件，那么这个资源的 uuid 就发生了变化，之前使用这个资源的地方将会遇到无法找到资源的问题。

---

### Creator 中销毁节点要使用什么方法， removeFromParent 和 destroy 有什么区别，调用 destroy 时会立即销毁节点回收组件吗
* 使用destroy
* removeFromParent方法执行的操作是从从父节点上移除这个节点，执行完后会立即从父节点上移除，最终会销毁节点，但并不会触发onDestroy回调，执行removeFromParent操作之后可以再为节点指定一个新的父节点
* destroy调用后不会立即从父节点上移除，而是在当前帧结束的时候移除并销毁节点，并会触发onDestroy回调，操作之后不能再为节点指定一个新的父节点。
* [参考资料1](https://forum.cocos.com/t/removefromparent-destroy/38396/10?u=1712655110)
* [参考资料2](https://forum.cocos.com/t/removefromparent-destroy/38396/14?u=1712655110)

---

### Creator 中 get set 方法有什么作用
* 使用 . 操作符可以调用 get 方法
* 使用 = 操作符可以调用 set 方法
* 一般用在关联数据与UI或者关联数据与数据这方面

---

### 一张 1024x1024,32 位的贴图，在内存里面占多少字节？
* 1024 * 1024 * 32 / 8 = 4M 字节

---

### 使用对象池时，想要在节点回收或者复用时执行某些操作，有什么方法？
* 在 put 和 get 方法调用的地方执行
* 创建对象池时指定组件，在组件的 unuse 和 reuse 回调中执行

### 27. Creator 中如何加载二进制文件
```
let url = cc.url.raw('resources/FishPathFormated') + '.fpf';
cc.loader.load(
    { url: url, type: 'binary'},
    (err, binary: Uint8Array) => {
        if (err) {
            console.error(err.message || err);
        } else {
            // todo
        }
    }
);
```


----------------------------------------

## JS 和 TS 问题

### 1. js 对象引用传递会造成哪些问题，如何避免
* 引用传递可能会导致对象在不知名的地方被改变
* 避免问题需要合理使用只读属性或使用深拷贝传递对象

---

### 2. 删除数组中一些值或者遍历删除一些子节点的时候应该采用什么方法
* 后序遍历删除，或记录对应节点引用之后删除

---

### 3. 函数中 this 指向什么
* this 指代函数的运行环境
* 使用 . 操作符调用函数时，this 指操作符的左值
* call, apply, bind 都可以改变函数执行时的运行环境，即 this 的指向

---

### 4. 在脚本开发中如何实现同步
* 使用 promise
```
public async loadRes(url:string) {
    return new Promise<typeof cc.Asset>((resolve, reject) => {
        cc.loader.loadRes(url, (err, asset) => {
            if (err) {
                cc.error(err.message || err)
                reject(err.message || err)
            } else {
                resolve(asset)
            }
        })
    })
}
```

---

### 5. ts 与 js 有什么区别，为什么用 ts 不用 js
* TS 是 JS 的一个超集
* TS 的优势在于类型检查和代码提示
* TS 在最终会被编译成 js

---

### 6. 如何实现对象深拷贝
* `JSON.stringfy()`
* `JSON.parse()`
* `Object.assgin()`
* `Array.slice()`

---

### 7. ES6 为什么要引入 let 关键字
* 因为要解决 var 声明对象产生的问题
* var 是函数级作用域，而let是块作用域
* var 存在变量提升，即变量可以在声明之前使用，值为undefined，而let声明的变量如果在声明之前使用会抛出一个错误
* 另外let不允许重复声明变量
* ES6 规定暂时性死区和 let、const 语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为


### 资源管理
* 主要解决3个问题，资源加载，资源查找（使用），资源释放。
 * 资源释放
   在cocos2d-x中，我们使用引用计数器，在引用计数器为0的时候释放资源，维护好引用计数器即可。在cocos creater中，项目的资源统一由cc.loader管理。大量使用预制体，与各种资源复杂的引用关系增加了资源管理的难度。每一个加载的资源都会放到cc.loader的_cache中，但是cc.loader.release只是将传入的资源进行释放，而没有考虑资源依赖的情况。
释放依赖资源，可以用cc.loader.getDependsRecursively;递归获取指定资源依赖的所有资源，放入一个数组并返回，然后在cc.loader.release中传入该数组，cc.loader会遍历它们，将其逐个释放。这种情况下虽然可以释放资源，但却有可能释放了不应该释放的资源。在新版本的creater中，可以输入cc.loader._cache来查看所有的资源。如果资源太多，只关心数量，可以输入Object.keys(cc.loader._cache).length来查看资源总数。

### 网络
* 使用websocket,是一种基于tcp的全双工网络协议，可以让网页创建持久性的连接，进行双向通讯。在使用websocket时，第一步应该创建一个websocket对象，可以传入两个参数，第一个是url字符串，第二个是协议字符串或字符串数组，指定了可接受的自协议，服务端需要选择其中的一个返回，才会建立连接，但一般用不到。
url参数，主要分为4部分：协议://地址.端口/资源：ex：ws://echo.websocket.org
协议:必选项，默认是ws协议，如果需安全加密则使用wss。
地址:必选项，可以是ip或域名，当然建议使用域名。
端口:可选项，在不指定的情况下，ws的默认端口为80，wss的默认端口为443。
资源:可选性，一般是跟在域名后某资源路径，基本不需要它。
websocket的状态：
0：尚未建立连接
1：链接已建立，可以进行通信
2：正在进行关闭握手
3：已关闭
websocket提供了4个回调函数提供绑定：onopen,onmessage,onerror,onclose
（websocket的底层协议传输是如何实现的？与tcp、http的区别在哪里？基于websocket能否使用udp进行传输呢？使用websocket发送数据是否需要自己对数据流进行分包（websocket协议保证了包的完整）？数据的发送是否出现了发送缓存的堆积（查看bufferedAmount）？）
 
