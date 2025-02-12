## nextjs 开发指南

参考 https://github.com/moon-in-river/NuggetsBooklet/tree/master/Next.js%20%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97

时间短，学习的量大。及时反馈、及时满足的方式比延迟满足好的多。

##### 怎么学这个小册

- 基础篇前 20 篇，了解如何使用脚手架、如何定义路由、如何获取数据、如何定义样式等写 Next.js 项目最基本的知识
- 基础篇后 16 篇作为开发手册使用，可以 10 分钟看完一篇
- 实战篇，至少写出第一个实战项目 React Notes
- 源码篇，了解实现原理

# 路由篇

## App 路由

同一文件夹下如果有 layout.js 和 page.js，page 会作为 children 参数传入 layout。换句话说，layout 会包裹同层级的 page。

- 如果你要更改这些标签，_不推荐直接修改_，参考《Metadata 篇》。
- 可以使用*路由组*创建多个根布局。

### 模板

用法跟布局一模一样。它们最大的区别就是状态的保持。layout 会包裹 template，template 又会包裹 page。适用场景：

- 依赖于 useEffect 和 useState 的功能，比如记录页面访问数（维持状态就不会在路由切换时记录访问数了）、用户反馈表单（每次重新填写）等
- 更改框架的默认行为，举个例子，布局内的 Suspense 只会在布局加载的时候展示一次 fallback UI，当切换页面的时候不会展示。但是使用模板，fallback 会在每次路由切换的时候展示https://juejin.cn/post/7343569488744300553

### 定义加载页面

使用 Suspense 组件或者使用 use api 都可以实现页面的加载效果

### 定义错误页面

error.js 展示错误发生时的展示页面，使用 React 的 Error Boundary 组件捕获错误，并展示错误页面。

- 有时错误是暂时的，只需要重试就可以解决问题，在 error.js 导出的组件中，传入 reset 函数，帮助尝试从错误中恢复。该函数会触发重新渲染错误边界里的内容。如果成功，会替换展示重新渲染的内容。
- 错误边界不能捕获同级的 layout.js 或者 template.js 中的错误。如果你想捕获特定布局或者模板中的错误，那就需要在父级的 error.js 里进行捕获。如果已经到了顶层，就比如根布局中的错误如何捕获呢？为了解决这个问题，Next.js 提供了 global-error.js 文件，使用它时，需要将其放在 app 目录下。

### 定义未找到页面

使用 not-found.js 文件，展示未找到页面。

只能由两种情况触发：

- 当组件抛出了 notFound 函数的时候
- 当路由地址不匹配的时候

## 链接与导航

在 Next.js 中，有 4 种方式可以实现路由导航：

- 使用 <Link> 组件
- 使用 useRouter Hook（客户端组件）
- 使用 redirect 函数（服务端组件）
- 使用浏览器原生 History API。usePathname 和 usestate 有什么区别？如何实时更新

useRouter() hook。注意使用该 hook 需要在客户端组件中。  
客户端组件使用 useRouter hook，服务端组件则可以直接使用 redirect 函数。  
也可以使用浏览器原生的 window.history.pushState 和 window.history.replaceState 方法更新浏览器的历史记录堆栈。通常与 usePathname（获取路径名的 hook） 和 useSearchParams（获取页面参数的 hook） 一起使用。

## 动态路由

- [folderName]  
  将文件夹的名字用方括号括住。这个路由的名字会作为 params prop 传给布局、 页面、 路由处理程序 以及 generateMetadata 函数。
- [...folderName]  
  在方括号内添加省略号，表示捕获所有后面所有的路由片段。
- [[...folderName]]  
  在双方括号内添加省略号，表示可选的捕获所有后面所有的路由片段。与上一种的区别就在于，不带参数的路由也会被匹配（就比如 /shop）

## 路由组

在 app 目录的文件夹会被映射到 URL 中，可以将文件夹标记为路由组，阻止文件夹名称被映射到 URL 中。  
使用路由组，你可以将路由和项目文件按照逻辑进行分组，但不会影响 URL 路径结构。  
把文件夹用括号括住就可以标记为路由组，就比如 (dashboard)。最终的 URL 中省略了带括号的文件夹。  
路由组的命名除了用于组织之外并无特殊意义。它们不会影响 URL 路径。

- 借助路由组，即便在同一层级，也可以创建不同的布局
- 创建多个根布局。需要删除顶层的 app/layout.js 文件，访问 /会报错，所以 app/page.js 需要定义在其中一个路由组中。

## 平行路由

在同一个布局中同时或者有条件的渲染一个或者多个页面。将文件夹以 @作为开头进行命名平行路由。  
插槽会作为 props 传给共享的父布局，props.slot1、props.slot2。  
平行路由可以让你为每个路由定义独立的错误处理和加载界面。  
平行路由内可以添加子页面，并且跟路由组一样，不会影响 URL。

硬导航（Hard Navigation，比如浏览器刷新页面），因为 Next.js 无法确定与当前 URL 不匹配的插槽的状态，所以会渲染 404 错误。

访问 /visitors 本身就会造成插槽内容与当前 URL 不匹配。

当读取路由时会去根路由、平行路由下读取，尽管这些路由可能没有目标 url，nextjs 没法确定哪个路由返回的结果是正确的。

为了解决这个问题，Next.js 提供了 default.js。当发生硬导航的时候，Next.js 会为不匹配的插槽呈现 default.js 中定义的内容，如果 default.js 没有定义，再渲染 404 错误。

## 拦截路由

拦截路由允许你在当前路由拦截其他路由地址并在当前路由中展示内容。

在命名文件夹的时候以 (..) 开头，其中：

- (.) 表示匹配同一层级
- (..) 表示匹配上一层级
- (..)(..) 表示匹配上上层级。
- (...) 表示匹配根目录

> 但是要注意的是，这个匹配的是路由的层级而不是文件夹路径的层级，就比如路由组、平行路由这些不会影响 URL 的文件夹就不会被计算层级。

## 路由处理程序

使用 Web Request 和 Response API 对于给定的路由自定义处理逻辑。  
对于前后端分离架构，客户端与服务端之间通过 API 接口来交互。这个“API 接口”在 Next.js 中有个更为正式的称呼，就是路由处理程序。nextjs 也能提供接口，以往都是后端服务提供。

### 定义路由处理程序

定义一个名为 route.js 的特殊文件。

该文件必须在 app 目录下，可以在 app 嵌套的文件夹下，

> 但是要注意 page.js 和 route.js 不能在同一层级同时存在。因为一个路由不能是页面，还是 api 接口。

使用 next/server 的 NextResponse 对象用于设置响应内容，但这里不一定非要用 NextResponse，直接使用 Response 也是可以的

> 但在实际开发中，推荐使用 NextResponse，因为它是 Next.js 基于 Response 的封装，对 TypeScript 更加友好，同时提供了更为方便的用法，比如获取 Cookie 等。

### 不支持的方法会返回 405

支持 GET、POST、PUT、PATCH、DELETE、HEAD 和 OPTIONS 这些 HTTP 请求方法。

### 入参

请求方法会被传入 2 个参数，request、context。

### 缓存行为

默认情况下，使用 Response 对象（NextResponse 也是一样的）的 GET 请求会被缓存。

以下行为会退出缓存：

- GET 请求钟使用 Request 对象
- 除了 GET 方法，还添加其他 HTTP 方法，比如 POST。  
  因为 POST 请求往往用于改变数据，表示数据会发生变化，此时不适合缓存。
- 使用像 cookies、headers 这样的动态函数。  
  因为 cookies、headers 这些数据只有当请求的时候才知道具体的值。
- 路由段配置项手动声明为动态模式
  ```ts
  export const dynamic = "force-dynamic";
  ```

触发更新的 2 种方式。

- 使用路由段配置项。超过 revalidate 设置时间的首次访问会触发缓存更新，如果更新成功，后续的返回就都是新的内容，直到下一次触发缓存更新。
  ```ts
  export const revalidate = 10;
  ```
- 使用 fetch 请求的 next.revalidate 选项。效果和上一项一致。

### 写接口常见问题

#### 获取网址参数

```ts
// app/api/search/route.js
// 访问 /api/search?query=hello
export function GET(request) {
  const searchParams = request.nextUrl.searchParams;
  const query = searchParams.get("query"); // query
}
```

#### 处理 Cookie

读取 Cookie

```ts
request.cookies.get("token");
```

设置 Cookie

```ts
import { cookies } from "next/headers";
... 其他代码
const cookieStore = cookies();
return new Response("Hello, Next.js!", {
  status: 200,
  headers: { "Set-Cookie": `token=${token}` },
});
... 其他代码
```

#### 重定向

重定向使用 next/navigation 提供的 redirect 方法

#### 获取请求体内容

#### 设置 CORS

#### Streaming

```ts
import { OpenAIStream, StreamingTextResponse } from "ai";
```

也可以直接使用底层的 Web API ReadableStream 实现 Streaming。

## 中间件

使用中间件，你可以拦截并控制应用里的所有请求和响应。

在项目的根目录定义一个名为 middleware.js 的文件。需要具名导出一个 middleware 函数，一个 config 对象。

> 注意：这里说的项目根目录指的是和 pages 或 app 同级。但如果项目用了 src 目录，则放在 src 下。

在 middleware 函数中设置中间件的逻辑，在 config 对象中设置中间件的配置。

### 设置匹配路径

有两种方式可以指定中间件匹配的路径。

- config.matcher。支持字符串形式，也支持数组形式。
- 使用条件语句，if 判断 url。

**:path\*** 用法来自于 path-to-regexp ,作用就是将 /user/:name 这样的路径字符串转换为正则表达式。

通过在参数名前加一个冒号来定义命名参数（Named Parameters）。比如 /about/:path 匹配 /about/a 和 /about/b，但是不匹配 /about/a/c。

> 实际测试的时候，/about/:path 并不能匹配 /about/xxx，只能匹配 /about，如果要匹配 /about/xxx，需要写成 /about/:path/
>
> 注意，路径必须以 /开头。matcher 的值必须是常量，这样可以在构建的时候被静态分析。使用变量之类的动态值会被忽略。

config.matcher 还可以判断查询参数、cookies、headers。

### 中间件逻辑

如何读取和设置 cookies ？
如何读取 headers ？
如何直接响应?

### 执行顺序

在 Next.js 中，有很多地方都可以设置路由的响应，比如 next.config.js 中可以设置，中间件中可以设置，具体的路由中可以设置，所以要注意它们的执行顺序：

1. headers（next.config.js）
2. redirects（next.config.js）
3. 中间件 (rewrites, redirects 等)
4. beforeFiles (next.config.js 中的 rewrites)
5. 基于文件系统的路由 (public/, \_next/static/, pages/, app/ 等)
6. afterFiles (next.config.js 中的 rewrites)
7. 动态路由 (/blog/[slug])
8. fallback 中的 (next.config.js 中的 rewrites)

注： beforeFiles 顾名思义，在基于文件系统的路由之前，afterFiles 顾名思义，在基于文件系统的路由之后，fallback 顾名思义，垫底执行。

### 运行时

写 Middleware 的时候，尽可能使用 Web API，避免使用 Node.js API。

### 中间件的代码维护

借助高阶函数。

# 渲染篇

## 基本概念

- 客户端渲染。浏览器会先下载一个非常小的 HTML 文件和所需的 JavaScript 文件。在 JavaScript 中执行发送请求、获取数据、更新 DOM 和渲染页面等操作。

最大的问题就是不够快。（SEO 问题是其次，现在的爬虫已经普遍能够支持 CSR 渲染的页面）

在页面中使用 React useEffect hook。或者在客户端使用数据获取的库比如 SWR（也是 Next.js 团队开发的）或 TanStack Query。

- 服务端渲染。由服务端直接请求接口、获取数据，然后渲染成静态的 HTML 文件返回给用户。总的速度比较快，也就是首屏渲染事件。但是 TTFB（Time to First Byte）相对较长。

使用 SSR，你需要导出一个名为 getServerSideProps 的 async 函数。这个函数会在每次请求的时候被调用。返回的数据会通过组件的 props 属性传递给组件。

- 静态站点生成。在获取数据之前，提前编译出 html，用户访问的时候直接返回。速度更快。  
  当不获取数据时，默认使用的就是 SSG。如果需要获取数据，使用 getStaticProps，getStaticProps 会在构建的时候被调用，并将数据通过 props 属性传递给页面。第二种方式是使用 getStaticPaths 定义了哪些路径被预渲染，搭配动态路由和 getStaticProps，获取路径参数，请求数据传给页面。

- 增量静态再生。相较于 SSG，页面内部分数据需要更新，比如博客点赞数。Next.js 提出了 ISR。当用户访问了这个页面，第一次依然是老的 HTML 内容，但是 Next.js 同时静态编译成新的 HTML 文件，当你第二次访问或者其他用户访问的时候，就会变成新的 HTML 内容了。  
  在 getStaticProps 中添加一个 revalidate 即可。

> 一个 nextjs 应用的不同页面支持各个模式的混合使用。

## React Server Component

SSR 的缺点在于：

- SSR 的数据获取必须在组件渲染之前
- 组件的 JavaScript 必须先加载到客户端，才能开始水合
- 所有组件必须先水合，然后才能跟其中任意一个组件交互

加载整个页面的数据，加载整个页面的 JavaScript，水合整个页面，还必须按此顺序串行执行。如果有某些部分慢了，都会导致整体效率降低。

RSC 的优点在于：

- RSC 提供了更细粒度的组件渲染方式，可以在组件中直接获取数据，而非像 Next.js v12 中的 SSR 顶层获取数据。
- RSC 在服务端进行渲染，组件依赖的代码不会打包到 bundle 中，而 SSR 需要将组件的所有依赖都打包到 bundle 中。

当然两者最大的区别是：

SSR 是在服务端将组件渲染成 HTML 发送给客户端，而 RSC 是将组件渲染成一种特殊的格式，我们称之为 RSC Payload。这个 RSC Payload 的渲染是在服务端，但不会一开始就返回给客户端，而是在客户端请求相关组件的时候才返回给客户端，RSC Payload 会包含组件渲染后的数据和样式，客户端收到 RSC Payload 后会重建 React 树，修改页面 DOM。  
每次 SSR 都是一个新的 HTML 页面，所以状态不会保持。但是 RSC 不同，可以多次重新获取，然后客户端根据这个特殊格式更新 UI，而不会丢失客户端状态。

# 工程

## 构建

> 必须拷贝静态资源，因为 Next.js 的静态资源是放在 .next/static 目录下的，而 standalone 模式下，静态资源是放在 .next/standalone/.next/static 目录下的，所以需要拷贝过去。

```bash
cp -r .next/static .next/standalone/.next/static && cp -r public .next/standalone/public
```
