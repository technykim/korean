# Ranges

컴파일러가 `foreach`문을 만나면

```
foreach (element; range)
{
    // Loop body...
}
```

내부적으로는 다음과 같이 재작성 됩니다.:

```
for (auto __rangeCopy = range;
     !__rangeCopy.empty;
     __rangeCopy.popFront())
 {
    auto element = __rangeCopy.front;
    // Loop body...
}
```

레인지 객체가 레퍼런스타입(예 : `class`)인 경우, (루프바디가 마지막 루프반복 전에 멈추지 않는 한) 레인지는 소비되고 이후 반복사용이 불가능 합니다. 
만약 레인지 객체가 밸류타입이면 레인지에 대한 복사본이 만들어지고 레인지가 정의되는 것에 따라 오리지널 레인지가 루프내에서 소비될지 말지 결정됩니다. 
표준 라이브러리 내의 대부분의 레인지는 구조체이기 때문에 `foreach`반복은 보통 비파괴적입니다(보장되지는 않음). 보장여부가 중요하다면 **forward**레인지를 사용합니다.

아래의 인터페이스를 따르는 객체는 **range**이고 반복될수 있는 유형입니다. 

If the range object is a reference type (e.g. `class`), then the range will be
consumed and won't available for subsequent iteration (that is unless the
loop body breaks before the last loop iteration). If the range object is
a value type, then a copy of the range will be made and depending on the
way the range is defined the loop may or may not consume the original
range. Most of the ranges in the standard library are structs and so `foreach`
iteration is usually non-destructive, though not guaranteed. If this
guarantee is important, require **forward** ranges.

Any object which fulfills the following interface is called a **range**
and is thus a type that can be iterated over:

```
    struct Range
    {
        T front() const @property;
        bool empty() const @property;
        void popFront();
    }
 ```
Note that while it is customary for `empty` and `front` to be defined as `const`
functions (implying that calling them won't modify the range), this is not
required.

The functions in `std.range` and `std.algorithm` provide
building blocks that make use of this interface. Ranges allow us
to compose complex algorithms behind an object that
can be iterated with ease. Furthermore, ranges allow us to create **lazy**
objects that only perform a calculation when it's really needed
in an iteration e.g. when the next range's element is accessed.
Special range algorithms will be presented later in the
[D's Gems](gems/range-algorithms) section.

### Exercise

Complete the source code to create the `FibonacciRange` range
that returns numbers of the
[Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_number).
Don't fool yourself into deleting the `assert`ions!

### In-depth

- [`std.algorithm`](http://dlang.org/phobos/std_algorithm.html)
- [`std.range`](http://dlang.org/phobos/std_range.html)

## {SourceCode:incomplete}

```d
import std.stdio : writeln;

struct FibonacciRange
{
    bool empty() const @property
    {
        // So when does the Fibonacci sequence
        // end?!
    }

    void popFront()
    {
    }

    int front() const @property
    {
    }
}

void main()
{
    import std.range : take;
    import std.array : array;

    FibonacciRange fib;

    // `take` creates another range which
    // will return N elements at maximum.
    // This range is _lazy_ and just
    // touches the original range
    // if actually needed
    auto fib10 = take(fib, 10);

    // But we do want to touch all elements and
    // convert the range to array of integers.
    int[] the10Fibs = array(fib10);

    writeln("The 10 first Fibonacci numbers: ",
        the10Fibs);
    assert(the10Fibs ==
        [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]);
}
```
