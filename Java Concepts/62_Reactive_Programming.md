# Reactive Programming

## What is Reactive Programming?
Reactive programming is a programming paradigm focused on asynchronous data streams and propagation of change.

## Reactive Streams API (Java 9+)

### Basic Concepts
```java
import java.util.concurrent.Flow.*;

// Publisher - produces items
interface Publisher<T> {
    void subscribe(Subscriber<? super T> subscriber);
}

// Subscriber - consumes items
interface Subscriber<T> {
    void onSubscribe(Subscription subscription);
    void onNext(T item);
    void onError(Throwable throwable);
    void onComplete();
}

// Subscription - controls flow
interface Subscription {
    void request(long n);
    void cancel();
}
```

### Custom Publisher
```java
import java.util.concurrent.Flow.*;

class SimplePublisher implements Publisher<Integer> {
    @Override
    public void subscribe(Subscriber<? super Integer> subscriber) {
        Subscription subscription = new SimpleSubscription(subscriber);
        subscriber.onSubscribe(subscription);
    }
    
    class SimpleSubscription implements Subscription {
        private Subscriber<? super Integer> subscriber;
        private boolean cancelled = false;
        
        public SimpleSubscription(Subscriber<? super Integer> subscriber) {
            this.subscriber = subscriber;
        }
        
        @Override
        public void request(long n) {
            if (!cancelled && n > 0) {
                for (long i = 0; i < n; i++) {
                    subscriber.onNext((int) i);
                }
                subscriber.onComplete();
            }
        }
        
        @Override
        public void cancel() {
            cancelled = true;
        }
    }
}
```

## RxJava Basics

### Setup
```xml
<dependency>
    <groupId>io.reactivex.rxjava3</groupId>
    <artifactId>rxjava</artifactId>
    <version>3.1.8</version>
</dependency>
```

### Observable
```java
import io.reactivex.rxjava3.core.Observable;

// Create Observable
Observable<String> observable = Observable.just("Hello", "World", "RxJava");

// Subscribe
observable.subscribe(
    item -> System.out.println("Received: " + item),
    error -> System.err.println("Error: " + error),
    () -> System.out.println("Completed")
);
```

### Operators
```java
import io.reactivex.rxjava3.core.Observable;

Observable.range(1, 10)
    .filter(x -> x % 2 == 0)
    .map(x -> x * 2)
    .subscribe(System.out::println);
```

### Backpressure
```java
import io.reactivex.rxjava3.core.Flowable;

Flowable.range(1, 1000000)
    .onBackpressureBuffer()
    .subscribe(System.out::println);
```

## Project Reactor

### Setup
```xml
<dependency>
    <groupId>io.projectreactor</groupId>
    <artifactId>reactor-core</artifactId>
    <version>3.6.0</version>
</dependency>
```

### Mono (0 or 1 item)
```java
import reactor.core.publisher.Mono;

Mono<String> mono = Mono.just("Hello");
mono.subscribe(System.out::println);

// Error handling
Mono.error(new RuntimeException("Error"))
    .onErrorReturn("Default")
    .subscribe(System.out::println);
```

### Flux (0 to N items)
```java
import reactor.core.publisher.Flux;

Flux<Integer> flux = Flux.range(1, 10);
flux
    .filter(x -> x % 2 == 0)
    .map(x -> x * 2)
    .subscribe(System.out::println);
```

### Operators
```java
Flux.range(1, 10)
    .filter(x -> x > 5)
    .map(x -> x * 2)
    .take(3)
    .subscribe(System.out::println);
```

## Best Practices
1. Understand backpressure
2. Handle errors properly
3. Use appropriate operators
4. Avoid blocking operations
5. Test reactive code thoroughly
6. Monitor resource usage
7. Use schedulers appropriately
8. Clean up subscriptions

