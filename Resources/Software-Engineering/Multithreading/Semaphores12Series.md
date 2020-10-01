# Print Odd Even Series using Semaphores

```java
public class SemaphoreControl {
    public static void main(String[] args) {
        SharedPrinter sharedPrinter = new SharedPrinter();
        Thread oddThread = new Thread(new Task(sharedPrinter, false), " Odd thread");
        Thread evenThread = new Thread(new Task(sharedPrinter, true), " Even Thread");
        oddThread.start();
        evenThread.start();
    }
}
```

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
            sharedPrinter.printNumber(number, isEven);
            number += 2;
        }
    }
}
```

```java

class SharedPrinter {
    private Semaphore semEven = new Semaphore(0);
    private Semaphore semOdd = new Semaphore(1);

    void printNumber(int number, boolean isEven) {
        try {
            if (!isEven) {
                semOdd.acquire();
                System.out.println(number + Thread.currentThread().getName());
                Thread.sleep(1000);
                semEven.release();
            } else {
                semEven.acquire();
                System.out.println(number + Thread.currentThread().getName());
                Thread.sleep(1000);
                semOdd.release();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```