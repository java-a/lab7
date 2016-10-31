# lab7

> 使用二维数组实现 lab5 的“井字棋”小游戏，实现悔棋和撤销悔棋，并了解文档和注释的书写。

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

### 要求

- 实现基本的“井字棋”的游戏逻辑（可以参考最后的[起始代码](#起始代码)）。
- 使用**二维数组**保存棋盘状态。
- 实现悔棋与撤销悔棋。
- 使用良好的代码风格和适当的注释。

## 代码风格和注释

PJ 约定的代码风格详见 lab3 中的 [Java Style Guide](https://github.com/java-a/lab3#java-style-guide) 。注意，提交的代码中若不符合该约定将会被酌情扣分。在这个 lab 中，着重强调以下几点：

- 缩进
  - 在IntelliJ IDEA中，你可以使用 <kbd>Ctrl + Alt + L</kbd> （Windows）或 <kbd>Option + Command + L</kbd>（MacOS）来自动格式化代码。
- 变量命名
  - 在Java中，我们使用[驼峰命名法](http://baike.baidu.com/link?url=36TNYWM87ZKQKN5r1RayLumvi7wqv3vmVcgi7eicJVD4VpbpNyMUp443RFJ4coFeosuNIg1TZny2p9fTTlpOva)。类名应该以**大写**字母开头，变量名应以**小写**字母开头，**杜绝中文字符**。 正确示例（类名）：`TicTacToe`。错误示例（类名）：`tic-tac-toe`、`tic_tac_toe`、`AnimalFight_landscape`。
  - 变量命名必须有意义。正确示例（变量名）：`nextPosition`（下一个落子位置）、`gameHistory`（历史记录对象的一个实例）、`animals`（储存所有动物的数组）。错误示例：`count_step_for_all`、`animals$int$2`、`player2animals$int$2`。
- 代码注释
  - 注释应用于阐述一段代码为什么要这样写，而不是在做什么。

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

