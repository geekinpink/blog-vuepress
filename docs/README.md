### 面试题准备

#### 算法
1. DFS，BFS
   1. 二叉树层序遍历
   2. 前中后遍历（迭代/递归）
2. 快排
3. 快慢指针
   1. 快乐数
4. 双指针
   1. 三数字之和
5. hash 表
   1. 最长连续上升子序列
6. 异或
   1. 不借助变量交换两数
7. 回溯
   1. 括号生成
   2. 全排列
   3. 数组子集
8. 动态规划
   1. 最长上升序列长度
   2. 矩阵最短路径
9. 分治
10. 贪心
    1. 股票买卖
11. 链表
    1. 反转链表

### 知识体系

#### 样式相关

1. 基础：盒子模型，浮动，BFC，flex：
   1. BFC：块级格式化上下文。应用：清除浮动，margin 重合等。
   2. 垂直居中：使用 flex。
   3. 画形状：扇形，三角形，椭圆
   4. flex：主轴，交叉轴，对齐，换行，放大缩小比例。
2. 动画相关：frame 动画，css 动画，animation。

#### TS 基础

1. unknown 和 any：推荐使用 unknow，它提供了安全的类型，有更好的类型收窄功能。

2. TS 中的 OOP

   1. OOP 三大特点：封装，继承，多态
   2. interface 可以用于 implements
   3. 

3. Decorator

   1. 实验性特性，es 也是提案中。

4. 泛型的理解

   1. 更加灵活的定义，和重用性，不必指定特定类型。

   2. 可以做到有意义的收束。

      1. extends，keyof 等

      2. 提供默认类型：

         ```typescript
         interface A<T = string> { a: T }
         const a: A = { a: 'string' }
         const a1: A<number> = { a: 1 }
         ```

      3. 泛型条件类型：

         ```typescript
         async function stringPromise() {
           return "Hello, Semlinker!";
         }
         
         interface Person {
           name: string;
           age: number;
         }
         
         async function personPromise() {
           return { name: "Semlinker", age: 30 } as Person;
         }
         
         type PromiseType<T> = (args: any[]) => Promise<T>;
         type UnPromisify<T> = T extends PromiseType<infer U> ? U : never;
         
         type extractStringPromise = UnPromisify<typeof stringPromise>; // string
         type extractPersonPromise = UnPromisify<typeof personPromise>; // Person
         ```

      4. 泛型工具类型

         ```typescript
         type Partial<T> = {
           [K in keyof T]?: T[K]
         }
         
         type Record<K extends keyof any, T>  {
         	[P in K]: T
         }
         ```

#### JS基础

1. EventLoop：微任务，宏任务
   1. 描述：js 运行时环境包括了堆，栈，队列。
   2. JS 是单线程，如果所有任务都排队执行则阻塞。所以有同步任务和异步任务，EventLoop 处理异步任务的调度机制。在执行完同步任务，则会从任务队列中出队执行。
   3. microtask 和 macrotask 区别。
2. 数据类型：基础，引用，垃圾回收，内存泄漏
   1. 类型检测方式：Object.prototype.toString.call
   2. constructor 判断
3. 原型链，继承，作用域
   1. new，instanceof 的实现
      - new：创建对象；对象原型指向构造器的原型；调用构造器，this 绑定刚创建的对象；如果构造器返回对象，则返回它，否则返回一开始创建的对象。
      - instanceof：沿着\__proto__链找，要么最后为 null，要么等于目标 prototype。
   2. this 指向问题，call，apply 等的实现
   3. 深克隆（特殊类型（Date，RegExp）考虑，循环应用问题）
4. 异步编程：Promise，async / await
   1. 并发控制（利用 race 和 all）
5. 函数式编程：1. 柯里化 2.高阶函数 3. 闭包
   1. 应用：节流、防抖 （立即执行版本）
   2. currying 实现
   3. 闭包是和其周围的环境引用绑定的函数 它能访问其外层的作用域
6. 常用的 ES6+：let const 模板字符串 箭头函数 数组/对象解构 optional chaining 空对象合并符 Promise async await Set Map 等

#### 前端模块化

1. 解决了作用域问题，通过文件系统为代码做合理分隔，并且不会污染全局环境
2. ESM，CommonJS，AMD（require.js），CMD（sea.js），UMD，IIFE
   1. ESM 静态依赖分析
   2. CommonJS 运行时依赖确定

#### 浏览器相关

1. perfermance
2. 缓存机制
   1. 强缓存：cache-control / expires，是否过期判断，如果没有则返回200，从浏览器的 memory 或者 disk 中读取。
   2. 协商缓存：浏览器发送 e-tag / last-modified 字段给浏览器，浏览器第二次请求带上 if-none-match / if-modified-since，校验新鲜度，需要缓存则返回304 读取缓存。

#### 框架

1. Vue 的响应式原理？
   1. 通过 Object.defineProperty 去拦截对象的 getter / setter，当 render 函数中访问了对应的 data，data 上的每个属性都会递归收集依赖，通过 dep 对象的 depend 方法，订阅了当前的 watcher ，当修改 data 时，触发 setter，调用了 dep 对象的 notify 方法，则触发了订阅 watcher 的重新计算。
2. Vue 3有些什么新特性？
   1. 整体架构重新设计，各模块采用 monorepo 管理，ts 重写，支持  tree shaking
   2. VCA 的支持，新增了 teleport，fragment 等新组件
   3. 响应式系统重写，Proxy 代替了原来。
   4. Diff 优化：编译静态提升，事件缓存。
3. keep-alive 原理
4. Vue 的编译原理？
   1. parser  -> optimizer -> codegen：首先对模板进行解析，生成 AST；AST 优化，标记静态节点，以优化 diff；最后生成 render 函数。
5. Vue Computed 原理？
   1. Vue 初始化时，会为每个 computed 值创建一个渲染 watcher，并且将 computed 值代理到 vm 上，并且 getter 是对当前渲染 watcher 的计算。
   2. 当渲染 watcher 计算时，触发了对应的 data 的getter，再次收集依赖，depend 到该 watcher。这样 computed watcher 就订阅了依赖的 data 的变化了。
6. diff 算法的描述？时间复杂度？
   1. 同层比较，四个指针移动比较，

#### 网络相关

##### 登录原理

1. cookie + sessionId：服务器验证成功后，通过 set-cookie 将 sessionId 种入 cookie，再次登录的时候带上 cookie 验证。问题：不能防止 CSRF 攻击，性能差（服务器需要存储）。
2. Token：登录成功产生 token，token 由**客户端存储**，再次登录的时候带上 token，服务器进行验证。JWT 签名算法：
   1. token 包括了 header（指定加密算法）， payload（指定 token 生成时间），signature (通过 header 和 payload 配合服务器的私钥生成)。
   2. 生成的 token 通过同样的算法，可以生成相同的签名，如果签名一致，并且没有过期，则通过鉴权。
3. 单点登录：a.com 未鉴权时候，302重定向到 sso 登录页，登录成功后带上 ticket 跳转至 a.com 的验证接口，并且 sso 种上了 cookie，然后 a.com 创建会话，种上 sessionId，这样就登录完成。

##### 短链原理

1. 随机生成 对应一个 id

#### http 相关

##### 基础知识

[http]: https://mp.weixin.qq.com/s/q6U82xcYUUqgrbDAIvNIEw	"http"

1. 报文组成：请求/响应行 + 请求/响应头 + 空行 + 请求体/响应体
2. POST 和 GET 的区别：缓存 编码 参数 幂等
3. 常用状态码：200，301（永久重定向），302（暂时重定向），304（命中协商缓存），401，404，500。
4. cookie：secure（只能 https 传输），SameSite（防范 CSRF），HttpOnly（防范 XSS）。
5. CORS：简单请求、非简单请求（预检请求）。

##### http2.0/https

1. http2.0 的升级：二进制传输，多路复用，头部压缩，服务器推送。
   - 头部压缩：两端建立哈希表，只需要传输索引就能知道传输字段。
   - 多路复用：http1.x ，在 tcp 的长链接中，会队头阻塞。http2 使用了二进制帧来进行传输，并且不像明文存在顺序问题，所以无需排队等待。
   - 服务器推送：可以主动推送内容，如请求 html，主动推送相关资源，客户端减少等待。
2. https：建立在 SSL/TLS 协议上的 http 协议，证书颁发，非对称加密，报文进行的是对称加密。
   1. 加密过程（对称加密和非对称加密）：server 持有私钥A和公钥A，公钥明文传送给浏览器，浏览器随机生成对称加密秘钥 X，用公钥A 加密 X 传送给 server，这样两端都有秘钥 X，传输过程中就能加密，并且秘钥 X 经过了非对称加密，能保证安全。
   2. 中间人攻击：利用浏览器无法知道公钥是网站的公钥，中间人拦截 server 的公钥 A，中间人用公钥 B 替换，得到 B 加密后的秘钥 X，然后解密，再用 A 加密传回 server 端，中间人得到了明文加密秘钥 X，并且两端没有察觉。
   3. CA 证书：证书由信任机构颁发，避免上述中间人拦截问题，证书中包含公钥和私钥，对明文 T 进行 hash 然后私钥加密得到签名 S。浏览器得到证书对 T 进行证书指定的 hash，然后签名 S 用公钥解密，两者值相等说明证书没有被篡改。（hash 作用是性能的优化）

##### 安全相关 

1. CSRF：跨站请求伪装

   1. 原理：攻击者伪装用户对网站进行恶意操作。
   2. 防范：SameSite 设置；登录时 token + cookie 双重认证，同源检测。

2. XSS：跨站脚本攻击 

   [XSS]: https://juejin.cn/post/6844903685122703367

   1. 原理：恶意代码注入
   2. 类型：反射型，注入型，Dom 型。
   3. 防范：前后端对用户输入转译，CSP规则开启，HttpOnly 开启。

##### 其他

1. tcp 和 udp 的区别：todo
2. WebSocket 应用：扫码上传图片？
3. 跨域的方式：
   1. CORS：简单请求，非简单请求（预检）。
4. 分片上传？
5. DNS：
   1. 概念：根域名，顶级域名，次级域名
   2. 查找过程：分级查找，
6. CDN（内容分发网络）： 
   1. 目的：加速请求，保证服务的可用性，降低了源站的压力。
   2. 过程：1. 应用了 CDN 之后，请求网站后， DNS 解析到 CNAME （CDN 专用 DNS），再指向了 CDN 的负载均衡系统，根据用户的 IP ，运营商和节点负载状况，返回最合适的节点给用户访问。
   3. 指标：命中率：命中缓存；回源率：访问源站；

#### 工程化

##### monorepo

1. yarn workspace：处理依赖问题。
   1. 根目录初始化：指定 private: true，workspaces 的路径。
   2. yarn install：会将指定的 workspace 都放到顶层 node_modules。
   3. 依赖提升，公用的依赖会放到顶层的 node_modules。
   4. 安装依赖：根目录；所有子包安装；特定子包安装。
2. lerna：和 yarn workspace 很多能力重合，最佳实践应该是处理发布问题。
3. 在业务中的实践：
    	1. 发布时包的裁剪，降低 npm 包安装时间。
    	2. shared 机制。
    	3. branchformat。

##### webpack

1. 流程概述：
   1. 将命令行参数，配置参数等合并校验，然后开始准备 Compiler 对象，初始化编译环境
   2. 开始从 Entry 开始，递归寻找对应的依赖模块， 使用对应的 Loader 进行编译。
   3. 完成了编译之后，将编译完成的 chunks 输出到文件系统。
2. tapable 简介：订阅发布系统的实现，以 hook 呈现，包括了同步，异步钩子，并且有Bail，WaterFall，Loop，Parallel，Series 等针对回调返回值的调度。
3. loader 和 plugin：loader 是在编译的过程中起到转译的作用。plugin 在编译的过程中，可以在各个阶段做不同事项的处理，是一个广播和订阅的过程。
4. 构建速度优化：
   1. resolve 优化：缩小查找范围，resolve.modules 指定依赖路径，减少层级搜索。
   2. noParse：指定无需处理的模块。
   3. 指定extensions：减少文件查找。
   4. DLLPlugin，DLLReferencePlugin：将第三方依赖打入 dll 文件中，省去多次打包。
   5. HappyPack：多进程 Loader 转换。

##### vite

1. bundleless：
2. 大致原理：

##### rollup

1. 介绍一下：

##### babel

1. AST：抽象语法树，源码语法结构的表示。
   1. 流程：1. 词法分析 scanner，生成 tokens，扁平的语法片段数组；2.语法分析，使用 parser 生成 ast；3. 对 ast 进行深度优先遍历，生成新的 ast；4. generator 将新 ast 转换成代码。

##### typescript

1. ts-loader：ts 文件处理，调用 tsc，transpileOnly 默认 false，改为 true 不做类型检查，加快编译速度。
2. 类型检测：fork-ts-checker-webpack-plugin，单启进程进行 ts 类型检测，上述方案的补充。
3. ts-loader 和 babel 的关系：babel7 处理 ts 只做编译，抛弃类型。需要 ts-loader 和 babel 二选一，最佳实践：ts-loader 编译 ts 为 js，babel 再接手，配合上述2。

##### 工作流规范

1. git 相关：
   1. commit 信息校验：@commitlint/cli，@commitlint/config-conventional（lint 的规则）。
   2. 提交规范 commit 信息 cli：commitizen，git cz 或者 package.json 中配置 commit script。cz-conventional-changelog，conventional-changelog-cl。
   3. git hooks：husky 配置 commit-msg 钩子做 commit 信息的 lint，可以配置 pre-commit 做 lint-staged。
2. eslint / prettier 相关：
   1. eslint 做代码 lint 检测，prettier 做代码格式化。冲突解决：eslint-config-prettier，eslint-plugin-prettier。
   2. typescript 的 lint 配置相关，plugin 和 parser。
   3. vue 文件的 lint 支持：vue-eslint-parser。
3. npm semver （语义化版本更新）：breaking change 更新 major，feature 更新 minor，bugfix 更新 patch，预发布更新 prerelease。

#### 前端领域方案

##### 错误监控

1. 错误类型：JS 错误，Ajax 错误，资源加载错误，Promise 抛出错误。
2. 怎么去捕获：onerror，error 事件，拦截xhr，fetch，unhandledrejection 事件，vue 的 errorHandler。

##### 性能监控

1. 性能指标，如何获取：
   1. 
2. Web 应用性能优化手段

##### 前端埋点

##### UI自动化测试

1. 利用 puppeteer (headless浏览器) 来模拟人工操作，然后断言结果。
1. 用户操作路径录制如何做，chrome 已经有支持。

##### 微前端

1. 背景：多团队参与，跨技术栈整合，巨石应用的分解。解决的是可控体系下前端协同开发的问题。
1. 常见方案：single-spa 类型，web-components，module federation 等。
1. 针对业务的整体解决方案：
   1. 主子应用运营管理：建设平台，管理
   1. DevOps 建设
   1. 子应用性能/安全监控
   1. 容灾机制
      1. 子应用入口加载重试
      1. CDN 容灾
1. 运行时组合需要解决的问题：
   1. 子应用加载、切换
   1. 隔离和通信
1. 常见问题：

1. 样式隔离 ：
   1. qiankun 使用了 html entry，会将子应用整个 dom 树都挂载在指定节点，所以也包含样式表。在子应用加载和卸载时，样式也会一起，天然形成了隔离。
   2. qiankun 内部提供了两种 css 沙箱：1. 利用 shadom dom；2. 类似 vue 的 scoped style，给全局样式加上 data 属性。
2.  Js 沙箱：
   1. SnapshotSandbox：子应用加载启动之后，记录全局变量状态，卸载的时候做 diff，恢复快照。进去都遍历一次 window，性能差。
   2. Proxy 沙箱：
      1. LegacySandbox：单例沙箱，利用 Proxy 记录每次 window 上变更和新增的值，卸载时候恢复原状，还是会污染 window。
      2. ProxySandbox：多实例沙箱，利用 Proxy 来代理全局对象，不会污染 window。

##### 持续集成/交付

##### 低代码





