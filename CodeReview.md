# Lab 7 Code Review

> 阅读样例代码，完成思考题。
>
> 提交：将所有回答写在同一个文档中，以学号+姓名命名，如`16302010002李云帆.txt`、`16302010063郭涵青.docx`。上传至 FTP `WORK_UPLOAD > CodeReview > lab7` 目录。
>
> Deadline: 2016.11.08 23:59:59 (UTC+8)

## 样例代码

```java
import java.util.Scanner;

public class TicTacToe {

    public static void main(String[] args) {
        char[][][] boardHistory = new char[10][][];
        int currentStep = 0, lastStep = 0;

        Scanner scanner = new Scanner(System.in);
        boardHistory[0] = new char[][]{{'.', '.', '.'}, {'.', '.', '.'}, {'.', '.', '.'}};

        while (currentStep <= 9) {
            printBoard(boardHistory[currentStep]);

            String nextMove = scanner.next();

            int nextStep = currentStep - nextMove.length();
            if (nextMove.charAt(0) == 'r') {
                currentStep = redo(currentStep, lastStep, nextStep);
            } else if (nextMove.charAt(0) == 'u') {
                currentStep = undo(currentStep, nextStep);
            } else {
                int nextPosition = Integer.parseInt(nextMove);
                lastStep = ++currentStep;
                boardHistory[currentStep] = copyBoard(boardHistory[currentStep - 1]);
                boardHistory[currentStep][nextPosition / 3][nextPosition % 3] = (currentStep % 2 != 0) ? 'O' : 'X';
            }
        }
    }

    private static int redo(int currentStep, int lastStep, int nextStep) {
        if (nextStep <= lastStep) {
            currentStep = nextStep;
        } else {
            System.out.println("Invalid redo.");
        }
        return currentStep;
    }

    private static int undo(int currentStep, int nextStep) {
        if (nextStep >= 0) {
            currentStep = nextStep;
        } else {
            System.out.println("Invalid undo.");
        }
        return currentStep;
    }

    private static void printBoard(char[][] board) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                System.out.print(board[i][j]);
            }
            System.out.println();
        }
        System.out.print("Next move: ");
    }

    private static char[][] copyBoard(char[][] array) {
        char[][] newArray = new char[3][3];
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                newArray[i][j] = array[i][j];
            }
        }
        return newArray;
    }
}
```

## 思考

1. `boardHistory[0]`的含义是什么？`boardHistory[0][0]`呢？
2. 这段代码中`currentStep`和`lastStep`分别有什么作用？它们的值在游戏过程中是如何变化的？`lastStep`是否可以省略？
3. 程序是如何实现悔棋和撤销悔棋的？详细描述这个过程。
4. 在复制棋盘时，我们为什么要使用`copyBoard`方法，而不直接使用`boardHistory[currentStep] = boardHistory[currentStep - 1];`语句？
5. 你的实现与样例代码有哪些不同？有哪些可以改进的地方？