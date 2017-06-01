# 배열

D에는 **static** 와 **dynamic** 등 두 종류의 배열이 있습니다.
배열에 접근할때는 종류에 상관없이 경계검사가 수행됩니다.(컴파일러가 경계검사의 불필요성을 증명 할 수 있는 경우는 제외)
경계검사에 실패하면 `RangeError`를 발생시켜 응용프로그램을 중단합니다.
용감한 분들은 다소 안전성을 해치더라도 속도개선을 위해서 컴파일러 플래그를 `-boundschecks=off`로 세팅할수 있습니다.

#### 정적 배열

정적 배열은 함수 내에서 정의된 경우 스택에, 그렇지 않으면 정적 메모리에 저장됩니다.
이것은 컴파일할때 고정된 길이값을 가지고 있습니다.
정적배열의 타입은 고정크기값을 가지고 있습니다 :

    int[8] arr;

`arr`'의 타입은 `int[8]` 입니다. 배열의 길이를 C/C++과 달리 변수옆이 아닌 타입 옆에 표시한다는 것에 주의하세요.

#### 동적 배열

동적 배열들은 힙(heap)에 저장되어 런타임 중에 확장되거나 축소될 수 있습니다. 동적 배열은 `new`라는 표현식과 길이값으로 생성합니다 :

    int size = 8; // run-time variable
    int[] arr = new int[size];

`arr`의 타입은 `int[]` 이고 이것은 **slice** 입니다. 슬라이스는 [다음장](basics/slices)에서 더 자세히 설명됩니다. 다차원배열은 `auto arr = new int[3][3]` 구문과 같이 쉽게 만들수 있습니다.

#### 배열 연산과 속성

 
배열은 `~`연산자를 이용해 연결될 수 있습니다. 이때 새로운 동적배열이 생깁니다.

수학적 연산은 예를 들어 `c[] = a[] + b[]` 구문과 같이 모든 배열에 적용할 수 있습니다. 이것은 `a`와`b`의 모든 요소를`c [0] = a [0] + b [0]``c [1] = a [1] + b [1]`등과 같이 서로 더합니다. 또한 단일 값을 배열과 연산할 수 도 있습니다 :

    a[] *= 2; // 모든 요소에 2를 곱한값
    a[] %= 26; // 모든 요소의 26에 대한 나머지값 계산

컴파일러는 이러한 연산들을 한번에 수행할 수 있는 특별 프로세서 명령어들을 사용하여 최적화 할 수도 있습니다.

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
