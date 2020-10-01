# Fizz Buzz Problem using four threads

```java
public class Playground {
    public static void main(String[] args) {
        Task task = new Task();
        Thread firstThread = new Thread(new Control(task, 1), " Fizz");
        Thread secondThread = new Thread(new Control(task, 2), " Buzz");
        Thread thirdThread = new Thread(new Control(task, 3), " Fizz-Buzz");
        Thread fourthThread = new Thread(new Control(task, 4), " ");
        firstThread.start();
        secondThread.start();
        thirdThread.start();
        fourthThread.start();
    }
}
```

<br/>

```java
class Control implements Runnable {
    Task task;
    int isTurn;

    public Control(Task task, int isTurn) {
        this.task = task;
        this.isTurn = isTurn;
    }
    
    @Override
    public void run() {
        while (task.currNum <= 25) {
            task.printNum(isTurn);
        }
    }
}
```

<br/>

```java
class Task {
    private volatile int isTurn = 4;
    volatile int currNum = 1;

    public synchronized void printNum(int isTurn) {
        try {
            while (currNum <= 25 && this.isTurn != isTurn) {
                wait();
            }
            if (currNum <= 25) {
                System.out.println(currNum + Thread.currentThread().getName());
                Thread.sleep(1000);
                this.isTurn = getNextTurn(currNum + 1);
                currNum++;
                notifyAll();
            }

        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private int getNextTurn(int number) {
        return number % 15 == 0 ? 3 : (number % 5 == 0 ? 2 : (number % 3 == 0 ? 1 : 4));
    }
}
```

