# 1.2. Understanding how it works.

![alt text](img/image1.png)

When you call println!("hey hey"), it executes right away on the main thread—no futures have been driven yet. Invoking spawner.spawn(...) merely places the async block into the executor’s work queue; it doesn’t start running its contents immediately. Once you call executor.run(), the executor pulls that task off the queue and polls it for the first time, which triggers the "howdy!" print. The timer inside then returns Poll::Pending, so the task is re-queued. Two seconds later, the timer’s background thread uses the stored waker to wake the task; when executor.run() polls it again, you see "done!" printed.