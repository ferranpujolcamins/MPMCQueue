# MPMCQueue.h

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/rigtorp/MPMCQueue/master/LICENSE)

A fork of [Erik Rigtorp's MPMCQueue](https://github.com/rigtorp/MPMCQueue), a bounded multi-producer multi-consumer concurrent queue written in C++11.

This fork offers additional methods:
- `try_consume_until_current_head` method that consumes all[1] elements currently on the queue. Elements concurrently added while the method is being executed won't be consumed.
    ```c++
    int counter = 0;

    MPMCQueue<int> q(16);
    q.push(1);
    q.push(2);
    q.push(3);
    
    q.try_consume_until_current_head(
        [&counter](int v) noexcept {
            counter += v;
        }
    );
    
    assert(counter == 6);
    ```

- `try_consume_until_current_head_order`, similar to `try_consume_until_current_head`, but the consumer takes an additional int parameter, which represents
  the order in which the elements were enqueued: the order value for an element `a` is strictly less than the order value for an element `b`
  if and only if `a` was enqueued before b.
    ```c++
    int lastValue = 0;
    
    MPMCQueue<int> q(16);
    q.push(1);
    q.push(2);
    q.push(3);
    
    q.try_consume_until_current_head(
        [&counter](int v, int order) noexcept {
            if 
        }
    );
    
    assert(counter == 3);
    ```

[1] This method consumes **all** the elements currently on the queue only when it is the only when there are no other concurrent consumers. If there are other concurrent consumers, this method might **not** consume all the elements currently on the queue because other consumers might consume some of them.

## About
[This fork](https://github.com/ferranpujolcamins/MPMCQueue) is mantained by Ferran Pujol Camins, based on [the original project](https://github.com/rigtorp/MPMCQueue) by Erik Rigtorp.
