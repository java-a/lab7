# lab7

> 使用二维数组实现 lab5 的“井字棋”小游戏，实现悔棋和撤销悔棋，并了解注释的书写。
>
> Deadline：`2016.11.02 23:59:59(UTC+8)`。

## 问题描述

井字棋的规则详见 lab5 的文档： https://github.com/java-a/lab5 。


### 约定

- 使用命令行读入控制信息及输出棋盘，棋盘中用`.`表示空位，`O`和`X`表示两位玩家。

- 两位玩家交替输入一个`[0, 8]`闭区间中的正整数，表示落子位置（如`0`表示第一排第一个坑，`5`表示第二排正中那个坑，`8`表示第三排最后一个坑）。具体的对应关系如下所示：

  > 0 1 2
  >
  > 3 4 5
  >
  > 6 7 8

- 玩家输入n个`u`（undo），回到n步之前的状态。如`u`表示回到上一步的状态，`uuu`表示回到三步前的状态。注意，悔棋后落子的玩家可能会发生变化（如悔棋1步后当前玩家可能从`X`变为`O`）。

- 玩家输入n个`r`（redo），撤销n步悔棋。如`r`表示回到下一步的状态，`rrr`表示回到三步后的状态。注意，撤销悔棋后落子的玩家可能会发生变化（如撤销悔棋1步后当前玩家可能从`X`变为`O`）。另外，可能会出现无法撤销悔棋的情况，如试图撤销的步数超过之前悔棋的步数，或上一次落子后尚未悔棋。

- 输入不要求合法性判断，仅考虑上述三种情况。可以在多次输入中连续悔棋、撤销悔棋，或混合使用。

### 要求

- 实现基本的“井字棋”的游戏逻辑（可以参考最后的[起始代码](#起始代码)）。
- 使用方法分离不同作用的代码块。
- 使用**二维数组**保存棋盘状态。
- 实现悔棋与撤销悔棋（优先实现悔棋，若时间上来不及撤销悔棋可留到课后）。
- 使用良好的代码风格和适当的注释。

## 起始代码

下面的起始代码为助教提供的 lab5 参考实现。你可以在这份代码的基础上进行修改，以完成本次 lab 提出的要求；也可以按照自己的编码习惯独立完成。

```java
import java.util.Scanner;

public class TicTacToe {

    public static void main(String[] args) {
        char[][] board = {{'.', '.', '.'}, {'.', '.', '.'}, {'.', '.', '.'}};
        boolean player = true;
        Scanner scanner = new Scanner(System.in);
        while (true) {
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    System.out.print(board[i][j]);
                }
                System.out.println();
            }
            System.out.print("Next move: ");
            int nextPosition = scanner.nextInt();
            board[nextPosition / 3][nextPosition % 3] = (player) ? 'O' : 'X';
            player = !player;
        }
    }
}
```

## Tips

1. 棋盘历史记录的保存：由于我们使用二维数组来保存棋盘状态，因此我们可以使用更高一维的数组来表示所有棋盘的历史状态。在这里`boardHistory[0]`表示棋盘初始状态，`boardHistory[3]`表示进行到第3步时的状态。

   ```java
   char[][] board = {{'.', '.', '.'}, {'.', '.', '.'}, {'.', '.', '.'}};
   char[][][] boardHistory = new char[10][][];
   ```

2. 多维数组的复制：可以通过循环来实现多维数组的深复制。

   ```java
   private static char[][] copyArray(char[][] array) {
           char[][] newArray = new char[3][3];
           for (int i = 0; i < 3; i++) {
               for (int j = 0; j < 3; j++) {
                   newArray[i][j] = array[i][j];
               }
           }
           return newArray;
   }
   ```

   用法：

   ```java
   board = TicTacToe.copyArray(boardHistory[nextStep]);
   ```

3. 棋盘状态的记录方法（供参考）：使用两个变量来分别记录当前步数`currentStep`和可以通过撤销悔棋回到的最大步数`lastStep`。

   - 初始状态时，`currentStep`与`lastStep`相等为0。
   - 执行悔棋时，若即将回到的步数`nextStep`是否大于等于0则可以执行悔棋，将`currentStep`修改为`nextStep`。
   - 执行撤销悔棋时，若即将回到的步数`nextStep`是否小于等于`lastStep`则可以执行撤销悔棋，将`currentStep`修改为`nextStep`。
   - 执行走棋时，清空`currentStep`到`lastStep`之间的棋盘状态。随后`currentStep`自增，`lastStep`与`currentStep`相等。

## 代码风格和注释

PJ 约定的代码风格详见 lab3 中的 [Java Style Guide](https://github.com/java-a/lab3#java-style-guide) 。注意，提交的代码中若不符合该约定将会被酌情扣分。在这个 lab 中，着重强调以下几点：

- 缩进

  - 在IntelliJ IDEA中，你可以使用 <kbd>Ctrl + Alt + L</kbd> （Windows）或 <kbd>Option + Command + L</kbd>（MacOS）来自动格式化代码。

- 变量命名

  - 在Java中，我们使用[驼峰命名法](http://baike.baidu.com/link?url=36TNYWM87ZKQKN5r1RayLumvi7wqv3vmVcgi7eicJVD4VpbpNyMUp443RFJ4coFeosuNIg1TZny2p9fTTlpOva)。类名应该以**大写**字母开头，变量名应以**小写**字母开头，**杜绝中文字符**。 正确示例（类名）：`TicTacToe`。错误示例（类名）：`tic-tac-toe`、`tic_tac_toe`、`AnimalFight_landscape`。
  - 变量命名必须有意义。正确示例（变量名）：`nextPosition`（下一个落子位置）、`gameHistory`（历史记录对象的一个实例）、`animals`（储存所有动物的数组）。错误示例：`count_step_for_all`、`animals$int$2`、`player2animals$int$2`。

- 代码注释

  PJ 中的注释可以参照下面的格式。

  -  文件开头的注释。IntelliJ在新建一个Java文件后会自动生成一段注释。这段注释中应包括作者、创建时间和该文件的主要内容。

     ```java
     /**
     * Created by Zhongyi on 01/11/2016.
     * This file contains.../ is used to ...
     */
     ```

  - 类前的注释。表明类的职责。

    ```java
    /**
     * Class <code>Object</code> is the root of the class hierarchy.
     * Every class has <code>Object</code> as a superclass. All objects,
     * including arrays, implement the methods of this class.
     */
    public class Object {}
    ```
    
  -  方法前的注释。表明方法的作用，描述参数和返回值的含义。实例中的`Examples`和`@exception`视情况可选。

     ```java
      /**
     * Returns a new string that is a substring of this string. The
     * substring begins with the character at the specified index and
     * extends to the end of this string.
     *
     * Examples:
     * "unhappy".substring(2) returns "happy"
     * "Harbison".substring(3) returns "bison"
     * "emptiness".substring(9) returns "" (an empty string)
     *
     * @param      beginIndex   the beginning index, inclusive.
     * @return     the specified substring.
     * @exception IndexOutOfBoundsException if
     *             <code>beginIndex</code> is negative or larger than the
     *             length of this <code>String</code> object.
     */
     public String substring(int beginIndex) {
     	return substring(beginIndex, count);
     }
    ```

  - 代码块中的注释。此处的注释应该用于阐述一段代码**为什么**要这样写，而不是在做什么。当且仅当代码本身无法让读者一眼明白在做什么时，才需要解释代码的作用。

    ```java
    // Check if the input is valid
    if (...) {}
    ```
    
## Code Review
1. [lab5 Code Review](https://github.com/java-a/lab5/issues/2)
2. [lab6 Code Review](https://github.com/java-a/lab6/issues/4)
   请参考链接中的提示，完成相关文档。

## 提交

Deadline延长至`2016.11.02 23:59:59(UTC+8)`。迟交酌情扣分。

将代码打包，以`学号_姓名.文件类型`的格式命名，如`13302010039_童仲毅.zip`。上传至FTP：

```json
ftp://10.132.141.33/classes/16/161 程序设计A （戴开宇）/WORK_UPLOAD/lab7
```
Lab6的Code Review文档请上传至：

```json
ftp://10.132.141.33/classes/16/161 程序设计A （戴开宇）/WORK_UPLOAD/Code Review/lab6
```
