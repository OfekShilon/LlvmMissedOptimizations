Not really a missed optimization, but interesting discussion.
Start with this, generating optimal code:

```cpp
struct C {
    int a,b,c,d;
    bool operator==(C other) const { return a==other.a & b==other.b & c==other.c & d==other.d;}
};
bool f(C c1, C c2) { return c1==c2 ;}
```

Now inspect modifications:

(1) `bool operator==(C other) const {  return a==other.a && b==other.b & c==other.c & d==other.d; }`    -> pessimization

(2) `bool operator==(C other) const {  return (==other.a & b==other.b && c==other.c & d==other.d; }`    -> no-op !

Why?    hint: try replacing int with `struct myInt` with opaque `operator==`.

(3) `int a,b,c,d,  e  ;`   -> pessimization!

Why?  hint: arguments are now too large to fit in registers

