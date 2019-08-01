---
layout:     post
title:      "Flutter for iOS 设计语言：Cupertino 中文翻译"
subtitle:   "用前人的尸骨，抹平道路的崎岖"
date:       2019-07-31
author:     "Abel"
header-img: "img/header/2019-07-31-flutter-zh-cn-h.jpg"
catalog: true
tags:
  - Flutter
  - 翻译
  - iOS
  - Cupertino
---

# 为什么要翻译

闲下来就要找事做，忙起来了就有意思了。

**Cupertino 是 iOS 的官方设计语言。Flutter 对其进行了高保真的还原，以谋求与原生控件一致的效果。**

本文对 [Cupertino (iOS-style) widgets](https://flutter.dev/docs/development/ui/widgets/cupertino) 下的内容进行非逐字对应的简要翻译，意图为清晰而准确的描述每个 widget 的用法。所有示例均带 demo code，效果与截图一致。欢迎交流和指正。

所有 demo 使用的 Flutter 库版本为 ``1.7.8+hotfix.3``。

> 本文授权并同步首发于欢聚时代旗下技术公众号：**YYGeeker**。更多干货，欢迎订阅。
>
> **未经许可，严禁转载。**
> 
> 本文地址 [https://abeljoo.com/2019/07/31/flutter-zh-cn/](https://abeljoo.com/2019/07/31/flutter-zh-cn/)

# CupertinoActionSheet & CupertinoActionSheetAction

``CupertinoActionSheet`` 仿生了 iOS 原生系统中的 ``Action Sheets`` 组件，该组件用于创建浮层 List 选择框。如下图所示。

> 前往 [Action Sheets](https://developer.apple.com/design/human-interface-guidelines/ios/views/action-sheets/) 了解更多关于 iOS 原生 ``Action Sheets`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-30 at 11.56.48.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## CupertinoActionSheet

``CupertinoActionSheet`` 构造函数的实现如下。

```
const CupertinoActionSheet({
Key key,
Widget title,
Widget message,
List<Widget> actions,
ScrollController messageScrollController,
ScrollController actionScrollController,
Widget cancelButton
})
```

- ``title`` 为顶部标题。此 Demo 中，``title`` 为 *Favorite Dessert*。

- ``message`` 为标题下的信息。此 Demo 中，``message`` 为 *Please select the best dessert from the options below*。

- ``actions`` 是一个 Widget List。该值承载列表中的选项按钮。此 List 的值通常使用 ``CupertinoActionSheetAction`` 为值。详见下方解释。

- 当顶部标题或列表选项过多时，``messageScrollController`` 及 ``actionScrollController`` 用于滚动控制。这两个值通常不需要设置。

- ``cancelButton`` 是一个 Widget，用于 cancel 事件按钮。详见 ``CupertinoActionSheetAction``。

## CupertinoActionSheetAction

``CupertinoActionSheetAction`` 构造函数的实现如下。

```
const CupertinoActionSheetAction({
@required VoidCallback onPressed,
bool isDefaultAction: false,
bool isDestructiveAction: false,
@required Widget child
})
```

在此 demo 中，四个按钮的实体都是通过 ``CupertinoActionSheetAction`` 实现的。

- ``onPressed`` 用于监听点击事件。

- ``isDefaultAction`` 标记是否使用粗体，本 demo 的 *Cannolis* 按钮体现了该特性。

- ``isDestructiveAction`` 标记是否显示警告样式，即红色字体。本 demo 的 *Trifle* 按钮体现了该特性。

## 示例代码

此 demo 完整实例如下。

```
void _onButtonClick() {
  setState(() {
    showDemoActionSheet(
      context: context,
      child: CupertinoActionSheet(
        title: const Text('Favorite Dessert'),
        message: const Text(
            'Please select the best dessert from the options below.'),
        actions: <Widget>[
          CupertinoActionSheetAction(
            child: const Text('Profiteroles'),
            onPressed: () {
              Navigator.pop(context, 'Profiteroles');
            },
          ),
          CupertinoActionSheetAction(
            isDefaultAction: true,
            child: const Text('Cannolis'),
            onPressed: () {
              Navigator.pop(context, 'Cannolis');
            },
          ),
          CupertinoActionSheetAction(
            isDestructiveAction: true,
            child: const Text('Trifle'),
            onPressed: () {
              Navigator.pop(context, 'Trifle');
            },
          ),
        ],
        cancelButton: CupertinoActionSheetAction(
          child: const Text('Cancel'),
          isDefaultAction: true,
          onPressed: () {
            Navigator.pop(context, 'Cancel');
          },
        ),
      ),
    );
  });
}

void showDemoActionSheet({BuildContext context, Widget child}) {
  showCupertinoModalPopup<String>(
    context: context,
    builder: (BuildContext context) => child,
  ).then((String value) {
    if (value != null) {
      setState(() {});
    }
  });
}
```

# CupertinoActivityIndicator

``CupertinoActivityIndicator`` 仿生了 iOS 原生系统中的 ``Activity Indicators`` 组件，该组件用于创建 Loading。如下图所示。

> 前往 [Activity Indicators](https://developer.apple.com/design/human-interface-guidelines/ios/controls/progress-indicators/#activity-indicators) 了解更多关于 iOS 原生 ``Activity Indicators`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-30 at 13.21.47.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## 构造 & 属性

``CupertinoActionSheet`` 构造函数的实现如下。

```
const CupertinoActivityIndicator({
Key key,
bool animating: true,
double radius: _kDefaultIndicatorRadius
})
```

- ``animating`` 用于指示 Loading 图标是否需要转动。

- ``radius`` 指定了 Loading 的半径。

## 示例代码

此 demo 完整实例如下。

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(widget.title),
    ),
    body: Center(
      child: Column(

        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          CupertinoActivityIndicator(),
          CupertinoActivityIndicator(radius: 30, animating: false,),
          CupertinoActivityIndicator(radius: 100,),
        ],
      ),
    ),
  );
}
```

# CupertinoAlertDialog & CupertinoDialogAction

``CupertinoAlertDialog`` 仿生了 iOS 原生系统中的 ``Alerts`` 组件，该组件用于创建浮层的中央弹窗样式。**根据按钮数量的不同，Alerts 会自动呈现不同的横向或纵向列表样式**，如下图所示。

> 前往 [Alerts](https://developer.apple.com/design/human-interface-guidelines/ios/views/alerts/) 了解更多关于 iOS 原生 ``Alerts`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-30 at 14.58.41.png)

![](/img/in_post/flutter_img/Screen Shot 2019-07-30 at 14.59.07.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。同时，``CupertinoAlertDialog `` 也用于取代 ``CupertinoDialog``，``CupertinoDialog`` 已被标记为 ``@Deprecated``。

## CupertinoAlertDialog

``CupertinoAlertDialog`` 构造函数的实现如下。

```
const CupertinoAlertDialog({
Key key,
Widget title,
Widget content,
List<Widget> actions: const [],
ScrollController scrollController,
ScrollController actionScrollController
})
```

- ``title`` 为顶部标题。此 Demo 中，``title`` 为 *Allow "Maps" to access your location while you are using the app*。

- ``content`` 为标题下的信息。此 Demo 中，``content`` 为 *Your current location will be displayed on the map and used for directions, nearby search results, and estimated travel times*。

- ``actions`` 是一个 Widget List。该值承载列表中的选项按钮。此 List 的值通常使用 ``CupertinoDialogAction`` 为值。详见下方解释。

- 当顶部标题或列表选项过多时，``scrollController`` 及 ``actionScrollController`` 用于滚动控制。这两个值通常不需要设置。

## CupertinoDialogAction

``CupertinoDialogAction`` 构造函数的实现如下。

```
const CupertinoDialogAction({
VoidCallback onPressed,
bool isDefaultAction: false,
bool isDestructiveAction: false,
TextStyle textStyle,
@required Widget child
})
```

在此 demo 中，所有按钮的实体都是通过 ``CupertinoDialogAction`` 实现的。

- ``onPressed`` 用于监听点击事件。

- ``isDefaultAction`` 标记是否使用粗体，本 demo 的 *Allow Always* 按钮体现了该特性。

- ``isDestructiveAction`` 标记是否显示警告样式，即红色字体。本 demo 的 *Don't Allow* 按钮体现了该特性。

## 示例代码

此 demo 完整实例如下。

```
void _onButtonClick() {
  showDemoDialog(
    context: context,
    child: const CupertinoDessertDialog(),
  );
}

void showDemoDialog({BuildContext context, Widget child}) {
  showCupertinoDialog<String>(
    context: context,
    builder: (BuildContext context) => child,
  );
}

class CupertinoDessertDialog extends StatelessWidget {
  const CupertinoDessertDialog({Key key})
      : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CupertinoAlertDialog(
      title: const Text(
          'Allow "Maps" to access your location while you are using the app?'),
      content: const Text(
          'Your current location will be displayed on the map and used '
          'for directions, nearby search results, and estimated travel times.'),
      actions: <Widget>[
        CupertinoDialogAction(
          isDefaultAction: true,
          child: const Text('Allow Always'),
          onPressed: () {
            Navigator.pop(context, 'Allow');
          },
        ),
        CupertinoDialogAction(
          child: const Text('Allow Once'),
          onPressed: () {
            Navigator.pop(context, 'Allow');
          },
        ),
        CupertinoDialogAction(
          isDestructiveAction: true,
          child: const Text('Don\'t Allow'),
          onPressed: () {
            Navigator.pop(context, 'Disallow');
          },
        ),
      ],
    );
  }
}

```

# CupertinoButton

``CupertinoButton`` 仿生了 iOS 原生系统中的 ``Buttons`` 组件，该组件用于创建按钮。如下图所示。

> 前往 [Buttons](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/) 了解更多关于 iOS 原生 ``Buttons`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-30 at 15.48.21.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## Normal Style

``CupertinoButton`` 有两种按钮样式可用。第一种是没有背景描边的 Button，即 demo 截图中上面两个按钮样式。在内部实现中，是通过控制内部成员变量 ``_filled`` 来控制的，详情请阅读源码。Normal Style 的构造函数的实现如下。

```
const CupertinoButton({
Key key,
@required Widget child,
EdgeInsetsGeometry padding,
Color color,
Color disabledColor,
double minSize: 44.0,
double pressedOpacity: 0.1,
BorderRadius borderRadius: const BorderRadius.all(Radius.circular(8.0)),
@required VoidCallback onPressed
})
```

- ``child`` 用于承载需要在按钮中放置的内容。

- ``padding`` 指定了内容与边界的距离。

- ``color`` 及 ``disabledColor `` 分别指定了按钮启用和禁用时的背景色。

- ``minSize`` 按钮的最小 size。

- ``pressedOpacity `` 按下时的透明度。

- ``borderRadius `` 按钮的圆角数据。

## Filled Style

这是 ``CupertinoButton`` 的第二种样式。这是我们跟平时更为常见的样式，含有背景描边的 Button。构造函数的实现如下。

```
const CupertinoButton.filled({
Key key,
@required Widget child,
EdgeInsetsGeometry padding,
Color disabledColor,
double minSize: 44.0,
double pressedOpacity: 0.1,
BorderRadius borderRadius: const BorderRadius.all(Radius.circular(8.0)),
@required VoidCallback onPressed
})
```

他通过 filled 方法来实现另一个构造。所有属性的含义与 Normal Style 是一致的。

## 示例代码

此 demo 完整实例如下。

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(widget.title),
    ),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          const Padding(padding: EdgeInsets.all(12.0)),
          Align(
            alignment: const Alignment(0.0, -0.2),
            child: Row(
              mainAxisSize: MainAxisSize.min,
              children: <Widget>[
                CupertinoButton(
                  child: const Text('Cupertino Button'),
                  onPressed: () {
                    setState(() {});
                  },
                ),
                const CupertinoButton(
                  child: Text('Disabled'),
                  onPressed: null,
                ),
              ],
            ),
          ),
          const Padding(padding: EdgeInsets.all(12.0)),
          CupertinoButton.filled(
            child: const Text('With Background'),
            onPressed: () {
              setState(() {});
            },
          ),
          const Padding(padding: EdgeInsets.all(12.0)),
          const CupertinoButton.filled(
            minSize: 300,
            child: Text('Disabled'),
            onPressed: null,
          ),
        ],
      ),
    ),
  );
}
```

# CupertinoDatePicker

``CupertinoDatePicker`` 仿生了 iOS 原生系统中的 ``Pickers`` 组件，该组件用于创建多个字段的列表选择框。在 ``CupertinoDatePicker`` 中，有三种与日期相关的选择样式，分别是：

- date

- time

- date & time

如下图所示。

> 前往 [Pickers](https://developer.apple.com/design/human-interface-guidelines/ios/controls/pickers/) 了解更多关于 iOS 原生 ``Pickers`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-31 at 11.42.15.png)

> date 类型的 picker

![](/img/in_post/flutter_img/Screen Shot 2019-07-31 at 11.42.18.png)

> time 类型的 picker

![](/img/in_post/flutter_img/Screen Shot 2019-07-31 at 11.42.21.png)

> date & time 类型的 picker

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## 构造 & 属性

``CupertinoDatePicker`` 构造函数的实现如下。

```
CupertinoDatePicker({
CupertinoDatePickerMode mode: CupertinoDatePickerMode.dateAndTime,
@required ValueChanged<DateTime> onDateTimeChanged,
DateTime initialDateTime,
DateTime minimumDate,
DateTime maximumDate,
int minimumYear: 1,
int maximumYear,
int minuteInterval: 1,
bool use24hFormat: false
})
```

- ``mode`` 表示需要使用的选择样式。如 demo 所示，分别使用了 ``CupertinoDatePickerMode`` 枚举中的 ``dateAndTime`` / ``date`` / ``time``。

- ``onDateTimeChanged`` 监听选择的变化。注意，在选择滚动的同时，此接口实时回调。

- ``initialDateTime`` 用于初始化显示时间。

- ``minimumDate`` 和 ``maximumDate`` 用于限定选择日期的下限和上限，此参数当 ``mode`` 为 ``dateAndTime`` 时有效。

- ``minimumYear `` 和 ``maximumYear `` 用于限定选择日期的下限和上限，此参数当 ``mode`` 为 ``date`` 时有效。

- ``minuteInterval`` 在分钟的滚轮中，分钟数的间隔。

- ``use24hFormat`` 用于表示时间是否以 24 小时显示。

## 示例代码

此 demo 完整实例如下。

```
DateTime dateTime = DateTime.now();

void _onButtonClick(CupertinoDatePickerMode mode) {
  setState(() {
    showCupertinoModalPopup<void>(
      context: context,
      builder: (BuildContext context) {
        return _buildBottomPicker(
          CupertinoDatePicker(
            mode: mode,
            initialDateTime: dateTime,
            onDateTimeChanged: (DateTime newDateTime) {
              setState(() => dateTime = newDateTime);
            },
          ),
        );
      },
    );
  });
}

static const double _kPickerSheetHeight = 216.0;

Widget _buildBottomPicker(Widget picker) {
  return Container(
    height: _kPickerSheetHeight,
    padding: const EdgeInsets.only(top: 6.0),
    color: CupertinoColors.white,
    child: DefaultTextStyle(
      style: const TextStyle(
        color: CupertinoColors.black,
        fontSize: 22.0,
      ),
      child: GestureDetector(
        // Blocks taps from propagating to the modal sheet and popping.
        onTap: () {},
        child: SafeArea(
          top: false,
          child: picker,
        ),
      ),
    ),
  );
}
```

# CupertinoPageScaffold & CupertinoNavigationBar

``CupertinoPageScaffold`` 是一个层级非常低的容器。这意味着大多数布局都以它作为根基。作为底层通用容器，看似强大，其实功能非常简单，只提供了几个接口。这使得其作用非常稳固，且不会被滥用。

``CupertinoNavigationBar`` 仿生了 iOS 原生系统中的 ``UINavigationBar`` 组件，该组件用于创建页面的顶部导航栏。通常 ``CupertinoNavigationBar`` 放置在 ``CupertinoPageScaffold`` 的顶部。导航栏由多个小部件组成，**在正确的设计指导中，您应该始终将其放置于顶部**，并且不得作出透明处理——因为他会自动对背景进行高斯模糊。

> 前往 [UINavigationBar](https://developer.apple.com/documentation/uikit/uinavigationbar) 了解更多关于 iOS 原生 ``UINavigationBar`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-31 at 17.31.15.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## CupertinoPageScaffold

``CupertinoPageScaffold`` 构造函数的实现如下。

```
const CupertinoPageScaffold({
Key key,
ObstructingPreferredSizeWidget navigationBar,
Color backgroundColor,
bool resizeToAvoidBottomInset: true,
@required Widget child
})
```

``CupertinoPageScaffold`` 布局分为两部分，一部分是顶部的导航栏，另一部分是剩余的主界面，即 ``child``。

- ``navigationBar`` 用于放置导航栏，详细定义见 ``CupertinoNavigationBar`` 部分。

- ``backgroundColor`` 设置主布局的背景色。

- ``resizeToAvoidBottomInset `` 当底部被遮挡后，是否重新计算布局大小。例如，当键盘从底部升起。

## CupertinoNavigationBar

``CupertinoNavigationBar`` 构造函数的实现如下。

```
const CupertinoNavigationBar({
Key key,
Widget leading,
bool automaticallyImplyLeading: true,
bool automaticallyImplyMiddle: true,
String previousPageTitle,
Widget middle,
Widget trailing,
Border border: _kDefaultNavBarBorder,
Color backgroundColor,
EdgeInsetsDirectional padding,
Color actionsForegroundColor,
bool transitionBetweenRoutes: true,
Object heroTag: _defaultHeroTag
})
```

- ``leading`` 用于设置左侧组件，``automaticallyImplyLeading`` 则用于设置组件是否可见。

- ``automaticallyImplyMiddle`` 设置中间的组件是否可见。

- ``previousPageTitle`` 当通过路由前往下一级菜单后，「返回」按钮所显示的文字。

- ``middle`` 中间的文字。

- ``border`` 设置导航栏的边框，默认情况下是为 1px 的描边。

- ``backgroundColor`` 设置导航栏背景色。

- ``padding`` 导航栏中的元素距离距离边缘的 padding 值设置。

- ``actionsForegroundColor`` 左侧组件的颜色。

## 示例代码

此 demo 完整实例如下。

```
class _MyPageState extends State {
  @override
  Widget build(BuildContext context) {
    return ChatPage();
  }
}

class ChatPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      backgroundColor: Color(0xffffaaaa),
      navigationBar: CupertinoNavigationBar(
        middle: Text("聊天面板"),
        trailing: Icon(CupertinoIcons.add),
        leading: Icon(CupertinoIcons.back),
      ),
      child: Center(
        child: Text(
          'This is 聊天面板 layout',
          style: Theme.of(context).textTheme.button,
        ),
      ),
    );
  }
}
```

# CupertinoPicker

``CupertinoPicker`` 仿生了 iOS 原生系统中的 ``Pickers`` 组件，该组件用于创建滚动的列表选择框。如下图所示。

> 前往 [Pickers](https://developer.apple.com/design/human-interface-guidelines/ios/controls/pickers/) 了解更多关于 iOS 原生 ``Pickers`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-31 at 22.30.19.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## 构造 & 属性

``CupertinoPicker`` 构造函数的实现如下。

```
CupertinoPicker({
Key key,
double diameterRatio: _kDefaultDiameterRatio,
Color backgroundColor: _kDefaultBackground,
double offAxisFraction: 0.0,
bool useMagnifier: false,
double magnification: 1.0,
FixedExtentScrollController scrollController,
double squeeze: _kSqueeze,
@required double itemExtent,
@required ValueChanged<int> onSelectedItemChanged,
@required List<Widget> children,
bool looping: false
})
```

- ``diameterRatio`` 表示该滚动柱子的大小。仔细观察，``CupertinoPicker`` 是一个立体的圆柱。

- ``backgroundColor`` 背景色。

- ``offAxisFraction`` 表示该滚动柱子左右偏移的角度。

- ``scrollController`` 滚动控制器。一般不需要进行设置。

- ``squeeze`` 每个 item 之间的紧凑度。

- ``itemExtent`` item 的高度。

- ``onSelectedItemChanged`` item 选择的回调。

- ``children`` 数据集。

- ``looping`` 列表是否持续循环，永无尽头。

## 示例代码

此 demo 完整实例如下。

```
showCupertinoModalPopup<void>(
context: context,
builder: (BuildContext context) {
return _buildColorPicker(context);
},
);

static const double _kPickerSheetHeight = 600.0;
static const double _kPickerItemHeight = 32.0;
static const List<String> coolColorNames = <String>[
  'Sarcoline',
  'Coquelicot',
  'Smaragdine',
  'Mikado',
  'Glaucous',
  'Wenge',
  'Fulvous',
  'Xanadu',
  'Falu',
  'Eburnean',
  'Amaranth',
  'Australien',
  'Banan',
  'Falu',
  'Gingerline',
  'Incarnadine',
  'Labrador',
  'Nattier',
  'Pervenche',
  'Sinoper',
  'Verditer',
  'Watchet',
  'Zaffre',
];

Widget _buildColorPicker(BuildContext context) {
  return _buildBottomPicker(
    CupertinoPicker(
      looping: true,
      itemExtent: _kPickerItemHeight,
      backgroundColor: CupertinoColors.white,
      onSelectedItemChanged: (int index) {
        setState(() => _selectedColorIndex = index);
      },
      children: List<Widget>.generate(coolColorNames.length, (int index) {
        return Center(
          child: Text(coolColorNames[index]),
        );
      }),
    ),
  );
}

Widget _buildBottomPicker(Widget picker) {
  return Container(
    height: _kPickerSheetHeight,
    padding: const EdgeInsets.only(top: 6.0),
    color: CupertinoColors.white,
    child: DefaultTextStyle(
      style: const TextStyle(
        color: CupertinoColors.black,
        fontSize: 22.0,
      ),
      child: GestureDetector(
        // Blocks taps from propagating to the modal sheet and popping.
        onTap: () {},
        child: SafeArea(
          top: false,
          child: picker,
        ),
      ),
    ),
  );
}
```

# CupertinoSlider

``CupertinoSlider`` 仿生了 iOS 原生系统中的 ``Sliders`` 组件，该组件用于创建滑动条。如下图所示。

> 前往 [Sliders](https://developer.apple.com/design/human-interface-guidelines/ios/controls/sliders/) 了解更多关于 iOS 原生 ``Sliders`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-07-31 at 23.03.36.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## 构造 & 属性

``CupertinoSlider`` 构造函数的实现如下。

```
const CupertinoSlider({
Key key,
@required double value,
@required ValueChanged<double> onChanged,
ValueChanged<double> onChangeStart,
ValueChanged<double> onChangeEnd,
double min: 0.0,
double max: 1.0,
int divisions,
Color activeColor
})
```

- ``value`` 为初始化值。

- ``onChanged`` 当值改变时持续回调此接口。

- ``onChangeStart`` 在一次滚动周期中，第一次改变的值。

- ``onChangeEnd`` 在一次滚动周期中，最后一次改变的值。

- ``min`` 允许的最小值。

- ``max`` 允许的最大值。

- ``divisions`` 值之间的间隔。demo 效果中第二个效果。

- ``activeColor`` 滑动条的颜色。demo 效果中第二个效果。

## 示例代码

此 demo 完整实例如下。

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(widget.title),
    ),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.start,
        children: <Widget>[
          const Padding(padding: EdgeInsets.all(12.0)),
          Text(
            'CupertinoSlider',
            style: TextStyle(fontWeight: FontWeight.bold, fontSize: 30),
          ),
          const Padding(padding: EdgeInsets.all(12.0)),
          Column(
            mainAxisSize: MainAxisSize.min,
            children: <Widget>[
              CupertinoSlider(
                value: _value,
                min: 0.0,
                max: 100.0,
                onChanged: (double value) {
                  setState(() {
                    _value = value;
                  });
                },
              ),
              Text('普通的 Continuous: ${_value.toStringAsFixed(1)}'),
            ],
          ),
          Column(
            mainAxisSize: MainAxisSize.min,
            children: <Widget>[
              CupertinoSlider(
                activeColor: Color(0xffff0000),
                value: _discreteValue,
                min: 0.0,
                max: 100.0,
                divisions: 5,
                onChanged: (double value) {
                  setState(() {
                    _discreteValue = value;
                  });
                },
              ),
              Text('带间隔的 Discrete，只能是 20 的倍数: $_discreteValue'),
            ],
          ),
        ],
      ),
    ),
  );
}
```

# CupertinoSwitch

``CupertinoSwitch`` 仿生了 iOS 原生系统中的 ``Switches`` 组件，该组件用于创建开关按钮。

开关本身不维护任何状态，外部唯一能感知的，只要开关状态被切换时的回调监听。因此，当开关状态变化时，你应当实时修改你的值。同样，当你的值发生变化时，应当及时更新开关状态。

> 前往 [Switches](https://developer.apple.com/design/human-interface-guidelines/ios/controls/switches/) 了解更多关于 iOS 原生 ``Switches`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-08-01 at 15.27.03.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## 构造 & 属性

``CupertinoSwitch`` 构造函数的实现如下。

```
const CupertinoSwitch({
Key key,
@required bool value,
@required ValueChanged<bool> onChanged,
Color activeColor,
DragStartBehavior dragStartBehavior: DragStartBehavior.start
})
```

- ``value`` 用于指定初始化状态为 on 或 off。

- ``onChanged`` 用于监听按钮的变化。**如果此值为 null，按钮则会自动变为 disable**。

- ``activeColor`` 按钮的颜色。

## 示例代码

此 demo 完整实例如下。

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(widget.title),
    ),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.start,
        children: <Widget>[
          const Padding(padding: EdgeInsets.all(12.0)),
          Text(
            'CupertinoSwitch',
            style: TextStyle(fontWeight: FontWeight.bold, fontSize: 30),
          ),
          const Padding(padding: EdgeInsets.all(12.0)),
          CupertinoSwitch(
            dragStartBehavior: DragStartBehavior.down,
            value: _switchValue,
            activeColor: Color(0xffff0000),
            onChanged: (bool value) {
              setState(() {
                _switchValue = value;
              });
            },
          ),
          Text("Enabled - ${_switchValue ? "On" : "Off"}"),
          CupertinoSwitch(
            value: true,
            activeColor: Color(0xffff0000),
            onChanged: null,
          ),
          Text('Disabled - On'),
          CupertinoSwitch(
            value: true,
            onChanged: null,
          ),
          Text('Disabled - On'),
          CupertinoSwitch(
            value: false,
            onChanged: null,
          ),
          Text('Disabled - Off'),
        ],
      ),
    ),
  );
}
```

# CupertinoTabScaffold & CupertinoTabBar & CupertinoTabView

``CupertinoTabScaffold`` 仿生了 iOS 原生系统中的 ``Tab Bars`` 组件，该组件用于创建屏幕底部的 tab 交互，及选中 tab 的布局切换。它分为两部分：底部的 tab 栏，以及剩余的空白区域。

``CupertinoTabBar`` 用于填充 ``CupertinoTabScaffold`` 中的 tab 栏，用户点击 tab 中不同的 item，以切换 ``CupertinoTabScaffold`` 中的空白区域。

``CupertinoTabView`` 则用于填充 ``CupertinoTabScaffold`` 中的空白区域，``CupertinoTabView`` 能通过参数指定需要加载的 widget，典型的用法是，当 ``CupertinoTabBar`` 的 item 被点击时，通知 ``CupertinoTabView`` 切换主界面。

需要注意的是，``CupertinoTabBar`` item 数量应当与 ``CupertinoTabView`` 所能切换的数量相等。当 ``CupertinoTabView`` 的界面被移出可视范围时，动画会被禁用。此外，虽然 ``CupertinoTabScaffold`` 底部 tab 支持制表符，但不建议这样操作。因为这违反了 iOS 交互指南。

> 前往 [iOS 人机交互指南](https://developer.apple.com/design/human-interface-guidelines/ios/bars/tab-bars/) 了解更多关于 iOS 原生 ``Tab Bars`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-08-01 at 17.04.23.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## CupertinoTabScaffold

``CupertinoTabScaffold`` 构造函数的实现如下。

```
CupertinoTabScaffold({
Key key,
@required CupertinoTabBar tabBar,
@required IndexedWidgetBuilder tabBuilder,
CupertinoTabController controller,
Color backgroundColor,
bool resizeToAvoidBottomInset: true
})
```

- ``tabBar`` 用于放置 ``CupertinoTabBar``。

- ``tabBuilder`` 用于放置 ``CupertinoTabView``。

- ``controller`` 自定义的控制器，如果您需要对切换行为自定义需求。默认情况下内置的控制器会自行管理。

- ``backgroundColor`` 背景色设置。

- ``resizeToAvoidBottomInset`` 当底部有其他内容入侵时，是否需要对界面进行重新计算。典型的情况是，底部键盘弹起时。

## CupertinoTabBar

``CupertinoTabBar`` 构造函数的实现如下。

```
const CupertinoTabBar({
Key key,
@required List<BottomNavigationBarItem> items,
ValueChanged<int> onTap,
int currentIndex: 0,
Color backgroundColor,
Color activeColor,
Color inactiveColor: CupertinoColors.inactiveGray,
double iconSize: 30.0,
Border border: const Border(top: BorderSide(color: _kDefaultTabBarBorderColor, width: 0.0, style: BorderStyle.solid))
})
```

- ``items`` 这是一个列表，用于指定每个 tab 的文字和 icon。

- ``onTap`` 当 tab 发生切换时回调。

- ``currentIndex`` 设置当前选中的 index，注意别的超过 ``items`` 的长度。

- ``backgroundColor`` tab 的背景色。

- ``activeColor`` item 被选中时元素的颜色。

- ``inactiveColor`` item 未被选中时元素的颜色。

- ``iconSize`` item 中 icon 的大小，即 ``BottomNavigationBarItem`` 中 ``activeIcon`` 的大小。

- ``border`` tab 描边的颜色。默认是 1px 的描边，一般情况不需要设置。

## CupertinoTabView

``CupertinoTabView`` 构造函数的实现如下。

```
const CupertinoTabView({
Key key,
WidgetBuilder builder,
GlobalKey<NavigatorState> navigatorKey,
String defaultTitle,
Map<String, WidgetBuilder> routes,
RouteFactory onGenerateRoute,
RouteFactory onUnknownRoute,
List<NavigatorObserver> navigatorObservers: const []
})
```

- ``builder`` 页面的构造器，通过所给予的参数决定需要构造什么样的 widget。典型的做法，即通过 ``CupertinoTabBar`` 中选中 item 的 index 做不同切换。

- ``defaultTitle`` 在路由中的默认名字。

- ``currentIndex`` 设置当前选中的 index，注意别的超过 ``items`` 的长度。

- ``routes`` 用于承载每个界面的路由信息。

- ``onGenerateRoute`` 为路由行为回调，当此行为发生异常时，则通过 ``onUnknownRoute`` 回调。

- ``navigatorObservers`` 导航器的观察者列表。

## 示例代码

此 demo 完整实例如下。

```
class _MyPageState extends State {
  @override
  Widget build(BuildContext context) {
    //最外层导航选项卡
    return CupertinoTabScaffold(
      //底部选项卡
      tabBar: CupertinoTabBar(
        activeColor: Color(0xffff0000),
        backgroundColor: CupertinoColors.lightBackgroundGray, //选项卡背景色
        items: [
          //选项卡项 包含图标及文字
          BottomNavigationBarItem(
            icon: Icon(CupertinoIcons.home),
            title: Text('主页'),
          ),
          BottomNavigationBarItem(
            icon: Icon(CupertinoIcons.conversation_bubble),
            title: Text('聊天'),
          ),
        ],
      ),
      tabBuilder: (context, index) {
        //选项卡绑定的视图
        return CupertinoTabView(
          builder: (context) {
            switch (index) {
              case 0:
                return HomePage();
                break;
              case 1:
                return ChatPage();
                break;
              default:
                return Container();
            }
          },
        );
      },
    );
  }
}

class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      backgroundColor: Color(0x6655500a),
      navigationBar: CupertinoNavigationBar(
        middle: Text("主页"),
      ),
      child: Center(
        child: Text(
          'This is 主页 layout',
          style: Theme.of(context).textTheme.button,
        ),
      ),
    );
  }
}

class ChatPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      backgroundColor: Color(0xffffaaaa),
      navigationBar: CupertinoNavigationBar(
        middle: Text("聊天面板"),
        trailing: Icon(CupertinoIcons.add),
        leading: Icon(CupertinoIcons.back),
      ),
      child: Center(
        child: Text(
          'This is 聊天面板 layout',
          style: Theme.of(context).textTheme.button,
        ),
      ),
    );
  }
}```

# CupertinoTextField

``CupertinoTextField`` 仿生了 iOS 原生系统中的 ``UITextField`` 组件，该组件用于创建输入框。

值得注意的是，**Flutter 几乎把整个 iOS 键盘输入特性搬了过来，包括长按空格键移动光标这一牛逼特性**。你可以使用此 widget 实现几乎仿真 iOS 的交互体验，最后不要忘记释放 ``TextEditingController``。

> 前往 [UITextField](https://developer.apple.com/documentation/uikit/uitextfield) 了解更多关于 iOS 原生 ``UITextField`` 的详细内容。

![](/img/in_post/flutter_img/Screen Shot 2019-08-01 at 21.51.30.png)

需要使用此特性，您需要在工程文件的头部引入 ``package:flutter/cupertino.dart``。

## 构造 & 属性

``CupertinoSwitch`` 构造函数的实现如下。

```
const CupertinoTextField({
Key key,
TextEditingController controller,
FocusNode focusNode,
BoxDecoration decoration: _kDefaultRoundedBorderDecoration,
EdgeInsetsGeometry padding: const EdgeInsets.all(6.0),
String placeholder,
TextStyle placeholderStyle: const TextStyle(fontWeight: FontWeight.w300, color: _kInactiveTextColor),
Widget prefix,
OverlayVisibilityMode prefixMode: OverlayVisibilityMode.always,
Widget suffix,
OverlayVisibilityMode suffixMode: OverlayVisibilityMode.always,
OverlayVisibilityMode clearButtonMode: OverlayVisibilityMode.never,
TextInputType keyboardType,
TextInputAction textInputAction,
TextCapitalization textCapitalization: TextCapitalization.none,
TextStyle style,
StrutStyle strutStyle,
TextAlign textAlign: TextAlign.start,
bool readOnly: false,
bool showCursor,
bool autofocus: false,
bool obscureText: false,
bool autocorrect: true,
int maxLines: 1,
int minLines,
bool expands: false,
int maxLength,
bool maxLengthEnforced: true,
ValueChanged<String> onChanged,
VoidCallback onEditingComplete,
ValueChanged<String> onSubmitted,
List<TextInputFormatter> inputFormatters,
bool enabled,
double cursorWidth: 2.0,
Radius cursorRadius: const Radius.circular(2.0),
Color cursorColor,
Brightness keyboardAppearance,
EdgeInsets scrollPadding: const EdgeInsets.all(20.0),
DragStartBehavior dragStartBehavior: DragStartBehavior.start,
bool enableInteractiveSelection,
ScrollController scrollController,
ScrollPhysics scrollPhysics
})
```

- ``autocorrect`` 是否启用语法自动修正。

- ``autofocus`` 当文本框获得焦点时，是否自动弹出键盘。否则只有用户点击时才会弹出。

- ``clearButtonMode`` 在空间尾部增加一个清除按钮。

- ``controller`` 文本的输入控制器。如果你有需要监听用户的输入行为，则需要自定义。

- ``cursorColor`` 光标的颜色。

- ``cursorRadius`` 光标的圆角半径。

- ``cursorWidth`` 光标的宽度。

- ``decoration`` 控件文本输入后框的框装饰。默认情况下有一个圆角矩形灰色边框，可以为空。

- ``enabled`` 如果为 false 则为禁用此输入框，并且背景为灰色。此时 touch 事件都将不会响应。

- ``enableInteractiveSelection`` 默认为 true，用户可以长按文字进行复制粘贴等操作。反之为 false。

- ``inputFormatters`` 提供你的格式验证和重写算法。widget 会依照你所提供的顺序执行。

- ``keyboardAppearance`` 键盘的外观，仅 iOS 可用。

- ``keyboardType`` 键盘的类型。

- ``maxLength`` 最大允许的字符数。

- ``maxLengthEnforced`` 如果为 true，则强行阻止其超过最大数限制，否则仅仅是警告。默认为 false。

- ``maxLines`` 为最大允许的行数，``minLines`` 则为最小允许的行数。

- ``obscureText`` 是否使用隐藏文本。例如，输入密码时。

- ``onChanged`` 当文本发生变化时回调。

- ``onEditingComplete`` 当用户按下键盘的「完成（Done）」时回调。

- ``placeholder`` 当文本框无内容时的浅色占位符。

- ``placeholderStyle`` 浅色占位符的样式。

- ``prefix`` 在文本框头部显示的前缀小控件，``prefixMode`` 设置前缀小控件的显示方式。

- ``readOnly`` 此编辑框是否仅仅是只读的模式。

- ``showCursor`` 光标是否显示。

- ``suffix`` 在文本框尾部显示的后缀小控件，``suffixMode`` 设置后缀小控件的显示方式。

- ``textAlign`` 在水平的方向上，文本要以何种方式对齐。

- ``textCapitalization`` 指定键盘的大小写显示模式。

- ``textInputAction`` 键盘右下角的「Enter」按钮应当显示什么文字。例如，「完成」，「加入」等样式。

## 示例代码

此 demo 完整实例如下。

```
class MyPrefilledText extends StatefulWidget {
  @override
  _MyPrefilledTextState createState() => _MyPrefilledTextState();
}

class _MyPrefilledTextState extends State<MyPrefilledText> {
  TextEditingController _textController;

  @override
  void initState() {
    super.initState();
    _textController = TextEditingController(text: 'input here!');
  }

  @override
  Widget build(BuildContext context) {
    return CupertinoTextField(
      clearButtonMode: OverlayVisibilityMode.always,
      keyboardAppearance: Brightness.dark,
      controller: _textController,
        cursorRadius: Radius.circular(40),
      cursorWidth: 8,
      cursorColor: Color(0xffff0000),
      autocorrect: true,
    );
  }
}
```

