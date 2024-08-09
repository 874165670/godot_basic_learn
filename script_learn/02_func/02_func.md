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

### 内置函数

- `_ready()`: 节点初始化完成时调用
- `_process(delta)`: 每帧调用
- `_physics_process(delta)`: 固定频率调用,用于物理模拟

```gdscript
func _ready():
    print("Node is ready!")

func _process(delta):
    rotate(delta)  # 每帧旋转物体
```

### 信号回调函数

```gdscript
signal health_changed(new_health)

func _on_health_changed(new_health):
    update_health_bar(new_health)

# 在适当的地方连接信号
health_changed.connect(_on_health_changed)
```

## 7. 高级特性和新功能

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

### 闭包

```gdscript
func create_multiplier(factor):
    return func(x): return x * factor

var double = create_multiplier(2)
print(double.call(5))  # 输出: 10
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

## 12. 小练习

### 1.编写一个名为 `greet` 的函数,接受一个用户名字参数,并打印出一句问候语，例如:

```gdscript
greet("John")  # 输出: Hello, John!
```

### 2.创建一个名为 `calculate_area` 的函数,接受长度和宽度两个参数,返回矩形的面积。

### 3.编写一个名为 `new_enemy` 的函数,接受敌人的名字和生命值这两个参数。生命值应该有一个默认值 100。

## 13. 总结和预告

- 回顾 func 关键字的关键点
- 强调函数在游戏开发中的重要性
- 预告下一期内容:关于类和对象

## 00. 备注<a name="ps"></a>

第 6 部分的内容由于涉及到了其他关键字（例如 func、class），所以暂时不要求掌握，如果完全听不懂可以直接跳过，等到我们讲完函数定义、类定义之后再回来温习一下，会更好理解作用域的概念。
