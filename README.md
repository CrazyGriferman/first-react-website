# 项目总结

这个油管上的项目花了两天时间完成，期间遇到了一些问题在README里总结一下。

GitHub地址：https://github.com/briancodex/react-website-v1

油管地址：https://www.youtube.com/watch?v=I2UBjN5ER4s

# 遇到的问题

## 图标

如果你想使用Fontawsome的图标，要去他们的官网注册一个账户，然后那道那三行CDN，然后把那串代码放到你的index.html里。

## export default function

如果你不写这个东西，那么当你在App.js或者是其它地方使用<Function_name />的时候，会遇到如下问题：

> Error: Element type is invalid: expected a string (for built-in
components) or a class/function (for composite components) but
got: undefined. You likely forgot to export your component from 
the file it's defined in, or you might have mixed up default and 
named imports.

## 导入

如果你要从某个js文件里导入一个函数，一定要写
```
import { function } from '...';
```

一定要有花括号

## 文件的地址

这个东西别人在issue里提过，有时候你会发现images加载不出来（尤其是你在css文件里想要对img进行操作的时候）网上查了一下，可能是因为这个原因：

react项目在搭载的时候会以public为根目录，所以在用到的时候，你需要可以这么写：

> \<img src={window.location.origin + '/img/yourimage.png'} />

stackOverflow 上的高赞回答

> With create-react-app there is public folder (with index.html...). If you place your "myImage.png" there, say under img sub-folder, then you can access them through:


贴一个对这个问题的讨论地址:

https://stackoverflow.com/questions/37644265/correct-path-for-img-on-react-js


## 一些动画和图标的变化（使用useState）

你要用两个state，一个是click，一个是button（click来管理用户的点击状态，button用来表示按钮是否出现）

当你屏幕小于一定的大小的时候，使用showButton更新Button的状态。

```
const showButton = () => {
    if(window.innerWidth <= 960) {
        setButton(false);
    } else {
        setButton(true);
    }
};
```

然后使用eventLinstener来监听用户是否放大/缩小网页。

以上按钮的出现与否取决于网页的大小，而下面按钮的出现与否取决于是否点击了导航栏图标。


首先对于导航栏的图标，你可以设置一个if语句，有两个状态，一个是用户没点击的默认图标，一个是点了之后的图标。

onClick用于接收用户是否点击的信息，然后里面的的\<i> 会根据点击与否切换图标

```
<div className='menu-icon' onClick={handleClick}>
    {/* a toggle */}
    <i className={click ? 'fas fa-times' : 'fas fa-bars'} />
</div>
```



然后再设置一个函数用来控制click，那些按钮的出现与否取决于用户是否点击了图标:

```
const handleClick = () => setClick(!click);
```

在设计也导航栏 \<ul>的时候，也是用到切换className的方法。

> \<ul className={click ? 'nav-menu active' : 'nav-menu'}>

这个是导航栏里的每个链接，onClick={closeMobileMenu}是确保在点击之后，导航栏能够顺利关闭

```
<li className='nav-item'>
       <Link to='/services' className='nav-links' onClick={closeMobileMenu}>
              Services
       </Link>
</li>
```

closeMobileMenu的原型

> const closeMobileMenu = () => setClick(false);

### 对于button的处理


我们不可能重复地对每个button都像 `className={click ? 'fas fa-times' : 'fas fa-bars'}` 一样写一遍，所以我们得创建一个button函数，来统一对button的大小进行控制。

```
const STYLES = ['btn-primary', 'btn-outline'];

const SIZES = ['btn-medium', 'btn-large'];


export const Button = ({children, type, onClick, buttonStyle, buttonSize}) => {
    const checkButtonStyle = STYLES.includes(buttonStyle) ? buttonStyle : STYLES[0]

    const checkButtonSize = SIZES.includes(buttonSize) ? buttonSize : SIZES[0]

    return (
        <Link to='/sign-up' className='btn-mobile'>
            {/* 控制button的方法 搞一个default的，点了之后就变成另外一个 */}
            <button className={`btn ${checkButtonStyle} ${checkButtonSize}`} onClick={onClick} type={type}>
                {children}
            </button>
        </Link>
    )
};
```

此处使用了es6标准写法,目的就是为了传入变化后的buttonClass，以便在css里面对不同的class进行控制。

> className={\`btn \${checkButtonStyle} \${checkButtonSize}\`}


## 整个网页的框架

在react中，原来网页的分布是这样的：

最大的框架肯定是你的App.js，在这个项目中，导航栏和各个页面之间是分开的，页面的切换使用<Switch> (我还没学到这里),每个页面使用<Route>,这些网页都放在pages文件夹里。


```
function App() {
  return (
    <>
    <Router>
      <NavBar />
      <Switch>
        <Route path='/' exact component= {Home} />
        <Route path='/services' component={Services} />
        <Route path='/products' component={Products} />
        <Route path='/sign-up' component={SignUp} />
      </Switch>
    </Router>
    </>
  );
}
```

对于单个页面，同样也是类似的，比如说Home页面：

```
function Home () {
    return (
        <>
            <HeroSection />
            <Cards />
            <Footer />
        </>
    );
}
```

> <> </.> 表示fragments，在react中假如你要在单个网页上渲染多个组件，要用<Fragment></Fragment>，简写的话就是<></>

从这个函数中可以看出，Home由三个组件组成:
1.HeroSection
2.Cards
3.Footer

# 一些疑惑

做完这个项目，我有一些疑惑分别是以下几点：

## Cards和CardItem之间的关系

目前我觉得CardItem是卡片里的东西，而Cards就是整个卡片的外部，在CardItem里面需要传入一些参数，好像就是在Cards里面写到的

```
eg:
<CardItem
    src='images/img-2.jpg'
    text='Travel through the Islands of Bali in a Private Cruise'
    label='Luxury'
    path='/services'
/>

```

## useEffect的使用

这个不是很懂，之后再看吧。

## 一个小问题

在搞卡片的时候，尽管我的代码和作者写的一样，但是展示的却不一样，后来我把我的代码全部换上他的，也不行。（已经刷新了好几次网页了），然后不知道为啥我又刷新了几次网页，突然就好了，一开始我以为是 cards-item-pic-wrap这个东东出问题了（于是我修改了css里面它的width，但是还是有问题），这个就挺奇怪的。


# 总结

看《learning React》看到一半，突然看不下去了，于是就像在油管上跟着做个小项目。

说实话，即使你跟着作者一行一行敲，但是还是有很多问题发生，总结思考这些问题就是学习的过程。单看书真的太枯燥了，更别说还是全洋文的，之前看《dom scripting》就花了好久，果然还是在学校里有学习的劲头。

希望之后能够学习数据库怎么和react网页交互，以及如何把网页搞到服务器上弄子域名访问。

# 网页展示

![](https://blog-1300038350.cos.ap-shanghai.myqcloud.com/blogImage/Jietu20210219-170025.jpg)

![](https://blog-1300038350.cos.ap-shanghai.myqcloud.com/blogImage/Jietu20210219-170005.jpg)

![](https://blog-1300038350.cos.ap-shanghai.myqcloud.com/blogImage/Jietu20210219-165942.jpg)

![](https://blog-1300038350.cos.ap-shanghai.myqcloud.com/blogImage/Jietu20210219-165923.jpg)

![](https://blog-1300038350.cos.ap-shanghai.myqcloud.com/blogImage/Jietu20210219-165854.jpg)