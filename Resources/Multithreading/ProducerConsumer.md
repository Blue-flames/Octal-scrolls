# Producer-Consumer


```
import java.util.LinkedList;
import java.util.Queue;

public class Control {
    public static void main(String[] args) {
        ProducerConsumer producerConsumer = new ProducerConsumer(3);
        Thread producer = new Thread(new Task(true, producerConsumer), "Producer ");
        Thread consumer = new Thread(new Task(false, producerConsumer), "Consumer ");
        producer.start();
        consumer.start();
    }
}
``` 
<br/>

```
class Task implements Runnable {
    boolean isProducer;
    ProducerConsumer producerConsumer;

    public Task(boolean isProducer, ProducerConsumer producerConsumer) {
        this.isProducer = isProducer;
        this.producerConsumer = producerConsumer;
    }

    @Override
    public void run() {
        try {
            while (true) {
                if (isProducer) {
                    producerConsumer.produce();
                } else {
                    producerConsumer.consume();
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
<br/>

```
class ProducerConsumer {
    Queue<Integer> list;
    int capacity;
    volatile int value = 1;

    public ProducerConsumer(int capacity) {
        this.capacity = capacity;
        list = new LinkedList<>();
    }

    public synchronized void produce() throws InterruptedException {
        while (capacity == list.size()) {
            wait();
        }
        System.out.println(Thread.currentThread().getName() + "Produced : " + value);
        list.add(value++);
        notify();
        Thread.sleep(1000);
    }

    public synchronized void consume() throws InterruptedException {
        while (0 == list.size()) {
            wait();
        }
        System.out.println(Thread.currentThread().getName() + "Consumed : " + list.poll());
        value--;
        notify();
        Thread.sleep(1000);
    }
}

```