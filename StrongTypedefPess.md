https://godbolt.org/z/b4jfbjaY9

The following is a very common code pattern when using any strong-typedef library - i.e., replacing (say) `int` with a wrapper like myInt.
```cpp
struct Wrapper1 { long int t; };
struct Wrapper2 { long int t; };
struct S { Wrapper1 a; Wrapper2 b; };

// Assignment vectorized properly:
void f1(S& s1, S& s2 ) {
    s1 = s2;
}

// Assignment not optimized due to bogus potential aliasing between a and b
// (see opt remarks):
void f3(S& s1, S& s2) {
    s1.a = s2.a;
    s1.b = s2.b;
}

// Assignment vectorized properly:
void f2(S& s1, S& s2) {
    s1.a.t = s2.a.t;
    s1.b.t = s2.b.t;
}
```
