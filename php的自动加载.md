- 两种方式。先spl_autoload_register()，后__autoload()
- 实现：类的自动加载实际就是定义了一个钩子函数，实例化类时如果在EG(class_table)中没有找到对应的类则会调用这个钩子函数，调用完以后再重新查找一次。这个钩子函数保存在EG(autoload_func)中。
- spl_autoload_register([ callable $autoload_function [, bool $throw = true [, bool $prepend = false ]]])。可多个加载器。
	- 实现：spl创建了一个队列来保存用户注册的加载器，然后定义了一个spl_autoload函数到EG(autoload_func)，当找不到类时内核回调spl_autoload，这个函数再依次调用用户注册的加载器，没调用一个重新检查下查找的类是否在EG(class_table)中已经注册，仍找不到的话继续调用下一个加载器，直到类成功注册为止。
- __autoload()。1个加载器。
