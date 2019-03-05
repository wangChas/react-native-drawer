
### USAGE WARNING
This module is not as performant as it should be. For production applications we generally recommend using either [React Native Side Menu](https://github.com/react-native-community/react-native-side-menu) or [React Navigation](https://github.com/react-navigation/react-navigation) as applicable. React Native Drawer will continue to be available and potentially useful for its high customizability but again it is **not** recommended for production appliciations. If you are interested in revamping react native drawer to be more performant (i.e. use Animated) please get in touch!


## React Native Drawer
<img width="220px" align="right" src="https://raw.githubusercontent.com/rt2zz/react-native-drawer/master/examples/rn-drawer.gif" />

React native drawer, configurable to achieve material design style, slack style, parallax, and more. Works in both iOS and Android.

[![npm version](https://img.shields.io/npm/v/react-native-drawer.svg?style=flat-square)](https://www.npmjs.com/package/react-native-drawer)
[![npm downloads](https://img.shields.io/npm/dm/react-native-drawer.svg?style=flat-square)](https://www.npmjs.com/package/react-native-drawer)

- [安装](#安装)
- [用法](#用法)
- [例子](#例子)
- [参数](#参数)
- [案例](#案例)
- [Credits](#credits)

### 安装
`npm install --save react-native-drawer`

### 用法
```javascript
import Drawer from 'react-native-drawer'

class Application extends Component {  
  closeControlPanel = () => {
    this._drawer.close()
  };
  openControlPanel = () => {
    this._drawer.open()
  };
  render () {
    return (
      <Drawer
        ref={(ref) => this._drawer = ref}
        content={<ControlPanel />}
        >
        <MainView />
      </Drawer>
    )
  }
})
```

### 例子
```js
//Parallax Effect (slack style)
<Drawer
  type="static"
  content={<ControlPanel />}
  openDrawerOffset={100}
  styles={drawerStyles}
  tweenHandler={Drawer.tweenPresets.parallax}
  >
    <Main />
</Drawer>

//Material Design Style Drawer
<Drawer
  type="overlay"
  content={<ControlPanel />}
  tapToClose={true}
  openDrawerOffset={0.2} // 20% gap on the right side of drawer
  panCloseMask={0.2}
  closedDrawerOffset={-3}
  styles={drawerStyles}
  tweenHandler={(ratio) => ({
    main: { opacity:(2-ratio)/2 }
  })}
  >
    <Main />
</Drawer>

const drawerStyles = {
  drawer: { shadowColor: '#000000', shadowOpacity: 0.8, shadowRadius: 3},
  main: {paddingLeft: 3},
}
```

### 参数

该插件支持大量的抽屉样式，因此拥有*大量*参数
##### 重要的
|参数|参数类型|默认值|原义|译意|
|---|----|----|---|---|
| content |React.Component| `null` |Menu component|菜单组件|
| type| String: displace:overlay:static| displace |Type of drawer.|定义抽屉类型,displace:替换, overlay:覆盖, static:静态，open参数无效|
| open | Boolean | `null` |If true will trigger drawer open, if false will trigger close.|如果为`true`则触发抽屉打开，而为`false`触发器关闭|
| openDrawerOffset | Number&#124;Function | 0 |Can either be a integer (pixel value) or decimal (ratio of screen width). Defines the right hand margin when the drawer is open. Or, can be function which returns offset: `(viewport) => viewport.width - 200`| 可以使用整型（像素值）或者小数（屏幕宽度的比率）。定义当抽屉打开时离右边缘的距离。或者可以用返回偏移量的函数：`(viewport) => viewport.width - 200`|
| closedDrawerOffset | Number&#124;Function | 0 |Same as openDrawerOffset, except defines left hand margin when drawer is closed.| 和openDrawerOffset一样，不过定义的是当抽屉关闭时离左边缘的距离|
| disabled | Boolean | `false` |If true the drawer can not be opened and will not respond to pans.| 如果为`true`的话，抽屉不能被打开和作出反应|
| styles | Object | `null` |Styles for the drawer, main, drawerOverlay and mainOverlay container Views.| 抽屉的样式，主要是drawerOverlay和mainOverlay内容视图|

##### Animation / Tween
**Note**: 在未来，使用Animation的动画和接口将会被改变

|参数|参数类型|默认值|原义|译意|
|---|----|----|---|---|
| tweenHandler|Function| `null`| Takes in the pan ratio (decimal 0 to 1) that represents the tween percent. Returns an object of native props to be set on the constituent views { drawer: {/*native props*/}, main: {/*native props*/}, mainOverlay: {/*native props*/} } | 调节小数点比率（0到1）代表渐变百分比，返回一个设置元素原生参数的对象：{ drawer: {/*native props*/}, main: {/*native props*/}, mainOverlay: {/*native props*/} }|
| tweenDuration | Integer | 250 | The duration of the open/close animation.|打开/关闭的动画的持续时间|
| tweenEasing | String | linear |A easing type supported by [tween-functions](https://www.npmjs.com/package/tween-functions)|由[tween-functions](https://www.npmjs.com/package/tween-functions)支持的缓存类型|

##### 时间处理器
|参数|参数类型|默认值|原义|译意|
|---|----|----|---|---|
| onOpen |Function ||Will be called immediately after the drawer has entered the open state.|在抽屉进入打开状态后立刻进行回调|
| onOpenStart | Function | |callback fired at the start of an open animation.|一个打开动画开始时触发回调|
| onClose | Function || Will be called immediately after the drawer has entered the closed state.|在抽屉进入关闭状态后立刻进行回调|
| onCloseStart | Function || callback fired at the start of a close animation.|一个关闭动画开始时触发回调|

##### 手势
|参数|参数类型|默认值|原义|译意|
|---|----|----|---|---|
| captureGestures | oneOf(true, false, 'open', 'closed')|`open`|If true, will capture all gestures inside of the pan mask. If 'open' will only capture when drawer is open.|如果为`true`时，会捕捉所有在pan掩膜中的手势.如果为`open`则只会捕捉当抽屉打开的时候的手势|
| acceptDoubleTap |Boolean|`false`| Toggle drawer when double tap occurs within pan mask?|当双击掩膜时会触发抽屉事件|
| acceptTap |Boolean|`false`|Toggle drawer when any tap occurs within pan mask?|当点击掩膜时会触发抽屉事件|
| acceptPan | Boolean |`true`|Allow for drawer pan (on touch drag). Set to false to effectively disable the drawer while still allowing programmatic control.|允许抽屉的区域触碰拖拽。设置为`false`来有效地禁用抽屉同时仍然允许编程控制。|
| tapToClose | Boolean |`false`|Same as acceptTap, except only for close.|和acceptTap功能相同，除了只在关闭状态下|
| negotiatePan | Boolean |`false`|If true, attempts to handle only horizontal swipes, making it play well with a child `ScrollView`.|如果为真，尝试只去处理一些水平滑动事件，让他与子控件`ScrollView`更好的运作|

##### 额外配置项
|参数|参数类型|默认值|原义|译意|
|---|----|----|---|---|
| panThreshold | Number |`.25`|Ratio of screen width that must be travelled to trigger a drawer open/close.|前往触发抽屉打开或关闭时的屏幕宽度比率|
| panOpenMask | Number |`null`|Ratio of screen width that is valid for the start of a pan open action. If null -> defaults to `max(.05, closedDrawerOffset)`.|在抽屉范围打开操作开始时有效调节屏幕宽度比例，如果为`null`则默认传值`max(.05, closedDrawerOffset)`|
| panCloseMask | Number |`null`| Ratio of screen width that is valid for the start of a pan close action. If null -> defaults to `max(.05, openDrawerOffset)`. |抽屉范围关闭操作开始时有效调节屏幕宽度比例，如果为`null`则默认传值`max(.05, openDrawerOffset)`|
| initializeOpen | Boolean |`false`| Initialize with drawer open? |是否初始化抽屉打开状态|
| side |(String left|right|top|bottom)|`left`|which side the drawer should be on.|抽屉打开的方向|
| useInteractionManager | Boolean |`false`|if true will run InteractionManager for open/close animations.|如果为真时，为打开/关闭动画运行交互管理器|
| elevation | Number |`0`|(Android-only) Sets the elevation of the drawer using Android's underlying [elevation API](https://developer.android.com/training/material/shadows-clipping.html#Elevation)|（只限于Android）|

### Tween Handler
You can achieve pretty much any animation you want using the tween handler with the transformMatrix property. E.G.
```js
tweenHandler={(ratio) => {
  var r0 = -ratio/6
  var r1 = 1-ratio/6
  var t = [
             r1,  r0,  0,  0,
             -r0, r1,  0,  0,
             0,   0,   1,  0,
             0,   0,   0,  1,
          ]
  return {
    main: {
      style: {
        transformMatrix: t,
        opacity: 1 - ratio/2,
      },
    }
  }
}}
```
Will result in a skewed fade out animation.

### Opening & Closing the Drawer Programmatically
Three options:

1. Use the open prop (controlled mode):  

    ```js
    <Drawer
      open={true}
    ```

2. Using the Drawer Ref:

    ```js
    // assuming ref is set up on the drawer as (ref) => this._drawer = ref
    onPress={() => {this._drawer.open()}}
    ```

3. Using Context

    ```js
    contextTypes = {drawer: React.PropTypes.object}
    // later...
    this.context.drawer.open()
    ```

### 案例
* `git clone https://github.com/rt2zz/react-native-drawer.git`
* `cd react-native-drawer/examples/RNDrawerDemo && npm install`
* **iOS**
	* Open `./examples/RNDrawerDemo/RNDrawerDemo.xcodeproject` in xcode
	* `command+r` (in xcode)
* **Android**
	* Run android simulator / plug in your android device
	* Run `react-native run-android` in terminal

### Credits
Component was adapted from and inspired by
[@khanghoang](https://github.com/khanghoang)'s [RNSideMenu](https://github.com/khanghoang/RNSideMenu)
*AND*
[@kureevalexey](https://twitter.com/kureevalexey)'s [react-native-side-menu](https://github.com/Kureev/react-native-side-menu)
