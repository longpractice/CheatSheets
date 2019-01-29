## Memory location
A memory location is
* an object of scalar type (arithmetic type, pointer type, enumeration type, or std::nullptr_t)
* or the largest contiguous sequence of bit fields of non-zero length 

```cpp
struct
{
    int i; //mem location
    double d; //mem location
    unsigned bf1:10; //mem location shared by bf1 and bf2
    int bf2:25; //mem location shared by bf1 and bf2
    int bf3:0; //mem location
    int fb4:9; //mem location
    std::string s; //several memory locations inside
    char c1, c1; // two mem locations
}
```
Concurrent Actions including writing on a same mem location in different threads creates race condition(undefined behavior).

## Modification order

For a certain memory location, when no race-condition, that is, in defined behavior, different threads must see the same modification order. 

Eg: a read of an object that follows a write to that object in the same thread must either return the value written or another value that occurs later in the modification order.

## `std::atomic_flag`

lock_free. must initialize with ATOMIC_FLAG_INIT. can not be copied, moved, assigned,copy-assigned by another var with same type(apply to all atomic types since these ops include two atomic types and cannot be done lock-freely atomically).

Only two operations: clear, test_and_set.

Normally used as the build block of other type: 

```cpp
class spinlock_mutex
{
    std::atomic_flag flag:
public:
    spinlock_mutex():flag(ATOMIC_FLAG_INIT){}
    void lock(){
        while(flag.test_and_set(std::memory_order_acquire));
    }
    void unlock(){
        flag.clear(std::memory_order_release);
    }
}
```

## all `std::atomic`

assignment to basic type will not return a ref but the newly stored value.

## `std::atomic<bool>`

`bool compare_exchange_weak(bool expected, bool toSet)`: exchange with toSet if old stored value val equals to expected. Spurious failure(does not exchange even when condition met) will return false.

`bool compare_exchange_strong(bool expected, bool toSet)`:
similar to above but only fail when expected condition not met.


## `std::atomic<T*>` pointer specialization

read-modify-write atomic pointer arithmetric operations: 

* fetch_add(), fetch_sub(): can have any of the memory-ordering tags 
* , +=, -=, ++(pre and post), --(pre and post): always memory_order_seq_cst.
  
## `std::atomic` integral type specialization

all normal integral operations are available for atomic operation, except division, multiplication and shift.

## `std::atomic` primary class template

The requirement of base type:
* memcpy copiable: must have trivial copy-assignment operator. (no virtual method or virtual base, must use the compiler-generated copy-assignment operator, all members must also have trivial copy-assignment operator). Copy assignment could be applied by memcpy.
* memcmp comparable: bitwise equality comparable

Reason: to be safe, the atomic types could not pass ref of mutex-protected data to the user-defined function. Also if so, for eg, we have some 8 bits type, compiler could use std::atomic<int64_t> to implement this.

Operation available on types other than integral or pointer:
limited to the set of operations available for std::atomic<bool>: load(), store(), exchange(), compare_exchange_weak(), compare_exchange_strong(), and assignment from and conversion to an instance of type T.

## free functions atomic on std::shared_ptr(deprecated in Cpp20)
