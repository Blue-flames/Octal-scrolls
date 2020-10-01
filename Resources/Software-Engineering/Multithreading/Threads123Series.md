# Print 1 2 3 Series using 3 Threads

```java
public class file {
    static final String one = " ONE_THREAD";
    static final String two = " TWO_THREAD";
    static final String three = " THREE_THREAD";

    public static void main(String[] args) {
        SharedPrinter sharedPrinter = new SharedPrinter();
        Thread oneThread = new Thread(new Task(sharedPrinter, true), one);
        Thread twoThread = new Thread(new Task(sharedPrinter, false), two);
        Thread threeThread = new Thread(new Task(sharedPrinter, null), three);
        oneThread.start();
        twoThread.start();
        threeThread.start();
    }
}
```
<br/>

```java
class Task implements Runnable {
    private SharedPrinter sharedPrinter;
    private Boolean isTurn;

    public Task(SharedPrinter sharedPrinter, Boolean isEven) {
        this.sharedPrinter = sharedPrinter;
        this.isTurn = isEven;
    }

    @Override
    public void run() {
        int number = isTurn != null ? (isTurn ? 1 : 2) : 3;
        while (number <= 10) {
            sharedPrinter.printNumber(number, isTurn);
            number += 3;
        }
    }
}
```
<br/>

```java
class SharedPrinter {
    private volatile Boolean isTurn = true;

    synchronized void printNumber(int number, Boolean isTurn) {
        try {
            while (this.isTurn != isTurn) {
                wait();
            }
            System.out.println(number + Thread.currentThread().getName());
            if (this.isTurn != null && this.isTurn) {
                this.isTurn = false;
            } else if (this.isTurn != null && !this.isTurn) {
                this.isTurn = null;
            } else {
                this.isTurn = true;
            }
            Thread.sleep(1000);
            notifyAll();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```