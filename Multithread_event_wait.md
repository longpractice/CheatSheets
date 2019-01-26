### Multithreading-wait for an event or other condition

Three methods:
1. `std::atomic<bool>` flag. Thread A sets this flag for notification. Thread B reads it in while loop to check notification.
2. Same as 1, but add a `std::this_thead::sleep_for` in thread B.
3. For multiple shots, wait on `std::condition_variable`. 
4. Single shot, use `std::future`(unique future) and `std::shared_future`
---
condition_variable must work with only std::mutex, std::condition_variable_any work with any mutex-like type. Classic usage of condition_variable.
```cpp
//mut and condtion_variable always come in pair(mut is used for locking cond var), would normally also come together with a specific data structure to check in lambda passed to wait
mutex mut; condition_variable cond;
queue<Data> data_queue;
void data_preparation_thread(){
    while(more_data_to_prepare()){
        Data const data = prepare_data();
        lock_guard<mutex> lk(mut);
        data_queue.push(data);
        cond.notify_one();
    }
}
void data_processing_thread()
{
    while(true){
        unique_lock<mutex> lk(mut); //lock_guard cannot unlock
        cond.wait(lk, []{return !data_queue.empty();});
        Data data = data_queue.front(); data_queue.pop();
        lk.unlock();
        process(data);
        if(is_last_chunk(data)) break;
    }
}
```
__`condition_variable::wait` does not wait at all if lambda passes(thus not wait forever if missed a single notification)___:
1. It first checks the lambda, if true, keep going, if false, wait and release the lock
2. When notified, it first acquired lock, then check lambda, if true, keep going, if false, release the lock and keep waiting.

---
### "Providers" of `std::future` / `std::shared_future`
`std::async` informs by the return of function it launches.

`std::packaged_task` informs by invoking `std::packed_tast::operator()`

`std::promise` informs by `std::promise::set_value/set_exception`



