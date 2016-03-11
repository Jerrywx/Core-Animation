# Core Animation
![Jerry header](https://avatars0.githubusercontent.com/u/12372823?v=3&s=40)
## Core Animation

## 目录
* [图层树](#图层树)
* [寄宿图](#storage)
* [图层几何学](#network)
* [视觉效果](#pictures)
* [变换](#interfaces)
* [专用图层](#framework)
* [隐士动画](#demo)
* [显示动画](#projects)
* [图层时间](#)
* [缓冲](#)
* [基于定时器的动画](#)
* [性能优化](#)
* [高绘图](#)
* [图像IO](#)
* [图层性能](#)


### <a id="图层树"></a>1. 图层树
* [图层与视图](#图层与视图)
* [图层的能力](#图层的能力)
* [使用图层](#使用图层)
* [总结](#1总结)

#### <a id="图层与视图"></a>1.1 图层与视图

##### UIView
    在iOS当中，所有的视图都从一个叫做UIVIew的基类派生而来，UIView可以处理触摸事件，可以支持基于Core Graphics绘图，可以做仿射变换（例如旋转或者缩放），或者简单的类似于滑动或者渐变的动画。

##### CALayer
    CALayer类在概念上和UIView类似，同样也是一些被层级关系树管理的矩形块，同样也可以包含一些内容（像图片，文本或者背景色），管理子图层的位置。它们有一些方法和属性用来做动画和变换。和UIView最大的不同是CALayer不处理用户的交互。

##### UIView & CALayer 区别
    每一个UIview都有一个CALayer实例的图层属性，也就是所谓的backing layer，视图的职责就是创建并管理这个图层，以确保当子视图在层级关系中添加或者被移除的时候，他们关联的图层也同样对应在层级关系树当中有相同的操作。

    实际上这些背后关联的图层才是真正用来在屏幕上显示和做动画，UIView仅仅是对它的一个封装，提供了一些iOS类似于处理触摸的具体功能，以及Core Animation底层方法的高级接口。 

##### 为什么要将 UIView & CALayer 分离
	原因在于要做职责分离，这样也能避免很多重复代码。在iOS和Mac OS两个平台上，事件和用户交互有很多地方的不同，基于多点触控的用户界面和基于鼠标键盘有着本质的区别，这就是为什么iOS有UIKit和UIView，但是Mac OS有AppKit和NSView的原因。他们功能上很相似，但是在实现上有着显著的区别。

#### <a id="图层的能力"></a>1.2 图层的能力
##### CALayer的功能
- 阴影，圆角，带颜色的边框
- 3D变换
- 非矩形范围
- 透明遮罩
- 多级非线性动画

#### <a id="使用图层"></a>1.3 使用图层


#### <a id="1总结"></a>1.4 总结
##### CALayer 的简单使用
- 创建一个工程
- 设置View背景色为灰色
- 创建一个 200x200 View 添加到View中央。设置为白色
- 创建一个 100x100 的蓝色CALayer 添加到白色View 的中央

```
@interface CoreAnimation1_3 ()
@property (nonatomic, strong) UIView	*layerView;
@property (nonatomic, strong) CALayer	*blueLayer;
@end

@implementation CoreAnimation1_3

- (void)viewDidLoad {
    [super viewDidLoad];

	[self setUpView];
}

- (void)setUpView {
	self.title = NSStringFromClass([self class]);
	self.view.backgroundColor = [UIColor grayColor];
	
	self.layerView = ({
		UIView *layerView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 200, 200)];
		layerView.backgroundColor = [UIColor whiteColor];
		[self.view addSubview:layerView];
		layerView;
	});
	
	self.blueLayer = ({
		CALayer *layer = [CALayer layer];
		layer.frame = CGRectMake(50, 50, 100, 100);
		layer.backgroundColor = [UIColor blueColor].CGColor;
		[self.layerView.layer addSublayer:layer];
		layer;
	});
	
}

- (void)viewWillLayoutSubviews {
	[super viewWillLayoutSubviews];
	self.layerView.center = CGPointMake(SCREEN_W * 0.5, SCREEN_H * 0.5);
}

@end
```

##### 何时使用VALayer
- 开发同时可以在Mac OS上运行的跨平台应用
- 使用多种CALayer的子类（见第六章，“特殊的图层“），并且不想创建额外的UIView去包封装它们所有
- 做一些对性能特别挑剔的工作，比如对UIView一些可忽略不计的操作都会引起显著的不同（尽管如此，你可能会直接想使用OpenGL绘图）
