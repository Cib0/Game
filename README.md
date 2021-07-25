# Game
  ##笔记 
1.坐标系
unity用的是左手坐标系，laya用的是右手坐标系。
unity的模型用laya工具导出来在laya中使用的时候，需要注意z轴坐标的值会变成unity中z轴坐标值的取反。并且getforaward的方向也是反的。

2.物理系统
  1）刚体组件
  如果通过直接对对象的transform.position和transform.eulerAngle赋值的方式改变其位置或旋转，
  或许不会出现你预期的和其他刚体交互应该出现的物理表现。而是应该调用刚体的addforce和axisAngleVelocity（轴角度速度）来施加力和改变轴角度速度。
  否则可能会出现模型突然陷进桌子，然后被快速吐出来。桌子在转动，模型原地不动等不符合现实中物理学效应的情况。
  2）碰撞盒组件
  unity中给一个新建的cube对象加个box碰撞盒组件，再加上一个刚体组件并应用重力。想在一个可改变自身旋转角度的方形盒子里模拟滑落的效果。导出并在laya中使用。有时候会因为cube滑到角落里，而被卡住。
  再怎么转动盒子就是纹丝不动。写到这的时候突然想到有可能是刚体休眠的原因。TODO：回头试验下。
  
需要注意应用了刚体组件的对象，不能放在一个会改变position或旋转值的对象的子节点下面。对于两个需要模拟它们物理效果的对象，它们不能为父子关系。

得出我做的项目的两个解决方案：
模型底座和桌子的节点都不能再为父子关系
1.应用重力
  ~~底座模型刚体的碰撞盒改用box，~~*还是用原来的球体碰撞盒，方便一点*。桌子会一直绕自身Y轴自旋。给桌子加刚体，再通过给刚体的轴角度速度赋值来转动桌子，用摩擦力带动桌子上模型底座刚体。模型的旋转根据桌子的旋转角度赋值。其他限制桌子上模型底座的拥有碰撞盒组件的对象放在这个有刚体组件的桌子对象下。
2.不应用重力
  底座同样用球体碰撞盒。桌子同样加刚体，通过给刚体轴角速度赋值来转动桌子。模型的旋转同样根据桌子的旋转角度赋值。然后根据桌子的角度，算出给刚体加的力的方向，给刚体加力。对限制底座的碰撞盒对象的处理同上。
