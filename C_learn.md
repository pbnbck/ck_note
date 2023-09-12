### 指针

**`int *p`**：

- 表示 `p` 是一个指向整数（`int`）的指针。它可以用来存储一个整数变量的地址。

- 通过 `*p` 可以访问指针 `p` 所指向的整数变量的值。

- 例如

  ```
  int x = 42;
  int *p = &x; // p 存储了 x 的地址
  
  // 通过指针 p 访问 x 的值
  printf("x = %d\n", *p); // 输出 x 的值，即 42
  
  // 通过指针 p 修改 x 的值
  *p = 99;
  printf("x = %d\n", x); // 输出修改后的 x 的值，即 99
  ```

  ```
  int *p;
  p = &a;	  //这种方式正确
  printf("p = %d\n",p);结果：p = 6618636，变量p存放的a的地址
  ```

`&x`：这是取地址运算符，用于获取整数变量 `x` 的内存地址。`&` 符号后跟一个变量名，表示获取该变量的地址。

`&x` 的结果是 `x` 变量的地址，因此 `int *p` 中的 `p` 被赋值为 `x` 变量的地址。

**`int \**p`**

 表示 `p` 是一个指向指向整数的指针的指针。也就是说，`p` 存储了指向整数指针的地址。

```
int x = 10;
int *px = &x;
int **p = &px; // p 存储了指向整数指针 px 的地址
printf("%d", **p); // 输出 10，通过 **p 访问 x 的值
```

```
int rows = 3, cols = 4;
int **matrix;
matrix = (int **)malloc(rows * sizeof(int *));
for (int i = 0; i < rows; i++) {
    matrix[i] = (int *)malloc(cols * sizeof(int));
}
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        // 使用 matrix[i][j] 来访问矩阵的元素
    }
}
```

1. `int rows = 3, cols = 4;`：这里定义了矩阵的行数为 3，列数为 4。

2. `int **matrix;`：这是一个指向指针的指针，它将用于存储整数指针的指针数组。这是因为你正在创建一个指向整数的指针数组，每个元素指向一个整数数组，形成二维矩阵。

3. `matrix = (int **)malloc(rows * sizeof(int *));`：这一行使用 `malloc` 函数分配内存，以容纳指向整数指针的指针数组。具体来说：

   - `rows` 表示指针数组的元素数量，也就是矩阵的行数。
   - `sizeof(int *)` 表示一个整数指针的大小。

   这行代码分配了足够的内存以容纳 3 个整数指针，并将其地址赋值给 `matrix`。此时，`matrix` 成为了一个指向指针的指针，可以用于存储整数指针的地址。

4. `for (int i = 0; i < rows; i++) {`：这是一个循环，遍历每一行。

5. `matrix[i] = (int *)malloc(cols * sizeof(int));`：在循环中，这行代码分配了内存以容纳整数数组，也就是矩阵的一行。具体来说：

   - `cols` 表示整数数组的元素数量，也就是矩阵的列数。
   - `sizeof(int)` 表示一个整数的大小。

   这行代码分配了足够的内存以容纳 4 个整数，并将其地址赋值给 `matrix[i]`。因此，`matrix[i]` 现在是一个指向整数的指针，表示矩阵的第 `i` 行。

最终，你成功创建了一个 3 行 4 列的二维整数数组（矩阵）。你可以使用 `matrix[i][j]` 访问其中的元素，其中 `i` 表示行索引，`j` 表示列索引。



结构体指针

```
int inet_add_protocol(const struct net_protocol *prot, unsigned char protocol)
```

- `int` 是函数的返回类型，表示该函数将返回一个整数值。

- `inet_add_protocol` 是函数的名称，这是您可以使用来调用该函数的标识符。

- ```
  (const struct net_protocol *prot, unsigned char protocol)
  ```

   是函数的参数列表。这里有两个参数：

  - `const struct net_protocol *prot` 是一个指向结构体的指针，这个结构体的类型是 `struct net_protocol`，并且在函数内部被视为常量（不能被修改）。
  - `unsigned char protocol` 是一个无符号字符，它是另一个函数参数。

当在函数中使用 `*prot` 时，实际上是在引用指向结构体的指针，而不是直接引用结构体本身。这允许您通过指针来访问和操作结构体的成员，而不是复制整个结构体，从而节省了内存和提高了效率。