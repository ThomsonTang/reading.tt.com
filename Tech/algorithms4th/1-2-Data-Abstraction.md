
## Q & A
### 1. Why the distinction between primitive and reference types? Why not just have reference types?
> **Performance.** Primitive types are closer to the types of data that are supported by computer hardware, **so programs that use them usually run faster than programs that use corresponding reference types.**

### 2. Do data types have to be abstract?
> No. Java also allows `public` and `protected` to allow some clients to refer directly to instance variables. (数据类型不一定都是抽象的，因为有些实例变量我们可以直接申明为`public`或`protected`，这样调用者就可以直接访问了。)

### 3. Where can I find more details on how java implements reference and does garbage collection?
> One Java system might differ completely from another. For example, one natural schema is to user a **pointer(machine address)**; another is to use a **handle(a pointer to a pointer).** The former gives faster access to data; the latter provides for better garbage collection.

### 4. How do I make a class immutable?
> To ensure immutability of a data type that includes an instance variable of a mutable type, we need to make a local copy, known as a **defensive copy**. And that may not be enough. Making the copy is one challenge; ensuring that none of the instance methods change values is another.