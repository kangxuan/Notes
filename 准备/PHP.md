# 基础

1. php中传值和传引用的区别

传值：通过函数参数传递值，在函数内部修改值，函数外部不生效

传引用：通过函数传递引用，是引用地址，在函数内部修改值会影响到函数外部

2. 判断为false的情况

整型0，浮点0.0，布尔值false，空字符串''和""，字符串'0'，空数组[]，NULL。

3. 超全局数组

```php
$GLOBALS
$_GET
$_POST
$_REQUEST
$_COOKIE
$_SESSION
$_SERVER
$_FILES
$_ENV
```

4. NULL的三种情况

变量设置为NULL，未定义的变量，unset变量

5. 常量的定义方法

```php
# 1. const定义
const a = 1;
# 2. define("", xxx)，函数定义
define("a", 1);
```

6. 抽象类和接口

```
抽象类：
1. 通过在class前面添加abstract关键字，且必须要有抽象方法。
2. 抽象类不能被直接实例化，必须通过子类继承抽象类并实现所有抽象方法，使得抽象具体化。
3. 抽象方法可以是private、protected、public，子类的权限必须比抽象方法宽松或一样。
接口：
1. 通过interface定义接口，只定义未实现的方法，不能定义其他变量。
2. 接口更不能直接实例化，必须通过implements实现其所有的方法。
3. 接口方法只能是public。
区别：
1. 抽象类使用abstract class，接口使用interface
2. 抽象类可以定义变量，接口不能
3. 抽象类可以定义构造函数，接口不能
4. 抽象类继承使用extends，接口通过implements实现
5. 接口方法只能是public修饰
6. 一个类可以继承多个接口，但只能继承一个抽象类
```

7. 运算符优先级

```
递增/递减 > ! > 算术运算符 > 大小比较 > （不）相等比较 > 引用 > 位运算符(^) > 
位运算符(|) > 逻辑与 > 逻辑或 > 三目 > 赋值 > and > oxr > or
```

8. 浮点数计算精度丢失问题

```
1. 使用BC MATCH拓展处理
2. 使用Decimal类处理（php版本>=7.4）
```

9. IP处理函数

```php
# 将ip转换成长整型
ip2long()
# 将长整型转换成ip
long2ip()
```

10. 打印处理

```php
# 单个变量打印
print()
# 格式化打印
printf("%d", $f)
# 格式化打印
print_r()
# 输出多个变量
echo()
# 按格式返回
sprintf()
# 格式化输出，并输出变量类型
var_dump()
# 将格式化输出，加 true 可返回，返回内容可直接做变量使用
var_export()
```

11. 序列化

```php
# 序列化
serialize()
# 反序列化
unserialize()
```

12. 字符串处理函数

```php
# 返回字符串的长度
strlen()
# 返回第一个字符串出现的位置
strpos()
# 返回字符串的子串
substr()
# 循环替换字符串
str_replace()
# 字符串转为小写
strtolower()
# 字符串转为大写
strtoupper()
# 去除字符串两端的空格或指定字符
trim()
# 分割字符串返回数据
explode()
# 将数组连接成字符串
implode()
# 反转字符串
strrev()
# 对字符串进行URL编码
urlencode()
# 对URL编码进行解码
urldecode()
# 转换成JSON字符串
json_encode()
# 解析JSON字符串
json_decode()
```

13. 数组处理

```php
# 返回数据的个数
count()
# 将一个或多个数组添加到数组的末尾
array_push()
# 删除数组中的最后一个元素
array_pop()
# 删除数组中的第一个元素
array_shift()
# 在数组开头插入一个或多个单元
array_unshift()
# 返回数组的（部分）key并返回数组
array_keys()
# 返回数组的所有值
array_values()
# 计算数组的差集
array_diff()
# 合并数组（求并集）
array_megre()
# 对数组升降序排序
sort(), rsort()
# 对数组键值升降序排序
asort(),arsort()
# 对数组键名升降序排序
ksort()，krsort()
# 交换数组中的key和value
array_flip()
# 返回一个反转顺序的数组
array_reverse()
# 去重函数
array_unique()
# 搜索值返回key
array_search()
# 检查值是否在数组中
in_array()
# 将回调函数作用到给定数组的每个元素上，并返回一个新数组
array_map()
# 使用回调函数过滤数组中的元素，返回一个新数组
array_filter()
# 通过回调函数迭代地将数组简化为单一的值
array_reduce()
# 对数组中的每个元素应用用户自定义函数
array_walk()
```

14. 文件操作

```php
# 打开文件,
# r/r+只读/读写打开，指针在开头
# w/w+只写/读写打开，文件存在会清空，不存在会创建
# a/a+写入追加写入/读写追加写入，指针在末尾
# x/x+写入/读写打开，指针在开头，文件存在返回false，不存在会创建
# b二进制打开
fopen()

# 写入,fputs是fwrite的别名
fwrite()/fputs()

# 读取指定长度
fread()

# 读取一行
fgets()

# 读取一个字符
fgetc()

# 关闭一个已打开的文件指针
fclose()

# 获取文件大小
filesize()

# 文件复制
copy()

# 文件删除
unlink()

# 获取文件类型
filetype()

# 移动或重命名
rename()

# 文件属性
file_exist() // 文件是否存在
is_readable() // 是否可读
is_writable() // 是否可写
is_executable() // 是否可执行
filectime() // 文件创建时间
fileatime() // 访问时间
filemtime() // 更新时间

# 打开并读取数据
file_get_contents()
# 打开并写入数据
file_put_contents()

# 循环读取文件内容
$f = fopen("ex.txt", "r")
while(!feof($f)) {
    // 读取一行
    $line = fgets($f);
    echo $line . "<br>";
}
fclose($f)

$lines = file("ex.txt", FILE_IGNORE_NEW_LINE)
forreach($lines as $line) {
    echo $line . "<br>"
}
```

15. 目录操作

```php
# 文件基础名称
basename()
# 文件夹名称
dirname()
# 文件信息数组
pathinfo()
# 打开目录
opendir()
# 读取目录
readdir()
# 关闭目录
closedir()
# 删除目录
rmdir()
# 目录创建
mkdir()
# 重命名或移动
rename()

# 循环读取文件
```

16. PHP7新增的新特性

```
1. 标量声明（declare定义为严格模式）
2. 返回值类型声明（declare定义为严格模式）
3. NULL合并运算符（??）
4. 组合比较符（<=>，当两者小于、等于、大于返回-1,0,1）
5. 通过define定义常量数组
6. 通过new class来实例化一个匿名类（用后即焚）
7. use批量声明
```

17. 二维数组中有两列，通过第一列升序，通过第二列降序排序

```php
$user_list = [
          ['name' => '张三', 'age' => 28, 'salary' => 100],
          ['name' => '赵六', 'age' => 21, 'salary' => 105],
          ['name' => '王五', 'age' => 20, 'salary' => 103],
          ['name' => '李四', 'age' => 21, 'salary' => 104]
        ];
array_multisort(
    array_column($user_list, 'age'), SORT_ASC,
    array_column($user_list, 'salary'), SORT_DESC,
    $user_list);
var_dump($user_list);
```

18. 不使用中间变量交换两值

```php
// 数字
$a = 1;
$b = 2;
$a = $a + $b;
$b = $a - $b;
$a = $a - $b;
// 字符串
$a = "123"
$b = "456"

$a .= $b;
$b = str_replace($b, "", $a);
$a = str_replace($b, "", $a);
```

19. `array_map`、`array_reduce`、`array_walk`、`array_fliter`的区别

```
array_reduce($arr, callback):将数据按照callback依次迭代将数组简化为单一的值。
array_map(callback, $arr, $arr1):为一个或多个数组的每个元素应用回调函数callback。
array_walk($arr, callback):使用自定义函数遍历每个元素。
array_filter($arr, callback):使用回调函数过滤数组
```

20. 读取指定文件夹下的所有文件，创建文件夹

```php
// 读取指定文件夹下的所有文件
function dirList(string $dir) : array
{
    $files = [];
    // 浏览
    $dirs = scandir($dir);
    foreach($dirs as $d) {
        if (is_dir("$dir/$d" && $d != "." && $d != "..") {
            dirList("$dir/$d");
        } else if ($d != "." && $d != "..") {
            $files[] = $d;
        }
    }

    return $files;
}
// 循环创建文件夹
function directory($dir)
{
    return is_dir($dir) || (directory(dirname($dir)) && mkdir($dir, 0777))
}
```

21. 下面输出什么

```php
$a = "1c" + 1; $a = 2;
$b = "c1" + 1; $b = 1;
```

22. PHP中有哪些魔术方法

```php
# 构造函数-当一个对象被创建时自动调用
__construct()
# 析构函数-当一个对象被销毁时自动调用
__destruct()
# 当读取一个不可访问的属性时自动调用
__get($property)
# 当给一个不可访问属性赋值时自动调用
__set($property, $value)
# 当调用isset()或empty()发现是一个不可访问属性时自动调用
__isset($property)
# 当删除一个不可访问属性时自动调用
__unset($property)
# 当调用一个不可访问方法时自动调用
__call($method, $arguments)
# 当调用一个不可访问静态方法时自动调用
__callStatic($method, $arguments)
# 当对象被当做字符串时自动调用
__toString()
# 当对象被当做函数调用时自动调用
__invoke()
# 当对象被克隆时自动调用
__clone()
# 当对象被传递给var_dump()函数时被调用
__debugInfo()
# 当对象被序列化之前自动调用
__sleep()
# 当对象被反序列化之后自动调用
__wakeup()
# 当调用var_export()导出类时自动调用
__set_state($properties)
# 当对象被序列化时自动调用
__serialize()
# 当对象被反序列化时自动调用
__unserialize($data)
```

23. 如何实现链式操作

```php
// 定义链式操作类
class Calc {
    private $res;

    public function __construct($in = 0)
    {
        $this->res = $in;
    }

    public function add($val)
    {
        $this->res += $val;
        return $this;
    }

    public function sub($val)
    {
        $this->res -= $val;
        return $this;
    }

    public function getRes()
    {
        return $this->res;
    }
}

// 调用
$calc = new Cacl(1);
echo $calc->add(2)->sub(1)->getRes();
// 结果输出
2
```

24. TCP和UDP这些协议有什么区别？

```
TCP（Transmission Control Protocol）和UDP（User Datagram Protocol）是两种最
常用的传输层协议，它们在网络通信中扮演着不同的角色，具有以下主要区别：

连接性：
1.TCP是一种面向连接的协议，它在数据传输之前会建立连接，然后确保数据可靠地到达目标，然后
再关闭连接。
2. UDP是一种无连接的协议，它不会建立连接，而是直接发送数据包，每个数据包都是独立的，没
有顺序，不保证可靠性。

可靠性：
1. TCP提供可靠的数据传输，它通过序列号、确认和重传机制来确保数据的可靠性，以及按顺序
传输数据包。
2. UDP不提供可靠性保证，它只是简单地发送数据包，不进行确认或重传，因此可能会丢失数据包
，或者数据包的顺序可能被打乱。

延迟和效率：
1. TCP通常比UDP慢，因为TCP需要进行连接的建立和维护、确认和重传等额外的操作，这些操作
会增加延迟。
2. UDP通常比TCP快，因为它没有连接建立和维护的开销，以及额外的确认和重传操作，但它的速
度是以牺牲可靠性为代价的。

应用场景：
1. TCP适用于对可靠性要求较高的应用，如文件传输、电子邮件、网页浏览等。
2. UDP适用于对延迟要求较高、对可靠性要求不那么严格的应用，如实时音频/视频传输、在线游戏
等。

请求方式：

TCP：
talnet/curl/nc/socat
telnet 主机名或IP地址 80
curl http://example.com/file.txt
nc 主机名或IP地址 80
socat - TCP:主机名或IP地址:80

UDP
nc/socat
nc -u 主机名或IP地址 端口号
socat - UDP:主机名或IP地址:端口号
```

25. composer的常见用法

```shell
# composer初始化项目，并添加一个composer.json文件
composer init
# 添加依赖
composer require monolog/monolog
# 安装依赖
composer install 
# 更新依赖
composer update vendor/package-name
# 查看已安装的依赖
composer show
# 发布自己的依赖
composer publish
```

26. cookie和session有什么区别

```
1. 存储位置：cookie存储在浏览器端，session存储在服务端
2. 安全性：cookie不安全，session安全
3. 容量限制：cookie大小受限（4K），session理论上没有限制。
4. 生命周期：cookie一直存储在浏览器端（过期前），即使关闭了浏览器；session默认情况下
浏览器关闭后被销毁
5. 跨页面传递：cookie在同一域名下传递，session不受限。

cookie在客户端用于用户状态和个性设置，而session主要在服务端存储用户状态和敏感信息。
```

27. 几种排序算法

```php
# 交换类-冒泡排序-时间复杂度是O(n^2)
# 第一次循环
3 2 4 6 1 7
2 3 4 6 1 7
2 3 4 6 1 7
2 3 4 6 1 7
2 3 4 1 6 7
2 3 4 1 6 7
function bubbleSort($arr) {
    $count = count($arr);
    for ($i = 0; $i < $count-1; $i++) {
        for($j = 0; $j < $count - $i - 1; $j++) {
            if ($arr[$j] > $arr[$j+1]) {
                $temp = $arr[$j];
                $arr[$j] = $arr[$j+1];
                $arr[$j+1] = $temp;
            }
        }
    }
}
# 交换排序-快速排序
```

# 中级

1. PHP 7 的内存回收原理？

```
PHP变量的内存管理采用的是引用计数机制，当变量赋值，传递时并不会直接进行值拷贝，而是增加
值的引用数，unset、return等操作时将减掉引用数，如果refcount变为0时则直接释放，这就是
gc的内存回收的基础原理
```

2. PHP7中的垃圾回收机制？

```
垃圾的产生：正常情况下的内存回收机制当refcount变为0时就会被直接回收，但有一种情况
就是当循环引用时，比如数组$a中的一个元素是$a本身的引用，此时refcount会变成2，此时
unset($a)，但unset只会将refcount减1，此时还是1得不到释放，无法正常回收。这种循环
引用也只会发生在array和object类型上。
垃圾回收过程：对于这样的垃圾不会立即回收，而是将其放入到一个缓冲区中，等待到达10000个值
时再统一处理，垃圾缓冲区是一个双向链表，遍历所有的value并减1，如果value变为0则说明是
循环引用产生的垃圾立即回收，如果不是则重新+1从垃圾缓冲区移除
```

3. zendMM详解原理

```
zendMM是PHPzend引擎的内存管理系统，主要负责分配释放PHP运行中的内存。zendMM主要负责：
1. 内存分配：zendMM使用不同的的内存分配器来管理不同大小的内存块，小的用自己的，大的使用
内存分配函数。
2. 内存池：zendMM维护一个内存池，先预分配一个大的内存区域，然后根据需要分成小的内存块，
减少动态内存分配和释放的频率，提高内存管理效率。
3. 引用计数：
4. 垃圾回收：
5. 内存合并：当某一块内存被释放，zendMM会将相邻的内存块进行合并，减少内存碎片。
6. 内存池销毁：当PHP执行完毕后会销毁内存池。
```

4. PHP哪些变量类型在栈，哪些变量类型在堆？

```
分配原理：往往将较小以及生命周期短的放在栈上，较大以及生命周期长的放在堆上。
栈上分配：
1. 标量类型：包括int、float、char（较小的字符串）、bool等
2. 资源：由于资源是对外部资源（如文件句柄、数据库连接等）的引用，PHP 
会在栈上分配资源变量本身，但资源所指向的数据通常存储在堆上。
堆上分配：
1. 数组
2. 对象
3. string（较大的字符串）
```

5. PHP变量在栈会有什么优势？

```
访问快：因为栈是有序的数据结构
自动内存管理：栈上的内存分配和释放是自动的。当一个函数结束时，它的局部变量（存储在栈上）
会自动被销毁，无需程序员显式释放内存。这降低了内存泄漏的风险，因为变量的生命周期受限于
其所在的作用域。
```

6. 详细描述 PHP 中 HashMap 的结构是如何实现的？

```
PHP中并没有hashmap的数据结构，而是通过关联数组来实现类似hashmap数据结构，允许键为
字符串或数字。
如何实现：
1. 哈希表：关联数组的底层是通过哈希表实现，哈希表效率高，通过哈希函数将键映射到存储桶
2. 哈希冲突：既然是哈希表就会有哈希冲突，关联数组通过链表地址法来解决哈希冲突，即使
键通过哈希函数得到的值是一样的，也会追加到链表上。
3. 动态调整大小：关联数组中的哈希表会根据负载因子动态调整哈希表的大小，当负载因子超过
阈值时会自动拓展。
```

7. 下面代码中，在 PHP 7 下， `$a` 和 `$b`、`$c`、`$d` 分别指向什么 zVal 结构？
   `$d` 被修改的时候，PHP 7 / PHP 5 的内部分别会有哪些操作？

```
当进行 $b = &$a; 操作时，$b 和 $a 指向同一个 zVal 结构，表示同一个字符串 'string'。

随后，$c = &$b; 操作使得 $c 也指向相同的 zVal 结构，即字符串 'string'。

最后，$d = $b; 操作将 $b 的值（即字符串 'string'）复制给了 $d。此时，$d 也指向了字符串 'string' 的 zVal 结构。

最后一行代码 $d = 'to'; 进行了修改操作，这时 PHP 7 和 PHP 5 内部的操作略有不同。

在 PHP 7 中，由于 $d 已经是一个普通变量，而不是引用，这个赋值操作会创建一个新的 zVal 结构，
将字符串 'to' 的值赋给 $d。现在，$d 指向了新的字符串 'to' 的 zVal 结构，而不再与 $a、
$b、$c 共享相同的 zVal 结构。

在 PHP 5 中，$d 是一个普通变量，但在赋值时，它会共享与 $a、$b、$c 相同的字符串 
'string' 的 zVal 结构。因此，修改 $d 的值也会影响到 $a、$b、$c。
```

8. strtr 和 str_replace 有什么区别，两者分别用在什么场景下？

```
strtr:可以传递3个参数，也可以传递2个参数，三个参数时from中每个字符都会替换成to的对应字符
from[n]都会替换成to[n],如果不同长度会忽略长的那一段，如果是两个参数，第二个参数必须是
一个数组，优先从长的开始匹配，一次性匹配(匹配一个后不再进行匹配）。
str_replace:传递4个参数，第四个参数是被替换的数量。由于 str_replace() 的替换时从
左到右依次进行的，进行多重替换的时候可能会替换掉之前插入的值，循环匹配。
```

```php
$trans = array("h" => "-", "hello" => "hi", "hi" => "hello");
echo strtr("hi all, I said hello h h hi", $trans); //hello all, I said hi - - hello
echo "\n";
$search = array("h", "hello", "hi");
$tag = array("-", "hi", "hello");
echo str_replace($search, $tag, "hi all, I said hello h h hi"); //-i all, I said -ello - - -i
echo "\n";
$letters = array('a', 'p');
$fruit   = array('apple', 'pear'); // apearpearle pear
$text    = 'a p';
$output  = str_replace($letters, $fruit, $text); //apearpearle pear
echo $output;
```

9. ”PHP 的字符串是二进制安全的“如何理解这句话？

```
PHP的字符串可以包含任意二进制数据，包括NULL等在其他语言中视为特殊字符，
为什么是二进制安全:
1. PHP不会解释数据，所以PHP字符串可以存储任何二进制
2. 不以NULL结尾，
```

10. 字符串连接符.，在 PHP 内核中有哪些操作？

```
1. 先创建一个新的字符串变量用于存储连接后的字符串
2. 处理不同的编码转换，如果一个是整型需要先转换成字符串类型
```

11. 多次连接，是否会造成内存碎片过多？

```
在 PHP 中，多次字符串连接可能会导致内存碎片的产生，尤其是对于大量临时字符串的拼接操作。
这是因为字符串连接操作的特性，每次连接都可能涉及到新的内存分配和释放。
如何解决：
1. 使用implode函数
2. 使用sprintf函数格式化
```

12. PHP 中创建多进程有哪些方式？

```
1. fork函数
$pid = pcntl_fork();

if ($pid == -1) {
    // 失败处理
} elseif ($pid) {
    // 父进程逻辑
} else {
    // 子进程逻辑
}
2. pcntl_exec() 函数：可以在当前多进程中调用外部程序，以此来替代当前进程的执行。
$pid = pcntl_fork();

if ($pid == -1) {
    // 失败处理
} elseif ($pid) {
    // 父进程逻辑
} else {
    // 子进程逻辑
    pcntl_exec('/path/to/other/script', $args);
}
```

13. 用 PHP 实现一个定时任务器，类似 crontab，需要做到前一个任务不论运行时长、运行失败，都不能影响下一个任务的准点执行？

```php
<?php
date_default_timezone_set('Asia/Shanghai');

// 定义任务列表
$tasks = array(
    array('name' => 'Task1', 'schedule' => '*/5 * * * *', 'command' => 'php task1.php'),
    array('name' => 'Task2', 'schedule' => '0 */2 * * *', 'command' => 'php task2.php'),
    // 添加更多任务...
);

// 主循环
while (true) {
    foreach ($tasks as $task) {
        if (shouldRunTask($task['schedule'])) {
            runTaskInBackground($task['command']);
        }
    }

    sleep(60); // 休眠1分钟，避免过于频繁检查任务

    // 可添加日志、监控等功能
}

// 检查任务是否应该执行
function shouldRunTask($schedule)
{
    $now = new DateTime();
    $nextRun = (new CronExpression($schedule))->getNextRunDate()->getTimestamp();

    return $now->getTimestamp() >= $nextRun;
}

// 在后台运行任务
function runTaskInBackground($command)
{
    $pid = pcntl_fork();

    if ($pid == -1) {
        // 失败处理
        exit(1);
    } elseif ($pid) {
        // 在父进程中
        pcntl_wait($status); // 等待子进程结束
    } else {
        // 在子进程中
        exec($command);
        exit(0);
    }
}
```

14. 实现如下函数(PHP 7) echo a(1, 3); //4
    echo a(3)(5); //8
    echo a(12)(35)(6); //21

```php
function a(...$args) {
    if (count($args) >= 2) {
        // 如果参数个数大于等于2，直接返回结果
        return array_sum($args);
    } else {
        // 如果参数个数小于2，返回一个匿名函数
        return function (...$newArgs) use ($args) {
            // 合并之前的参数和新参数
            $mergedArgs = array_merge($args, $newArgs);
            // 递归调用自身
            return a(...$mergedArgs);
        };
    }
}

// 示例使用
echo a(1, 3);        // 输出：4
echo a(3)(5);        // 输出：8
echo a(1, 2)(3, 4, 5)(6);  // 输出：21
```

15. PHP7底层优化

```
1. PHP7将zval的数据结构调整了，占用的字节数减少了
2. PHP7通过参数传输优化了函数调用
3. PHP7优化了数组的底层存储结构，由hashtable+linkedlist优化成了zval array，实际上
就是将hashtable和linkedlist分配到同一块内存空间，提高查询效率。
```

16. PHP-FPM与nginx的通信机制

```
名词解释：
CGI:是web server和web app之间的数据交换协议，就是规定要传递什么数据，以什么样的格式
传递给后方处理这个请求的协议。
FastCGI:是用于提高CGI性能的协议，他允许web server和web app（PHP解释器）保持长连接
而不是每一个请求创建一个进程。
PHP-FPM：是PHP（web app）对web server提供的FastCGI协议的进程管理器。
FastCGI与PHP-FPM的关系：FastCGI是一种数据交换的协议，PHP-FPM是实现FastCGI协议的
进程管理器。

通信机制：PHP-FPM和nginx的通信机制也就是FastCGI协议，FastCGI是改良版本的CGI协议，
允许nginx和php-fpm保持长连接。但在FastCGI的基础上分为两种不同的通信方式：
1. Unix Socket：通常在同一台服务器上部署时使用unix socket
2. tcp/ip socket: 通常不在同一台服务器部署时使用tcp/ip
```

17. PHP7数组如何解决hash冲突的？

```
PHP7使用哈希表来存储数组元素。为了解决哈希冲突的问题，PHP7内部使用了链式哈希算法。
当桶中有多个元素时，它们会形成一个链表。如果需要查找或操作元素，则需要遍历整个链表
来查找目标元素。可以通过改变桶的个数来控制哈希表的性能和空间占用率。此外，PHP7还会
动态调整哈希表的尺寸和重新哈希来控制哈希冲突的个数。
所以php数组是由哈希表+双向链表实现。
```

18. 如何用原生代码实现RPC远程调用？

```
什么是RPC？
RPC叫做远程过程调用
服务端搭建：
创建一个接收服务index.php
<?php
    // 建立一个socket服务
    $socketServer = stream_socket_server("tcp:127.0.0.1:8887", $errno, $errstr);
    if (!$socketServer) {
        echo "建立socket服务失败";
        die(1);
    }
    while(1) {
        // 接收服务请求
        $buff = stream_socket_accept($socketServer);
        // 读取数据
        $data = fread($buff, 2048);
        // 解析数据
        $json = json_decode($data, true);
        // 判断类文件是否存在
        $class = $json['class'];
        $file = $class.".php";
        require($file);
        if (!file_exists($file) {
            continue;
        }
        $method = $json['method'];
        $obj = new $class();
        $objData = $obj->$method();
        frwrite($buff, json_encode($objData));
        fclose($buff);
    }

创建具体的服务user.php
<?php
    class User {
        public function getName() {
            return [
                "code":1,
                "data": "sdf",
                "msg": "success"
            ];
        }
    }

创建客户端client.php
<?php
    $client = stream_socket_client("tcp://127.0.0.1:8887", $errno, $errstr);
    if (!$client) {
        echo "客户端创建失败";
        die(1);
    }
    // 请求数据
    frwite(json_encode([
        "class" => "user",
        "method" => "getName"    
    ]))
    // 接收数据
    $returnData = json_decode(fread($client, 2048));
    fclose($client);‘
```
