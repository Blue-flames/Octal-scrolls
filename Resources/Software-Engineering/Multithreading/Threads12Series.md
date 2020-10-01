#Print Odd even numbers using 2 Threads

```java
public class file {
    static final String odd = " ODD_THREAD";
    static final String even = " EVEN_THREAD";

    public static void main(String[] args) {
        SharedPrinter sharedPrinter = new SharedPrinter();
        Thread evenThread = new Thread(new Task(sharedPrinter, true), even);
        Thread oddThread = new Thread(new Task(sharedPrinter, false), odd);
        evenThread.start();
        oddThread.start();
    }
}
```
<br/>

```java
class Task implements Runnable {
    private SharedPrinter sharedPrinter;
    private boolean isEven;

    public Task(SharedPrinter sharedPrinter, boolean isEven) {
        this.sharedPrinter = sharedPrinter;
        this.isEven = isEven;
    }

    @Override
    public void run() {
        int number = isEven ? 2 : 1;
        while (number <= 10) {
            if (isEven) {
                sharedPrinter.printNumber(number, isEven);
            } else {
                sharedPrinter.printNumber(number, isEven);
            }
            number += 2;
        }
    }
}
```
<br/>

```java
class SharedPrinter {
    private volatile boolean isEven = true;
  
    synchronized void printNumber(int number, boolean isEven) {
        try {
            while (this.isEven == isEven) {
                wait();
            }
            System.out.println(number + Thread.currentThread().getName());
            if (this.isEven) {
                this.isEven = false;
            } else {
                this.isEven = true;
            }
            Thread.sleep(1000);
            notify();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```