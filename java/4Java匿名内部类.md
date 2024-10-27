## Java匿名内部类
#### 匿名内部类介绍：
<table>
    <th>解释</th>
    <th>作用</th>
    <tr>
        <td>一种没有显式命名的内部类。</td>
        <td>用于简化代码，常见于临时对象创建，实现接口，减少代码的复杂性，类的数量，提高代码的可读性和可维护性</td>
    </tr>
</table>

```
/**
 * <p>
 *     匿名内部类的方式：
 *     优点：减少类文件创建数量，代码直观，可维护
 *     缺点：仅能被使用一次，创建匿名内部类时，会立即创建一个该类的实例，
 * </p>
 * @author 吴宇伦
 */
public class BirdTest {

    public void test(Bird bird) {
        System.out.println("birds' name: " + bird.getName() + " can fly:" + bird.fly() + " miles");
    }

    public static void main(String[] args) {
        BirdTest birdTest = new BirdTest();
        birdTest.test(new Bird() {

            @Override
            public int fly() {
                return 0;
            }

            @Override
            public String getName() {
                return "bee";
            }
        });
    }
}
```
#### 查看匿名内部类在编译和运行时，会产生什么

```shell
    javac Bird.java BirdTest.java
```

#### 可以看到，会自动为匿名内部类生成一个final修饰的类

```
package com.garden.alanni.inner;

final class BirdTest$1 extends Bird {
    BirdTest$1() {
    }

    public int fly() {
        return 0;
    }

    public String getName() {
        return "bee";
    }
}

```

#### 匿名内部类注意点
<table>
    <th>注意点</th>
    <th>解释</th>
    <tr>
        <td>使用匿名内部类时，必须是继承一个类或者实现一个接口，但是两者不可兼得，只能实现继承类或者实现一个接口</td>
        <td>-</td>
    </tr>
    <tr>
        <td>匿名内部类中是不能定义构造函数的</td>
        <td>-</td>
    </tr>
    <tr>
        <td>匿名内部类中不能存在任何静态成员变量和静态方法</td>
        <td>编译运行，匿名内部类只生成一次，只使用一次</td>
    </tr>
    
</table>
