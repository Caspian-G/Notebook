#### 控制台输出

```
Debug.log() 
```

**注** game运行时可调节Inspector中序列化参数 但在game结束后不会保存

**注** 序列化参数优先级大于脚本初始化 修改脚本参数不会改变序列化中参数 需手动修改序列化参数

**注** 如果脚本无需start或update方法 则去掉 不去掉它们将会一直空跑占用cpu资源

**注** 按住Ctrl再拖拽物体会以一个特定值移动(该值可调节)

#### 移动

##### 修改transform对象(三个属性Position Rotation Scale)

```
void start(){
	transform.position = new Vector3(1, 2.5f, 3);  //初始化
}
transform.Translate(1.5f, 0, 0);  //position属性中 x-右 y-上 z-前 此处transform默认使用脚本绑定的对象坐标系而非世界坐标系
transfrom.Translate(1.5f, 0, 0, Space.World);  //世界坐标轴
transform.position += new Vector(1.5f, 0, 0);  //同上世界坐标轴
```

**注** 仅写1.5f会导致不同帧率下移动速度不同 使用1.5f*Time.deltaTime可保证速度同步(Time.deltaTime数值上等于帧率分之一)

#### 输入

```
float h = Input.GetAxis("horizontal");
float v = Input.GetAxis("vertical");
```

#### 碰撞检测

##### 触发器

```
```

