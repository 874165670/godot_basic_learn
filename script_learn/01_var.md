# GDScript 基础 - 变量声明和类型

大家好，欢迎来到 GDScript 教程系列的第一课。今天我们要学习的是 GDScript 中最基础也是最重要的概念之一：变量声明和类型。

## 1. 什么是变量?

变量是用来存储数据的容器。在编程中，我们使用变量来临时保存和操作数据。想象一下，变量就像是一个贴了标签的盒子，我们可以往里面放东西，也可以随时取出来使用。

## 2. 在 GDScript 中如何声明变量?

在 GDScript 中，我们使用`var`关键字来声明变量。基本语法如下：

```gdscript
var 变量名 = 值
```

例如：

```gdscript
var player_name = "Alice"
var player_health = 100
var is_game_over = false
```

## 3. 什么是数据类型?

在我们深入 GDScript 的具体类型之前，让我们先理解一下什么是数据类型，以及为什么它们如此重要。

想象一下，如果你正在整理一个仓库。你会把不同的物品放在不同的容器里，对吧?你可能会用盒子来存放书籍，用瓶子来装液体，用袋子来装衣服。在编程中，数据类型就像这些不同的容器。

数据类型告诉计算机：

### 1. 如何在内存中存储这个数据

### 2. 这个数据可以进行哪些操作

### 3. 这个数据占用多少存储空间

为什么数据类型重要?

- 效率： 合适的数据类型可以优化内存使用和运行速度。
- 安全性： 防止对数据进行错误的操作，如试图将文本"加入"到数字中。
- 可读性： 明确的类型让代码更容易理解。
- 功能性： 不同类型提供不同的操作和方法。

在 GDScript 中，虽然你不总是需要明确声明类型(这就是所谓的"动态类型")，但了解类型仍然非常重要。

## 4. GDScript 中的基本数据类型

现在让我们看看 GDScript 中最常用的数据类型：

a) 整数 (int)：

- 用于表示整数，如 1， 100， -50
- 例子： `var player_level = 5`
- 可以进行加减乘除等数学运算

b) 浮点数 (float)：

- 用于表示带小数点的数字，如 3.14， -0.01
- 例子： `var player_speed = 7.5`
- 适用于需要精确小数的场景，如物理计算

c) 字符串 (String)：

- 用于表示文本
- 例子： `var player_name = "Alice"`
- 可以进行拼接、切割、查找等文本操作

d) 布尔值 (bool)：

- 只有两个可能的值：true(真)或 false(假)
- 例子： `var is_game_over = false`
- 用于控制流程，如条件判断

e) 数组 (Array)：

- 用于存储多个值的列表
- 例子： `var inventory = ["sword"， "shield"， "potion"]`
- 可以添加、删除、遍历元素

f) 字典 (Dictionary)：

- 用于存储键值对
- 例子： `var character = {"name"： "Bob"， "class"： "Warrior"}`
- 这个例子中，键是字符串"name"，值是字符串"Bob"；键是字符串"class"，值是字符串"Warrior"
- 适合存储复杂的结构化数据

在游戏开发中，我们经常会混合使用这些类型。例如：

```gdscript
var player = {
    "name"： "Alice"，
    "level"： 5，
    "health"： 100.0，
    "is_alive"： true，
    "inventory"： ["sword"， "shield"]
}
```

这个例子中，我们使用了字典来存储玩家信息，其中包含了字符串、整数、浮点数、布尔值和数组。

理解这些类型如何工作，将帮助你更好地组织游戏数据，写出更清晰、更高效的代码。

## 5. 类型推断与显式类型声明

GDScript 是动态类型语言，这意味着你不必总是明确指定变量类型。引擎会自动推断类型：

```gdscript
var score = 0  # 自动推断为整数类型
var name = "Player"  # 自动推断为字符串类型
```

但有时我们也需要显式声明类型，特别是当我们想要确保变量只能存储特定类型的数据时：

```gdscript
var health： int = 100
var player_name： String = "Alice"
```

## 6. [变量作用域\*](#ps)

在 GDScript 中，变量的作用域取决于它们声明的位置，并且决定了在代码的哪些部分可以访问这个变量：

- 全局变量： 在脚本的最外层声明，整个脚本都可以访问
- 局部变量： 在函数内部声明，只在该函数内可用
- 类变量： 在类中声明，该类的所有实例都可以访问

理解作用域对于编写清晰、无错误的代码至关重要，让我们通过一个简单的游戏角色脚本来看看三种主要的作用域：

首先，创建一个名为 `GlobalSettings.gd` 的脚本，作为全局配置：

```gdscript
# GlobalSettings.gd
extends Node

var game_version = "1.0"
var difficulty_level = "Normal"

func get_game_info()：
    return "Version： " + game_version + "， Difficulty： " + difficulty_level
```

然后，创建一个 `Player.gd` 脚本：

```gdscript
# Player.gd
extends KinematicBody2D

# 类变量
var player_name = "Alice"
var health = 100
var score = 0

func _ready()：
    # 局部变量
    var welcome_message = "Welcome， " + player_name + "!"
    print(welcome_message)

    # 访问全局变量
    print("Game Info： " + GlobalSettings.get_game_info())

    # 使用类变量
    print("Initial Health： " + str(health))

    introduce_player()

func introduce_player()：
    # 使用类变量
    print(player_name + " is ready to play!")
    # print(welcome_message)  # 这行会报错，因为welcome_message是_ready函数的局部变量

func take_damage(amount)：
    # amount 是局部变量
    health -= amount
    print(player_name + " took " + str(amount) + " damage. Health： " + str(health))

    # 访问和修改全局变量
    if health < 30 and GlobalSettings.difficulty_level != "Easy"：
        GlobalSettings.difficulty_level = "Easy"
        print("Difficulty adjusted to Easy")

func gain_score(points)：
    # points 是局部变量
    score += points
    print(player_name + " gained " + str(points) + " points. Total score： " + str(score))

    # 访问全局变量
    if score > 1000 and GlobalSettings.difficulty_level != "Hard"：
        GlobalSettings.difficulty_level = "Hard"
        print("Difficulty increased to Hard")

func _process(delta)：
    # delta 是局部变量
    if Input.is_action_just_pressed("ui_select")：
        take_damage(10)
    if Input.is_action_just_pressed("ui_focus_next")：
        gain_score(100)
```

现在让我们一起分析一下在这个例子中，不同作用域中的变量：

1. 全局变量 (在 `GlobalSettings.gd` 中)：

   - `game_version` 和 `difficulty_level` 是全局变量
   - 它们可以在任何脚本中通过 `GlobalSettings` 访问
   - 用于存储整个游戏中都需要的信息

2. 类变量 (在 `Player.gd` 中)：

   - `player_name`， `health`， 和 `score` 是类变量
   - 它们在整个 `Player` 类中都可以访问
   - 用于存储特定于该玩家实例的信息

3. 局部变量：
   - 在函数内定义，如 `welcome_message`， `amount`， `points`， 和 `delta`
   - 只在定义它们的函数内可用
   - 用于临时计算和存储

## 7. 实际应用案例

让我们来看一个简单的游戏角色脚本，展示如何使用变量：

```gdscript
extends KinematicBody2D

var player_name： String = "Alice"
var health: int = 100
var speed: float = 200.0
var items: Array = ["sword"， "shield"]
var is_invincible: bool = false

func _ready():
    print("Player " + player_name + " is ready!")
    print("Initial health: " + str(health))
    print("Inventory: " + str(items))

func take_damage(amount: int):
    if not is_invincible:
        health -= amount
        print(player_name + " took " + str(amount) + " damage. Health: " + str(health))
        if health <= 0:
            game_over()

func game_over():
    print(player_name + " is defeated!")

func _process(delta):
    # 简单的移动逻辑
    var velocity = Vector2.ZERO
    if Input.is_action_pressed("move_right"):
        velocity.x += 1
    if Input.is_action_pressed("move_left"):
        velocity.x -= 1
    if Input.is_action_pressed("move_down"):
        velocity.y += 1
    if Input.is_action_pressed("move_up"):
        velocity.y -= 1

    velocity = velocity.normalized() * speed
    move_and_slide(velocity)
```

在这个例子中，我们声明了多种类型的变量来表示游戏角色的不同属性。我们使用这些变量来控制角色的行为，如移动速度、生命值变化等。

## 8. 小练习

尝试修改上面的脚本，添加新的变量来表示角色的其他属性，如魔法值、经验值等。然后编写一个函数来显示角色的所有属性。

## 9. 总结

今天我们学习了 GDScript 中的变量声明和基本类型。记住，合理使用变量可以让你的代码更加清晰、易读，并且更容易管理游戏状态。在接下来的课程中，我们将学习如何使用这些变量来创建更复杂的游戏逻辑。

下一课，我们将探讨条件语句，学习如何根据不同的情况让游戏做出不同的反应。感谢观看，我们下次再见!

## 00. 备注<a name="ps"></a>

第 6 部分的内容由于涉及到了其他关键字（例如 func、class），所以暂时不要求掌握，如果完全听不懂可以直接跳过，等到我们讲完函数定义、类定义之后再回来温习一下，会更好理解作用域的概念。
