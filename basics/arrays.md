# 배열

D에는 **static** 와 **dynamic** 등 두 종류의 배열이 있습니다.
배열에 접근할때는 종류에 상관없이 경계검사가 수행됩니다.(컴파일러가 경계검사의 불필요성을 증명 할 수 있는 경우는 제외)
경계검사에 실패하면 `RangeError`를 발생시켜 응용프로그램을 중단합니다.
용감한 분들은 안전성을 해치더라도 속도개선을 위해 컴파일러 플래그를 `-boundschecks=off`로 세팅할수 있습니다.

#### 정적 배열

정적 배열은 함수 내에서 정의된 경우 스택에, 그렇지 않으면 정적 메모리에 저장됩니다.
이것은 컴파일할때 고정된 길이값을 가지고 있습니다.
정적배열의 타입은 고정크기값을 가지고 있습니다:

    int[8] arr;

`arr`'s type is `int[8]`. Note that the size of the array is denoted
next to the type, and not after the variable name like in C/C++.

#### Dynamic arrays

Dynamic arrays are stored on the heap and can be expanded
or shrunk at runtime. A dynamic array is created using a `new` expression
and its length:

    int size = 8; // run-time variable
    int[] arr = new int[size];

The type of `arr` is `int[]`, which is a **slice**. Slices
will be explained in more detail in the [next section](basics/slices). Multi-dimensional
arrays can be created easily using the `auto arr = new int[3][3]` syntax.

#### Array operations and properties

Arrays can be concatenated using the `~` operator, which
will create a new dynamic array.

Mathematical operations can
be applied to whole arrays using a syntax like `c[] = a[] + b[]`, for example.
This adds all elements of `a` and `b` so that
`c[0] = a[0] + b[0]`, `c[1] = a[1] + b[1]`, etc. It is also possible
to perform operations on a whole array with a single
value:

    a[] *= 2; // multiple all elements by 2
    a[] %= 26; // calculate the modulo by 26 for all a's

These operations might be optimized
by the compiler to use special processor instructions that
do the operations in one go.

Both static and dynamic arrays provide the property `.length`,
which is read-only for static arrays, but can be used in the case of
dynamic arrays to change its size dynamically. The
property `.dup` creates a copy of the array.

When indexing an array through the `arr[idx]` syntax, a special
`$` symbol denotes an array's length. For example, `arr[$ - 1]` references
the last element and is a short form for `arr[arr.length - 1]`.

### Exercise

Complete the function `encrypt` to decrypt the secret message.
The text should be encrypted using *Caesar encryption*,
which shifts the characters in the alphabet using a certain index.
The to-be-encrypted text only contains characters in the range `a-z`,
which should make things easier.

### In-depth

- [Arrays in _Programming in D_](http://ddili.org/ders/d.en/arrays.html)
- [Array specification](https://dlang.org/spec/arrays.html)

## {SourceCode:incomplete}

```d
import std.stdio : writeln;

/**
Shifts every character in the
array `input` for `shift` characters.
The character range is limited to `a-z`
and the next character after z is a.

Params:
    input = array to shift
    shift = shift length for each char
Returns:
    Shifted char array
*/
char[] encrypt(char[] input, char shift)
{
    auto result = input.dup;
    // ...
    return result;
}

void main()
{
    // We will now encrypt the message with
    // Caesar encryption and a
    // shift factor of 16!
    char[] toBeEncrypted = [ 'w','e','l','c',
      'o','m','e','t','o','d',
      // The last , is okay and will just
      // be ignored!
    ];
    writeln("Before: ", toBeEncrypted);
    auto encrypted = encrypt(toBeEncrypted, 16);
    writeln("After: ", encrypted);

    // Make sure we the algorithm works
    // as expected
    assert(encrypted == [ 'm','u','b','s','e',
            'c','u','j','e','t' ]);
}
```
