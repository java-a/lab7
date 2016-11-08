# Lab 7 Code Review

> 阅读样例代码，完成思考题。
>
> 提交：将所有回答写在同一个文档中，以学号+姓名命名，如`16302010002李云帆.txt`、`16302010063郭涵青.docx`。上传至 FTP `WORK_UPLOAD > CodeReview > lab7` 目录。
>
> Deadline: 2016.11.08 23:59:59 (UTC+8)

## 样例代码

```java
/*
 * Created by Zhongyi on 06/11/2016.
 * TicTacToe with multiple-step undo and redo.
 * Example code for Lab 7.
 */

import java.util.Scanner;

/**
 * Class TicTacToe is the entry point of the game.
 */
public class TicTacToe {

    public static void main(String[] args) {
        char[][][] boardHistory = new char[10][][];
        // What are these variables used for? Attach your answer as part of the code review.
        int currentStep = 0, lastStep = 0;

        Scanner scanner = new Scanner(System.in);
        // Initial state of the game, as step 0
        boardHistory[0] = new char[][]{{'.', '.', '.'}, {'.', '.', '.'}, {'.', '.', '.'}};

        /*
        * In an legal game, there shall be up to nine steps.
        * Considering the initial state, the step varies from 0 to 9, inclusively.
        */
        while (currentStep <= 9) {
            printBoard(boardHistory[currentStep]);

            String nextMove = scanner.next();

            /*
            * If the input is an undo or redo command, its length is the steps to jump.
            * We can calculate the nextStep based on that.
            * */
            int nextStep = currentStep - nextMove.length();
            if (nextMove.charAt(0) == 'r') {
                // Redo
                currentStep = redo(currentStep, lastStep, nextStep);
            } else if (nextMove.charAt(0) == 'u') {
/*
 * Created by Zhongyi on 06/11/2016.
 * TicTacToe with multiple-step undo and redo.
 * Example code for Lab 7.
 */

import java.util.Scanner;

/**
 * Class TicTacToe is the entry point of the game.
 */
public class TicTacToe {

    public static void main(String[] args) {
        char[][][] boardHistory = new char[10][][];
        // What are these variables used for? Attach your answer as part of the code review.
        int currentStep = 0, lastStep = 0;

        Scanner scanner = new Scanner(System.in);
        // Initial state of the game, as step 0
        boardHistory[0] = new char[][]{{'.', '.', '.'}, {'.', '.', '.'}, {'.', '.', '.'}};

        /*
        * In an legal game, there shall be up to nine steps.
        * Considering the initial state, the step varies from 0 to 9, inclusively.
        */
        while (currentStep <= 9) {
            printBoard(boardHistory[currentStep]);

            String nextMove = scanner.next();

            /*
            * If the input is an undo or redo command, its length is the steps to jump.
            * We can calculate the nextStep based on that.
            * */
            int nextStep;
            if (nextMove.charAt(0) == 'r') {
                // Redo
                nextStep = currentStep + nextMove.length();
                currentStep = redo(currentStep, lastStep, nextStep);
            } else if (nextMove.charAt(0) == 'u') {
                // Undo
                nextStep = currentStep - nextMove.length();
                currentStep = undo(currentStep, nextStep);
            } else {
                // Drop a new piece.
                // Parse the input string to integer to get the position to drop the piece.
                int nextPosition = Integer.parseInt(nextMove);
                // Update lastStep as long as currentStep
                lastStep = ++currentStep;
                // Store the current state into boardHistory,
                // by deep copying the previous state and substitute the position with new piece.
                boardHistory[currentStep] = copyBoard(boardHistory[currentStep - 1]);
                boardHistory[currentStep][nextPosition / 3][nextPosition % 3] = (currentStep % 2 != 0) ? 'O' : 'X';
            }
        }
    }

    /**
     * Redo from currentStep to nextStep.
     * Throw an error message to stdout if the move is illegal.
     *
     * @param currentStep the step to jump from
     * @param nextStep    the step to jump to
     * @param lastStep    the maximum step to jump
     * @return update currentStep to nextStep, if the move is legal; or change nothing
     */
    private static int redo(int currentStep, int lastStep, int nextStep) {
        if (nextStep <= lastStep) {
            currentStep = nextStep;
        } else {
            System.out.println("Invalid redo.");
        }
        return currentStep;
    }

    /**
     * Undo from currentStep to nextStep.
     * Throw an error message to stdout if the move is illegal.
     *
     * @param currentStep the step to jump from
     * @param nextStep    the step to jump to
     * @return update currentStep to nextStep, if the move is legal; or change nothing
     */
    private static int undo(int currentStep, int nextStep) {
        if (nextStep >= 0) {
            currentStep = nextStep;
        } else {
            System.out.println("Invalid undo.");
        }
        return currentStep;
    }

    /**
     * Iterate over the two dimensional array, to print the board.
     * Then, prompt a message to ask user for the next input.
     */
    private static void printBoard(char[][] board) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                System.out.print(board[i][j]);
            }
            System.out.println();
        }
        System.out.print("Next move: ");
    }

    /**
     * Use a nested iteration to deep copy a 2D array, i.e. the board.
     */
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
