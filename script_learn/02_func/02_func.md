# GDScript 基础 - func 关键字

## 1. 引言

- 简单回顾上一期关于变量声明和类型的内容
- 本期主题:func 关键字

## 2. func 关键字基础

### 什么是函数?

通俗地讲，函数是一段可重用的代码块,用于执行特定任务。它可以接收输入(参数),执行操作,并可能返回结果。当你有一个需要多次执行相同的操作时（例如游戏中的生命值增减、伤害计算等），可以考虑使用函数将这一个过程封装起来。

### 为什么需要函数?

- 代码复用
- 模块化编程
- 提高可读性和可维护性

### func 的基本语法

```gdscript
func function_name(parameters):
    # function body
    pass
```

### 示例:创建和调用简单函数

```gdscript
func greet(name):
    print("Hello, " + name + "!")

# 调用函数
greet("Alice")  # 输出: Hello, Alice!
```

## 3. 函数参数和返回值

### 参数的定义和使用

```gdscript
func add(a, b):
    return a + b
```

### 默认参数值

```gdscript
func greet(name = "Guest"):
    print("Hello, " + name + "!")

greet()  # 输出: Hello, Guest!
greet("Alice")  # 输出: Hello, Alice!
```

### 返回值的使用

```gdscript
func calculate_area(width, height):
    return width * height

var area = calculate_area(5, 3)
print("Area: ", area)  # 输出: Area: 15
```

### 示例:计算器函数

```gdscript
func calculator(a, b, operation = "add"):
    match operation:
        "add":
            return a + b
        "subtract":
            return a - b
        "multiply":
            return a * b
        "divide":
            if b != 0:
                return a / b
            else:
                print("Error: Division by zero")
                return null

print(calculator(10, 5, "add"))  # 输出: 15
print(calculator(10, 5, "multiply"))  # 输出: 50
```

## 4. [函数重载\*](#ps)

GDScript 不支持传统的函数重载,但可以通过默认参数和可选参数来模拟:

```gdscript
func process_data(data, option1 = null, option2 = null):
    if option1 == null and option2 == null:
        # 处理只有data的情况
        pass
    elif option2 == null:
        # 处理有data和option1的情况
        pass
    else:
        # 处理所有参数都有的情况
        pass
```

## 5. 静态类型和类型提示

### 参数类型提示

```gdscript
func set_health(value: int):
    health = value
```

### 返回值类型提示

```gdscript
func get_player_name() -> String:
    return player_name
```

### 示例:使用类型提示的函数

```gdscript
func calculate_damage(base_damage: float, critical_multiplier: float = 1.0) -> int:
    return int(base_damage * critical_multiplier)
```

## 6. 特殊函数

### 常见内置函数

- `_ready()`: 节点初始化完成时调用
- `_process(delta)`: 每帧调用
- `_physics_process(delta)`: 固定频率调用,一般用于物理模拟较多

```gdscript
func _ready():
    print("Node is ready!")

func _process(delta):
    rotate(delta)  # 每帧旋转物体
```

```
相关拓展1.：_process(delta)和_physics_process(delta)有什么区别：

        1._process(delta)每帧都会执行,帧率可能不稳定。
        2._physics_process(delta)以固定间隔执行(该间隔一般在你的项目中设置，默认一般是60帧每秒),更适合物理计算。

相关拓展2.：_process(delta)和_physics_process(delta)中的delta参数是什么：

        delta参数值是当前帧与上一帧的时间差,单位为秒。这是一个系统默认的参数,不需要给他赋值,只需要在使用这两个函数时不要忘了它就行。
```

### [信号回调函数\*](#ps)

```gdscript
signal health_changed(new_health)

func _on_health_changed(new_health):
    update_health_bar(new_health)

# 在适当的地方连接信号
health_changed.connect(_on_health_changed)
```

## 7. [高级特性与新功能\*](#ps)

### 匿名函数(lambda)

```gdscript
var multiply = func(a, b): return a * b
print(multiply.call(4, 5))  # 输出: 20
```

### 函数作为参数传递

```gdscript
func apply_operation(a, b, operation):
    return operation.call(a, b)

var result = apply_operation(5, 3, func(x, y): return x + y)
print(result)  # 输出: 8
```

### Godot 4.0 新特性:函数指针

```gdscript
var fn_pointer = Callable(self, "function_name")
fn_pointer.call()  # 调用function_name函数
```

## 8. 最佳实践和性能考虑

### 函数命名规范

- 使用小写字母和下划线
- 动词开头,描述函数功能

### 避免过长函数

- 保持函数简短,聚焦于单一任务
- 超过 20-30 行考虑拆分

### 性能优化技巧

- 避免在循环中频繁调用函数
- 使用静态类型提高性能
- 考虑使用内联函数进行关键性能优化

## 9. func 关键字的历史变迁

### Godot 2.x

- 基本的 func 定义
- 无类型提示

### Godot 3.x

- 引入类型提示
- 改进的语法检查

### Godot 4.0

- 引入函数指针(Callable)
- 改进的 lambda 支持
- 更强大的类型系统

## 10. 实践示例

创建一个简单的角色控制脚本,展示不同类型的函数使用:

```gdscript
extends KinematicBody2D

signal health_changed(new_health)

var health: int = 100
var speed: float = 200.0

func _ready():
    print("Player is ready!")

func _process(delta):
    move(delta)

func _physics_process(delta):
    handle_collision()

func move(delta: float) -> void:
    var velocity = Vector2.ZERO
    if Input.is_action_pressed("ui_right"):
        velocity.x += 1
    if Input.is_action_pressed("ui_left"):
        velocity.x -= 1
    if Input.is_action_pressed("ui_down"):
        velocity.y += 1
    if Input.is_action_pressed("ui_up"):
        velocity.y -= 1
    velocity = velocity.normalized() * speed
    move_and_slide(velocity)

func handle_collision() -> void:
    for i in get_slide_count():
        var collision = get_slide_collision(i)
        if collision.collider.is_in_group("enemies"):
            take_damage(10)

func take_damage(amount: int) -> void:
    health -= amount
    health_changed.emit(health)
    if health <= 0:
        die()

func die() -> void:
    print("Game Over")
    queue_free()

func _on_health_changed(new_health: int) -> void:
    print("Health changed to: ", new_health)

# 使用匿名函数
var heal = func(amount: int):
    health = min(health + amount, 100)
    health_changed.emit(health)

# 函数作为参数
func apply_powerup(powerup_func: Callable) -> void:
    powerup_func.call()

func _on_powerup_collected():
    apply_powerup(func(): speed *= 1.5)
```

## 11. 常见陷阱和问题解答

### 递归函数注意事项

- 确保有基本情况避免无限递归
- 注意栈溢出风险

### 变量作用域问题

- 注意局部变量和全局变量的区别
- 使用 self 关键字访问类变量

### self 关键字的使用

- 在方法内部引用当前实例的属性和方法时使用
- 解决命名冲突

### GDscript 中的闭包是不完整的

```gdscript
func create_multiplier(factor):
    return func(x): return x * factor

var double = create_multiplier(2)
print(double.call(5))  # 输出: 10

func other_multiplier():
    var i = 0
    return func(): i += 1 return i
var result = other_multiplier()
print(result.call())  # 输出: 1
print(result.call())  # 输出: 1
print(result.call())  # 输出: 1
```

```
相关拓展：闭包是什么：
        闭包是一个函数和其周围状态（词法环境）的组合。换句话说，闭包允许一个内部函数访问其外部函数的作用域，并允许创造一个私有的空间。在上面的这个例子create_multiplier(factor)中，闭包捕获到了传入的数字2，这样在之后每一次调用它时，都会使用2来执行函数，而且此后在调用时也不需要再传入给他2了，相较于传统的函数，闭包更灵活。与此同时，一般的闭包都提供了一种方式来创建持久的私有状态，例如上面的例子中的other_multiplier()。在其他语言中（例如JavaScript），这个count的状态在闭包的整个生命周期内都存在，但仍然对外部不可直接访问，只能通过调用这个函数来让这个值不断加1。
        但是在GDscript中，并不会一直加1，而是一直输出1。
        在绝大多数编程语言中，都支持闭包，闭包是函数式编程的重要语法结构，但是在GDscript中，闭包是不完整的，只具有部分的特性，值得注意这一点。
```

## 12. 小练习

### 1.编写一个名为 `greet` 的函数,接受一个用户名字参数,并打印出一句问候语，例如:

```gdscript
greet("John")  # 输出: Hello, John!
```

### 2.创建一个名为 `calculate_area` 的函数,接受长度和宽度两个参数,返回矩形的面积。

### 3.编写一个名为 `new_enemy` 的函数,接受敌人的名字和生命值这两个参数。生命值应该有一个默认值 100。

## 13. 总结

Godot 引擎中的 func 关键字是定义函数的核心元素，它不仅仅是一个简单的语法构造，而是贯穿整个游戏开发过程的重要工具。从基本的自定义函数到内置的特殊函数（如\_ready()和\_process()），从信号处理到协程实现，func 在 Godot 中扮演着多样化的角色。它是实现游戏逻辑、处理用户输入、管理对象生命周期的关键。通过合理使用函数，开发者可以组织代码、提高可读性、实现模块化设计，并充分利用 Godot 的面向对象特性。掌握 func 的高级特性，如静态函数、虚函数重写和匿名函数，能够显著提升代码的灵活性和效率。同时，了解函数在性能优化、调试过程中的应用，以及在大型项目中的最佳实践，对于创建高质量的游戏项目至关重要。总的来说，func 不仅是一个语言特性，更是连接 Godot 各个方面的纽带，精通它将极大地提升开发效率和代码质量。
但是对于初学者来说，你拿到一个 func 时，首先需要关注入参以及返回值，也就是函数的（）中的内容以及 return 后面的内容，这将帮助你正确地调用函数；而当你自己去写函数时，你则需要思考一下你这个函数要实现的目的是什么？拥有最基础的思路，可以帮助你在第一次写代码时不至于手足无措。

## 00. 备注<a name="ps"></a>

第 4/6/7/10 部分的内容由于涉及到了其他关键字（例如 if、self），所以暂时不要求掌握，如果完全听不懂可以直接跳过，等到我们讲完其关联的定义、类定义之后再回来温习一下，会更好理解这些更深入的概念，本节对于新手来说，只要了解 func 关键字的几种基本使用方式即可。
