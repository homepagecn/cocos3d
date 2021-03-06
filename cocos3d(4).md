# cocos3d(4)
## 层
Cocos3D被创建的环境中运行的Cocos2D。一个Cocos3D 层形成的2D环境之间的桥梁的Cocos2D，和3D环境Cocos3D。

一个Cocos3D层是由表示CC3Layer类。CC3Layer是2D的子类的Cocos2D CCNode类，因此可以放置2D的结构层次结构中的Cocos2D节点。它有一个2D节点作为父，并且可具有2D节点作为子节点。

的每个实例CC3Layer保存单个3D场景，作为一个实例CC3Scene，在其cc3Scene属性，并触发3D场景内的3D内容的呈现其自己的渲染的一部分，由此形成的2D内容之间的桥梁的Cocos2D，并且3D内容的Cocos3D。你能想到的CC3Layer是提供通过它查看3D场景二维门户。

### 定制 CC3Layer
如同CC3Scene类，通 ​​常，您创建一个自定义子类CC3Layer来定义层的行为。大多数情况下，你创建一个自定义CC3Layer的每个自定义子类CC3Scene的子类，通 ​​常他们的名字以兼容的方式，比如MyLevel2Layer和MyLevel2Scene。

这种设计模式是如此普遍，如果你没有明确设置cc3Scene自定义的属性CC3Layer例如，当cc3Scene第一次被访问的财产，您的自定义CC3Layer实例将自动实例匹配的CC3Scene实例来填充该属性的基础上，寻找CC3Scene一个子类命名结构相同的CC3Layer子类名称。例如，一个名为层子类MyLevel2Layer将寻找一个CC3Scene指定的子类MyLevel2Scene，MyLevel2或MyLevel2LayerScene，并命名为图层子类MyLevel2将寻找一个CC3Scene指定的子类MyLevel2Scene。

您也可以修改您的自定义这种模式CC3Layer通过重写子类cc3SceneClass属性访问器方法返回一个不同的CC3Scene子类。最后，你总是可以绕过这个自动化和实例化自己的实例CC3Scene子类，该实例设置成cc3Scene您的自定义的任何实例的属性CC3Layer子类。

使用的最后两个技术之一允许初始化相同的不同实例CC3Layer使用的子类有几个不同的自定义CC3Scene子类。例如，如果您正在构建一个游戏，用户的交互是你的游戏的各个层面一样，你可能只需要一个定制的CC3Layer用于子与许多不同的CC3Scene子类，每个自定义为您的游戏不同的级别。

CC3Layer还提供了cocos2d的计划更新机制和3D场景之间的桥梁。如果你想updateBeforeTransform:和updateAfterTransform:您的自定义的方法CC3Scene被调用等3D节点，一定要调用scheduleUpdate自定义的方法，CC3Layer其初始化期间的子类。


### 用户交互
因为CC3Layer是2D节点，它可以参与的Cocos2D节点结构组件。特别是，您可以添加其他的2D节点的子节点的CC3Layer实例，以用作用户界面控件，如按钮，菜单，游戏杆，标签和抬头显示器。

像其他2D节点，如果它的userInteractionEnabled属性已被设置为YES，您的CC3Layer实例可以与用户接口事件诸如触摸事件或鼠标事件交互。触摸在您的事件和鼠标事件CC3Layer被自动传递到CC3Scene实例解释（通常选择触摸点下3D节点）。

CC3Layer 还提供了一些方便的方法来帮助你用手势作为用户交互的一部分：

cc3AddGestureRecognizer:
cc3ConvertUIPointToNodeSpace:
cc3ConvertUIMovementToNodeSpace:
cc3NormalizeUIMovement:
cc3WillConsumeTouchEventAt:

### 层配置和行为
存在由提供几个模板的方法和接入点CC3Layer可以覆盖或访问来构建用户界面，包括：

initializeControls-时，这种方法被自动调用CC3Layer被实例化。您可以覆盖此方法来添加用户界面控件，如按钮，菜单，游戏杆，标签和抬头显示器，为2D子节点到您的自定义CC3Layer实例。

scheduleUpdate-从调用此方法initializeControls的方法将确保层和3D场景，在每个渲染帧的开始接收更新的呼叫，允许您控制您的应用程序中的作用。一旦这个方法被调用，您的自定义CC3Scene将获得其定期调用updateBeforeTransform:和updateAfterTransform:方法。

userInteractionEnabled-在initializeControls方法，将该属性设置为YES允许层，3D场景，以接收触摸事件和鼠标事件。将此属性设置为NO，如果你喜欢使用手势处理程序来提供用户交互。

onOpenCC3Layer - 调用此方法首先显示的层之前。在这一点上，认为，在其上层和3D场景将渲染，已创建。您可以覆盖此方法以手势识别器添加到您的层，如上所述。

update:-如果你已经使用计划定期更新scheduleUpdate的方法，这种update:方法将每一帧上调用。在这里，您可以更新任何2D的控制您添加或从动态控件，如滑块或操纵杆到您的自定义传递数据CC3Scene实例。如果覆盖此方法时，一定要调用超类实现，因此它可以通过更新通知CC3Scene实例。

ccTouchMoved:withEvent: - 默认情况下触摸移动和鼠标移动事件没有处理，因为他们是汗牛充栋，很少使用。您可以覆盖此方法，通过它们传递给标准的触摸处理方法来处理触摸移动和鼠标移动事件：

```
-(void) ccTouchMoved: (UITouch *)touch withEvent: (UIEvent *)event {
    [self handleTouch: touch ofType: kCCTouchMoved];
}
```
### 添加图层到cocos2d的节点环境

如上所述，CC3Layer是2D的一个子类CCNode，因此可以放置2D的结构层次结构中的Cocos2D节点。当你建立你的2D节点的装配，可以添加一个或多个CC3Layer实例的组合，为客户提供3D内容。

在许多应用，3D内容就是一切（除了2D控制），并在一个大场景呈现（例如医疗成像应用程序，或标准的第一人称射击游戏）。在这种情况下，您的自定义CC3Layer是所有你需要，你可以直接将它添加到CCDirector通过包装定制CC3Layer的实例，CCScene实例，这样它可以被运行CCDirector。

这是这样一个共同的要求，即CC3Layer支持一个便捷方法asCCScene，包装了CC3Layer一个CCScene实例，以便您的CC3Layer实例是一个子节点CCScene的实例，并返回该CCScene实例。这种便利功能，允许您使用以下内容作为startScene你的标准方法实现的Cocos2D CCAppDelegate子类：

```
-(CCScene*) startScene {
    return [[MyLevel2Layer layer] asCCScene] ;
}
```

注：本CCScene类不应该与混淆CC3Scene表示3D场景类。这两个类都在使用Cocos3D的应用程序，一个在上你的CC3Layer，低于一个，但非常不同的目的。该CCScene是cocos2d的组成部分，是一个简单的包装，让你CC3Layer由使用CCDirector。您定制的CC3Scene实例，在另一方面，包含了所有的3D内容。
除了 ​​有一个单一的CC3Layer，拿着一个CC3Scene包含所有3D内容，也可以实例化的若干CC3Layer/CC3Scene对，并将它们作为3D在2D，否则瓷砖的应用程序。例如，你可能有一些3D角色在创建一个原本2D或2.5D的环境中走动的Cocos2D。在这种情况下，每个自定义的CC3Layer情况下，在您的结构增加地方作为一个孩子的Cocos2D节点和2D环境中四处移动。每个自定义CC3Layer实例显示其包含的3D场景，在该位置CC3Layer进行定位。的CC3Demo3DTiles演示应用程序，包含在Cocos3D分布提供这种方法的一个例子。