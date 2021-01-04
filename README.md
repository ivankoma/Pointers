# Pointers in C/C++

These examples are based on [this](https://www.youtube.com/playlist?list=PL2_aWCzGMAwLZp6LMUKI3cc7pgGsasm2_) playlist, but are more readable and enhanced with many more examples.

> Pointers are variables that store address of another variable.

|Memory segment name|What is stored here|Memory size|
|-|-|-|
|Heap|`{int *p = new int;}`|Not fixed in size. Can grow.|
|Stack|Local variables `{int a;}`.<br>Functions calls.|Fixed.|
|Static/Global|`static int a;`|Fixed. |
|Code| All sequential instructions are kept in memory.<br>`ASM_INSTRUCTION_1; ASM_INSTRUCTION_2; ASM_INSTRUCTION_3...` |Fixed.|

- When a variable is declared as **static**, space for it gets allocated for the lifetime of the program. Even if the function is called multiple times, space for the static variable is allocated only once and the value of variable in the previous call gets carried through the next function call.
- **Global** variables can be accessed and modified anywhere in the program.

```cpp
int main() {
    int var = 5;
    int* pVar = &var;

    cout << "a\t" << var << endl;;
    cout << "*p\t" << *pVar << endl;
    cout << "&a\t" << &var << endl;;
    cout << "p\t" << pVar << endl;
    return 0;
}
```

```cpp
a       5
*p      5
&a      008FF7F8
p       008FF7F8
```

<br>

```cpp
int main() {
    int array[3] = { 100,200,300 };
    int* pArray = array;
    for (int i = 0; i < 3; i++) {
        cout << "// " << &array[i] << endl;
    }
    cout << endl;

    cout << "pArray\t\t" << pArray << endl;
    cout << "*pArray\t\t" << *pArray << endl;
    cout << "&pArray\t\t" << &pArray << endl << endl;

    cout << "array\t\t" << array << endl;
    cout << "*array\t\t" << *array << endl;
    cout << "&array\t\t" << &array << endl << endl;

    cout << "pArray[1]\t" << pArray[1] << endl;
    cout << "*pArray[1]\t" << "operand of '*' must be a pointer" << endl;
    cout << "&pArray[1]\t" << &pArray[1] << endl << endl;

    cout << "array[1]\t" << array[1] << endl;
    cout << "*array[1]\t" << "operand of '*' must be a pointer" << endl;
    cout << "&array[1]\t" << &array[1] << endl << endl;;

    cout << "array+1\t\t" << array + 1 << endl;
    cout << "pArray+1\t" << pArray + 1 << endl;
    cout << "*array+1\t" << *array + 1 << endl;
    cout << "*pArray+1\t" << *pArray + 1 << endl;
    cout << "*(array+1)\t" << *(array + 1) << endl;
    cout << "*(pArray+1)\t" << *(pArray + 1) << endl;
    return 0;
}
```

```cpp
// 009EF7E8
// 009EF7EC
// 009EF7F0

pArray          009EF7E8
*pArray         100
&pArray         009EF7DC

array           009EF7E8
*array          100
&array          009EF7E8

pArray[1]       200
*pArray[1]      operand of '*' must be a pointer
&pArray[1]      009EF7EC

array[1]        200
*array[1]       operand of '*' must be a pointer
&array[1]       009EF7EC

array+1         009EF7EC
pArray+1        009EF7EC
*array+1        101
*pArray+1       101
*(array+1)      200
*(pArray+1)     200
```
---

## Pointer types

```cpp
int main() {
    cout << "sizeof(int)\t\t" << sizeof(int) << endl;
    cout << "sizeof(float)\t" << sizeof(float) << endl;
    cout << "sizeof(double)\t" << sizeof(double) << endl;
    cout << "sizeof(char)\t" << sizeof(char) << endl;
    return 0;
}
```

Output:

```cpp
sizeof(int)		4
sizeof(float)	4
sizeof(double)	8
sizeof(char)	1
```

```cpp
int main() {
    int a = 1026;
    int* pa = &a;
    cout << a << " = 00000000 00000000 00000100 00000010\n----" << endl;
    cout << "pa\t\t" << pa << "\t*pa\t\t" << *pa << endl;
    cout << "pa+1\t\t" << pa + 1 << "\t*(pa+1)\t\t" << *(pa + 1) << "\t(garbage value)" << endl;
    cout << "----" << endl;
    char* pc = (char*)pa;   // typecasting
    //char *pc = pa;        // Incompatible pointer types assigning to 'char *' from 'int *'
    cout << "(int*)pc\t" << (int*)pc << "\t(int)*pc\t" << (int)*pc << "\t\t(00000010)" << endl;
    cout << "(int*)pc+1\t" << (int*)pc + 1 << "\t(int)*(pc+1)\t" << (int)*(pc + 1) << "\t\t(00000100)" << endl;
    return 0;
}
```

Output:

```cpp
1026 = 00000000 00000000 00000100 00000010
----
pa              001FF8F0        *pa             1026
pa+1            001FF8F4        *(pa+1)         -858993460      (garbage value)
----
(int*)pc        001FF8F0        (int)*pc        2               (00000010)
(int*)pc+1      001FF8F4        (int)*(pc+1)    4               (00000100)
```

---

## Pointers to pointers

```cpp
int main() {
    int a = 5;
    cout << "//a = " << a << endl;
    cout << "//&a = " << &a << endl;
    cout << endl;
    int* pA = &a;
    cout << "//int* pA = &a;" << endl;
    cout << "*pA" << "\t" << *pA << endl;
    cout << "pA" << "\t" << pA << endl;
    cout << "&pA" << "\t" << &pA << endl;
    cout << endl;
    int** ppA = &pA;
    cout << "//int** ppA = &pA;" << endl;
    cout << "**ppA" << "\t" << **ppA << endl;
    cout << "*ppA" << "\t" << *ppA << endl;
    cout << "ppA" << "\t" << ppA << endl;
    cout << "&ppA" << "\t" << &ppA << endl;
    cout << endl;
    int*** pppA = &ppA;
    ***pppA = 10;
    cout << "//***pppA = 10;" << endl;
    cout << "a" << "\t" << a << endl;
    **ppA = *pA * 3;
    cout << "//**ppA = *pA * 3;" << endl;
    cout << "a" << "\t" << a << endl;
    cout << endl;
    return 0;
}
```

Output:

```cpp
//a = 5
//&a = 012FFA10

//int* pA = &a;
*pA     5
pA      012FFA10
&pA     012FFA04

//int** ppA = &pA;
**ppA   5
*ppA    012FFA10
ppA     012FFA04
&ppA    012FF9F8

//***pppA = 10;
a       10
//**ppA = *pA * 3;
a       30
```

---

## Pointer as Function Arguments (Call by Reference)

```cpp
void increment(int *p){
    *p = *p + 1;
}

int main() {
    int a = 0;
    cout << a << endl;
    increment(&a);
    cout << a << endl;
    return 0;
}
```

```cpp
0
1
```

---

## Pointers and 1D arrays

```cpp
int main() {
    const int N = 3;
    int array[N] = { 100,200,300 };
    int* pArray = array;
    cout << "//int array[N] = { 100,200,300 };\n//int* pArray = array;" << endl;
    cout << "*array\t\t" << *array << "\t&array[0]\t" << &array[0];
    cout << " //array (same)" << endl;
    for (int i = 0; i < N; i++) {
        cout << "array[" << i << "]\t" << array[i] << "\t&array[" << i << "]\t" << &array[i] << endl;
        cout << "*(pArray+" << i << ")\t" << *(pArray + i) << "\tpArray+" << i << "\t" << pArray + i << endl;
        cout << endl;
    }
    return 0;
}
```

```cpp
//int array[N] = { 100,200,300 };
//int* pArray = array;
*array          100     &array[0]       0095F7E0 //array (same)
array[0]        100     &array[0]       0095F7E0
*(pArray+0)     100     pArray+0        0095F7E0

array[1]        200     &array[1]       0095F7E4
*(pArray+1)     200     pArray+1        0095F7E4

array[2]        300     &array[2]       0095F7E8
*(pArray+2)     300     pArray+2        0095F7E8
```

### Arrays as function arguments

```cpp
void doubleValues(int A[], int n) { // `int A[]` is the same as `int *A`
    cout << "(func) sizeof(A)\t" << sizeof(A) << endl;
    cout << "(func) sizeof(A[0])\t" << sizeof(A[0]) << endl;
    for (int i = 0; i < n; i++) {
        A[i] *= 2;
    }
}

int main() {
    const int N = 5;
    int array[N] = { 1,2,3,4,5 };
    for (int i = 0; i < N; i++) cout << array[i] << " ";
    cout << "\n(main) sizeof(array)\t" << sizeof(array) << endl;
    cout << "(main) sizeof(array[0])\t" << sizeof(array[0]) << endl;
    doubleValues(array, sizeof(array) / sizeof(array[0]));
    for (int i = 0; i < N; i++) cout << array[i] << " ";
    return 0;
}
```

```cpp
1 2 3 4 5
(main) sizeof(array)    20
(main) sizeof(array[0]) 4
(func) sizeof(A)        4
(func) sizeof(A[0])     4
2 4 6 8 10
```

### Character arrays and pointers

```cpp
#include <cstring>

int main() {
    char CA[5] = "ABCD"; // Cannot be CA[4] because of the NULL (\0) character
    cout << "\n" << CA << endl;
    cout << "strlen(CA)\t" << strlen(CA) << endl;
    cout << "sizeof(CA)\t" << sizeof(CA) << endl;
    //CA = "red"; //Error: expression must be a modifiable lvalue. Array type 'char [5]' is not assignable
    return 0;
}

```

```cpp
ABCD
strlen(CA)	4
sizeof(CA)	5
```

<br>

```cpp
int main() {
    char CA[5] = "ABCD";
    char* pCA = CA;
    cout << pCA << "\t// pCA" << endl;
    cout << *pCA << "\t// *pCA" << endl;
    cout << *(pCA + 1) << "\t// *(pCA + 1)" << endl;
    cout << *(&pCA[1]) << "\t// *(&pCA[1])"<< endl;
    return 0;
}
```

```cpp
ABCD    // pCA
A       // *pCA
B       // *(pCA + 1)
B       // *(&pCA[1])
```

<br>

```cpp
void print(const char *c){
    while(*c != '\0'){
        cout << *c;
        c++;
    }
}

int main() {
    char CA[5] = "ABCD";
    print(CA);
    return 0;
}
```

```cpp
ABCD
```

---

## Pointers and 2D arrays

```cpp
B[i][j] = *(B[i] + j)
        = *(*(B+i) + j);
```


<br>

```cpp
void printViaPointerToArray(int(*p)[3], const int ROWS, const int COLS) {
    for (int row = 0; row < ROWS; row++) {
        cout << "[";
        for (int col = 0; col < COLS; col++) {
            cout << p[row][col];
            if (col != COLS - 1) cout << " ";
        }
        cout << "]\t[";
        for (int col = 0; col < COLS; col++) {
            cout << &p[row][col];
            if (col != COLS - 1) cout << " ";
        }
        cout << "]" << endl;
    }
}

void printViaPointer(int* p, const int ROWS = 1, const int COLS = 1, bool addr = false) {
    for (int row = 0; row < ROWS; row++) {
        cout << "[";
        for (int col = 0; col < COLS; col++) {
            cout << *(p + row * COLS + col);
            if (col != COLS - 1) cout << " ";
        }
        cout << "]\t[";
        for (int col = 0; col < COLS; col++) {
            cout << p + row * COLS + col;
            if (col != COLS - 1) cout << " ";
        }
        cout << "]" << endl;
    }
}
```

```cpp
int main() {
    const int ROWS = 2, COLS = 3;
    int B[ROWS][COLS] = { 100, 200, 300, 400, 500, 600 }; // Each element is not an integer but an array of 3 integers.
    // 100 200 300    1D array of 3 integers.
    // 400 500 600    1D array of 3 integers. 
    printViaPointerToArray(B, ROWS, COLS);
    cout << endl;
    //printViaPointer(&B[0][0], ROWS, COLS);
    //cout << endl;

    int *p, value;
    int (*p1D)[COLS];
    int (*p2D)[ROWS][COLS];
    cout << "int *p, value;" << endl << "int (*p1D)[COLS];" << endl << "int (*p2D)[ROWS][COLS];" << endl;
    cout << "B" << "\t\t" << B << "\t" << "p1D = B;" << endl;
    p1D = B;

    cout << "&B[0]" << "\t\t" << &B[0] << "\t" << "p1D = &B[0];" << endl;
    p1D = &B[0];
    cout << "&B" << "\t\t" << &B << "\t" << "p2D = &B;" << endl;
    p2D = &B;
    cout << "*B" << "\t\t" << *B << "\t" << "p = *B;" << endl;
    p = *B;
    cout << "B[0]" << "\t\t" << B[0] << "\t" << "p = B[0];" << endl;
    p = B[0];
    cout << "*B[0]" << "\t\t" << *B[0] << "\t\t" << "value = *B[0];" << endl;
    value = *B[0];
    cout << endl;

    cout << "B+1" << "\t\t" << B+1 << "\t" << "p1D = B + 1;" << endl;
    p1D = B + 1;
    cout << "*(B+1)" << "\t\t" << *(B+1) << "\t" << "p = *(B+1);" << endl;
    p = *(B+1);
    cout << "&B[1][0]" << "\t" << &B[1][0] << "\t" << "p = &B[1][0];" << endl;
    p = &B[1][0];
    cout << "*(B+1)+2" << "\t" << *(B+1)+2 << "\t" << "p = *(B + 1) + 2;" << endl;
    p = *(B + 1) + 2;
    cout << "B[1]+2" << "\t\t" << B[1] + 2 << "\t" << "p = B[1] + 2;" << endl;
    p = B[1] + 2;
    cout << "&B[1][2]" << "\t" << &B[1][2] << "\t" << "p = &B[1][2];" << endl;
    p = &B[1][2];
    cout << endl;
    
    value = *B[1];
    cout << "*B[1]" << "\t\t" << value << "\t\t" << "value = *B[1];" << endl;
    
    value = *(*B + 1);
    cout << "*(*B+1)" << "\t\t" << value << "\t\t" << "value = *(*B + 1);" << endl;
    
    cout << "*B[0][1]" << "\t" << "operand of '*' must be a pointer" << endl;
    //
    cout << endl;
    value = B[1][2];
    cout << "B[1][2]" << "\t\t" << value << "\t\t" << "value = B[1][2];" << endl;
    value = *(B[1] + 2);
    cout << "*(B[1]+2)" << "\t" << value << "\t\t" << "value = *(B[1] + 2);" << endl;
    value = *(*(B + 1) + 2);
    cout << "*(*(B+1)+2)" << "\t" << value << "\t\t" << "value = *(*(B + 1) + 2);" << endl;
    
    return 0;
}
```

```cpp
[100 200 300]   [00EFF6C0 00EFF6C4 00EFF6C8]
[400 500 600]   [00EFF6CC 00EFF6D0 00EFF6D4]

int *p, value;
int (*p1D)[COLS];
int (*p2D)[ROWS][COLS];
B               00EFF6C0        p1D = B;
&B[0]           00EFF6C0        p1D = &B[0];
&B              00EFF6C0        p2D = &B;
*B              00EFF6C0        p = *B;
B[0]            00EFF6C0        p = B[0];
*B[0]           100             value = *B[0];

B+1             00EFF6CC        p1D = B + 1;
*(B+1)          00EFF6CC        p = *(B+1);
&B[1][0]        00EFF6CC        p = &B[1][0];
*(B+1)+2        00EFF6D4        p = *(B + 1) + 2;
B[1]+2          00EFF6D4        p = B[1] + 2;
&B[1][2]        00EFF6D4        p = &B[1][2];

*B[1]           400             value = *B[1];
*(*B+1)         200             value = *(*B + 1);
*B[0][1]        operand of '*' must be a pointer

B[1][2]         600             value = B[1][2];
*(B[1]+2)       600             value = *(B[1] + 2);
*(*(B+1)+2)     600             value = *(*(B + 1) + 2);
```

---

## Pointers and multi-dimensional arrays

```cpp
void func1(int A[][2][2]){
    ...
} 

void func2(int (*A)[2][2]){
    ...
}

void main(){
    int A3D[ARRAYS][COL_SIZE][ROW_SIZE]={
                                            {
                                                {100,101,102,103},
                                                {110,111,112,113}
                                            },
                                            {
                                                {200,201,202,203},
                                                {210,211,212,213},
                                            },
                                            {
                                                {300,301,302,303},
                                                {310,311,312,313}
                                            },
                                        };
    func1(A3D);
    func2(A3D);
}
```

```cpp
int a[2][2][3] = {1,2,3,4,5,6,7,8,9,10,11,12};
int a[2][2][3] ={
                    {{1,2,3},{4,5,6}},
                    {{7,8,9},{10,11,12}}
                };
```

<br>

```cpp
void print2D(int* p2D, const int COL_SIZE, const int ROW_SIZE) {
    cout << ">>>> 2D >>>>" << endl;
    for (int col = 0; col < COL_SIZE; col++) {
        cout << "  [";
        for (int row = 0; row < ROW_SIZE; row++) {
            cout << p2D[col * ROW_SIZE + row];
            if (row != ROW_SIZE - 1) cout << " ";
        }
        cout << "]\n";
    }
    cout << "<<<< 2D <<<<" << endl << endl;
}

void print3D(int* p3D, const int ARRAYS, const int COL_SIZE, const int ROW_SIZE) {
    for (int array = 0; array < ARRAYS; array++) {
        for (int col = 0; col < COL_SIZE; col++) {
            cout << "[";
            for (int row = 0; row < ROW_SIZE; row++) {
                cout << p3D[array * ROW_SIZE * COL_SIZE + col * ROW_SIZE + row];
                if (row != ROW_SIZE - 1) cout << " ";
            }
            cout << "]\t";
            cout << "[";
            for (int row = 0; row < ROW_SIZE; row++) {
                cout << &p3D[array * ROW_SIZE * COL_SIZE + col * ROW_SIZE + row];
                if (row != ROW_SIZE - 1) cout << " ";
            }
            cout << "]\n";
        }
        cout << endl;
    }
}

void assignValue(int* p3D, const int ARRAYS, const int COL_SIZE, const int ROW_SIZE) {
    int i = 0;
    for (int array = 0; array < ARRAYS; array++) {
        for (int col = 0; col < COL_SIZE; col++) {
            for (int row = 0; row < ROW_SIZE; row++) {
                p3D[array * ROW_SIZE * COL_SIZE + col * ROW_SIZE + row] = i++;
            }
        }
    }
}
```

```cpp
int main() {
    const int ARRAYS = 3, COL_SIZE = 2, ROW_SIZE = 4;
    int value;
    int* p;
    int (*p1D)[ROW_SIZE];
    int (*p2D)[COL_SIZE][ROW_SIZE];
    int (*p3D)[ARRAYS][COL_SIZE][ROW_SIZE];
    int A3D[ARRAYS][COL_SIZE][ROW_SIZE] = {
                                        {
                                            {100,101,102,103},
                                            {110,111,112,113}
                                        },
                                        {
                                            {200,201,202,203},
                                            {210,211,212,213},
                                        },
                                        {
                                            {300,301,302,303},
                                            {310,311,312,313}
                                        },
    };
    //assignValue(&A3D[0][0][0], ARRAYS, COL_SIZE, ROW_SIZE);
    print3D(&A3D[0][0][0], ARRAYS, COL_SIZE, ROW_SIZE);

    cout << "&A3D" << "\t\t\t" << &A3D << "\t" << "p3D = &A3D;" << endl;
    p3D = &A3D;

    cout << "A3D" << "\t\t\t" << A3D << "\t" << "p2D = A3D;" << endl;
    p2D = A3D;

    cout << "&A3D[1]" << "\t\t\t" << &A3D[1] << "\t" << "p2D = &A3D[1];" << endl;
    p2D = &A3D[1];

    cout << "A3D[1]" << "\t\t\t" << A3D[1] << "\t" << "p1D = A3D[1];" << endl;
    p1D = A3D[1];

    cout << "&A3D[1][0]" << "\t\t" << &A3D[1][0] << "\t" << "p1D = &A3D[1][0];" << endl;
    p1D = &A3D[1][0];

    cout << "*A3D" << "\t\t\t" << *A3D << "\t" << "p1D = *A3D;" << endl;
    p1D = *A3D;

    cout << endl;
    // C[i][j][k] = *(C[i][j]+k) = *(*(C[i]+j)+k) = *(*(*(c+i)+j)+k)
    
    cout << "A3D[2][1][0]" << "\t\t" << A3D[2][1][0] << endl;
    value = A3D[2][1][0];
    cout << "*(A3D[2][1]+0)" << "\t\t" << *(A3D[2][1] + 0) << endl;
    cout << "*(*(A3D[2]+1)+0)" << "\t" << *(*(A3D[2] + 1) + 0) << endl;
    cout << "*(*(*(A3D+2)+1)+0)" << "\t" << *(*(*(A3D + 2) + 1) + 0) << endl;
    cout << endl;
    cout << "*(A3D[0][1]+1)" << "\t\t" << *(A3D[0][1] + 1) << endl;
    return 0;
}
```

```cpp
[100 101 102 103]       [0095FBD0 0095FBD4 0095FBD8 0095FBDC]
[110 111 112 113]       [0095FBE0 0095FBE4 0095FBE8 0095FBEC]

[200 201 202 203]       [0095FBF0 0095FBF4 0095FBF8 0095FBFC]
[210 211 212 213]       [0095FC00 0095FC04 0095FC08 0095FC0C]

[300 301 302 303]       [0095FC10 0095FC14 0095FC18 0095FC1C]
[310 311 312 313]       [0095FC20 0095FC24 0095FC28 0095FC2C]

&A3D                    0095FBD0        p3D = &A3D;
A3D                     0095FBD0        p2D = A3D;
&A3D[1]                 0095FBF0        p2D = &A3D[1];
A3D[1]                  0095FBF0        p1D = A3D[1];
&A3D[1][0]              0095FBF0        p1D = &A3D[1][0];
*A3D                    0095FBD0        p1D = *A3D;

A3D[2][1][0]            310
*(A3D[2][1]+0)          310
*(*(A3D[2]+1)+0)        310
*(*(*(A3D+2)+1)+0)      310

*(A3D[0][1]+1)          111
```

---

## Pointers and dynamic memory

```cpp
#include <iostream>
using namespace std;
int main () {
    int *pI = NULL;
    pI = new int;
    float *pF = new float(12.34);   // (defaultValue)
    int *pArray = new int[4];       //
    int *pArrayZero = new int[4](); // () will fill whole array with zeroes
    *pI = 5;

    cout << "*pI\t" << *pI << endl;
    cout << "*pF\t" << *pF << endl;
    if (!pArray){
        cout << "Allocation of memory failed\n";
    }else {
        cout << "pArray[0..4]\t\t";;
        for (int i = 0; i < 4; i++){
            cout << pArray[i] << " ";
        }
        cout << "\t(random garbage value)" << endl;
        cout << "pArrayZero[0..4]\t";;
        for (int i = 0; i < 4; i++){
            cout << pArrayZero[i] << " ";
        }
        cout << endl;
    }
    cout << "After deleting:" << endl;
    delete pI;
    if(pI) cout << "After deleting pI exists." << endl;
    delete pF;
    delete[] pArray;
    delete[] pArrayZero;
    //cout << "*p1\t\t\t" << *p1 << endl; // Undefined behavior. https://stackoverflow.com/questions/7827504/c-delete-pointer-issue-can-still-access-data
    pI = NULL, pF =NULL, pArray=NULL, pArrayZero=NULL; //     https://stackoverflow.com/questions/1931126/is-it-good-practice-to-null-a-pointer-after-deleting-it
    if(pI) cout << "After deleting and setting to NULL pI exists." << endl;
    //cout << "*pI\t\t\t" << *pI << endl; // Causes error.
    return 0;
}
```

```cpp
*pI	5
*pF	12.34
pArray[0..4]		1470232 1441984 1600680809 1953066581 	(random garbage value)
pArrayZero[0..4]	0 0 0 0 
After deleting:
After deleting pI exists.
```

---

## Returning a pointer from a function

```cpp
int* Add(int *a, int *b){
    int *sum = new int;
    *sum = *a + *b;
    return sum; // returns pointer stored on the heap
}

void Add(int *a, int *b, int **sum){
    **sum = *a + *b;
}

int main()
{
    int a = 1, b = 2;
    int* sum = Add(&a, &b);
    cout << a << "+" << b << "=" << *sum << endl;

    int x = 3, y = 5;
    int *z = new int;
    Add(&x, &y, &z);
    cout << x << "+" << y << "=" << *z << endl;
}
```

```cpp
1+2=3
3+5=8
```

---

## Function Pointers

```cpp
void Hello(){
    cout << "Hello." << endl;
}

int main() {
    void (*pfHello)();
    pfHello = Hello;
    pfHello();
    return 0;
}
```

```cpp
Hello.
```

### Function pointer with parameters

```cpp
#include <string>
void Hello(string name){
    cout << "Hello" << name << "." << endl;
}

int main() {
    void (*pfHello)(string);
    pfHello = Hello;
    pfHello("Michael");
    return 0;
}
```

```cpp
Hello Michael.
```

<br>

```cpp
float Add(int a, int b){
    return a + b + 0.5;
}

int main() {
    float (*pfAdd)(int, int); // If we don't type () like `float *pf(int, int)` it means that we are returning a pointer to float variable.
    float (*pfAdd2)(int, int);
    pfAdd = &Add;
    pfAdd2 = Add;   // Same 
    cout << "pfAdd(2,3)\t\t" << pfAdd(2,3) << endl;
    cout << "pfAdd(2,3)\t\t" << pfAdd(2,3) << endl;
    cout << "(*pfAdd2)(2,3)\t" << (*pfAdd2)(2,3) << "\t(same)" << endl;
    return 0;
}
```

```cpp
pfAdd(2,3)		5.5
(*pfAdd2)(2,3)	5.5	same
```

### Function Pointers Callbacks

Function pointers can be passed as arguments to functions.

```cpp
void B(){
    // This function can be called-back by A thought the reference, through the function pointer.
    cout << "B()" << endl;
}

void A(void (*pf)()){ // Function pointer as argument.
    cout << "A(pf)" << endl;
    pf(); //Call-back function that "pf" points to.
}

int main() {
    //void (*pfB)() = B;
    //A(pfB);
    A(B); // Same.
    return 0;
}
```

Output:

```cpp
A(pf)
B()
```

Sorting example:<br>

```cpp
bool isGreater(int a, int b){
    if(a>=b){
        return true;
    }else{
        return false;
    }
}
#include <math.h>
bool isGreaterAbsolute(int a, int b){
    if(abs(a)>=abs(b)){
        return true;
    }else{
        return false;
    }
}

void BubbleSort(int *A, const int N, bool (*compare)(int, int)){
    int i,j, temp;
    for(int i=0;i<N;i++){
        for(int j=0;j<N-1;j++){
            if(compare(A[j],A[j+1])){
                temp = A[j];
                A[j] = A[j+1];
                A[j+1] = temp;
            }
        }
    }
}

int main() {
    int A[] = {3,-2,1,-5,-6,4};
    BubbleSort(A,6, isGreaterAbsolute);
    for(int i=0;i<6;i++) cout << A[i] << "\t";
    cout << endl;
    BubbleSort(A,6, isGreater);
    for(int i=0;i<6;i++) cout << A[i] << "\t";
    return 0;
}
```

```
 1	-2	 3	 4	-5	-6	
-6	-5	-2	 1	 3	 4	
```
