Linear Algebra in R
========================================================
author: Tim Hoolihan
date: 2016-01-27

Linear Algebra
========================================================
> "Linear algebra is the branch of mathematics concerning vector
spaces and linear mappings between such spaces. It includes the
study of lines, planes, and subspaces, but is also concerned with properties common to all vector spaces."

*https://en.wikipedia.org/wiki/Linear_algebra*

Applications
========================================================
* Computer Animation
* Social Sciences (economics)
* Physics
* Topology
* Engineering
* Machine Learning
* ... and more

Optimization
========================================================
Most general purpose CPUs are optimized for matrix [1] operations.

The next couple of slides will discuss why that is. It applies to linear algebra structures in R. However, it also applies to vectors within data frames and other structures in R. In other words, these optimizations are why you should always prefer vector and matrix operations over loops.

*[1] note that vectors are 1-dimensional matrices*

Hardware Optimization
========================================================

When you can use matrix or vector operations, you will take advantage of any relevant cpu instructions that compute more efficiently.

For example:

> "Single instruction, multiple data (SIMD), is a class of parallel computers in Flynn's taxonomy. It describes computers with multiple processing elements that perform the same operation on multiple data points simultaneously. Thus, such machines exploit data level parallelism, but not concurrency"

*https://en.wikipedia.org/wiki/SIMD*

Hardware Optimization Example
=======================================================
Latest Intel Microarchitecture (Skylake) Supports:

>All models support: ***MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1, SSE4.2, AVX, AVX2, FMA3,*** Enhanced Intel SpeedStep Technology (EIST), Intel 64, XD bit (an NX bit implementation), Intel VT-x, Intel VT-d, Turbo Boost, AES-NI, Smart Cache, Intel TSX-NI.

*[Wikipedia](https://en.wikipedia.org/wiki/List_of_Intel_Core_i5_microprocessors#Skylake_microarchitecture_.286th_generation.29_2)*

Software Optimization
========================================================
> "BLAS (Basic Linear Algebra Subprograms) is a specification that prescribes a set of low-level routines for performing common linear algebra operations such as vector addition, scalar multiplication, dot products, linear combinations, and matrix multiplication."

*https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms*

Software Optimizations Continued
========================================================
Examples of R using BLAS Libraries

* Default R links to an internal BLAS library
  * This is the case for R in Windows
* R port for Mac OS on CRAN links to ATLAS[1]
* Microsoft R (was Revolution) is linked to Intel Math Kernel[2] Library (Intel MKL)

The default implementation does not, but both ATLAS and Intel MKL  parallelize across CPU Cores when possible.

[1] Atlas: http://math-atlas.sourceforge.net/

[2] Intel MKL: https://software.intel.com/en-us/intel-mkl/

Create a Vector
========================================================

```r
v1 = c(1,2,3)
v1
```

```
[1] 1 2 3
```

```r
v2 = 5:10
v2
```

```
[1]  5  6  7  8  9 10
```

```r
v3 = seq(0, 50, by = 5)
v3
```

```
 [1]  0  5 10 15 20 25 30 35 40 45 50
```

Vector Math
========================================================
Addition

```r
1:2 + 1
```

```
[1] 2 3
```

```r
c(1,2) + c(10,20)
```

```
[1] 11 22
```

```r
c(1,2) + c(10,20,30)
```

```
[1] 11 22 31
```

Vector Math continued
========================================================
Multiplication

```r
1:5 * 10
```

```
[1] 10 20 30 40 50
```

```r
c(1,2) * c(10,20)
```

```
[1] 10 40
```

```r
c(1,2) * c(10,20,30)
```

```
[1] 10 40 30
```

Create a Matrix
========================================================

```r
X = matrix(data = 1:4, ncol = 2)
X
```

```
     [,1] [,2]
[1,]    1    3
[2,]    2    4
```

```r
X2 = matrix(data = c(1,1,1,32,34,33), nrow = 2, byrow = TRUE)
X2
```

```
     [,1] [,2] [,3]
[1,]    1    1    1
[2,]   32   34   33
```

Matrix Math
========================================================
Addition to a scalar

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
+ 1
$$


```r
X + 1
```

```
     [,1] [,2]
[1,]    2    4
[2,]    3    5
```

Matrix Math continued
========================================================
Addition to a vector

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
+
\begin{pmatrix}
20 \\
30
\end{pmatrix}
$$


```r
X + c(20,30)
```

```
     [,1] [,2]
[1,]   21   23
[2,]   32   34
```

Matrix Math
========================================================
Addition to a matrix

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
+
\begin{bmatrix}
100 & 300 \\
200 & 400
\end{bmatrix}
$$


```r
X + matrix(seq(100,400,by = 100), ncol = 2)
```

```
     [,1] [,2]
[1,]  101  303
[2,]  202  404
```

Matrix Math
========================================================
Multiplication by a scalar

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\times 2
$$


```r
X * 2
```

```
     [,1] [,2]
[1,]    2    6
[2,]    4    8
```

Matrix Math continued
========================================================
Matrix Product (Normal Matrix Multiplication)

- Uses `%*%` operator in R
- Traditional matrix product
- Associative and distrubitive

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\times
\begin{bmatrix}
1 & 2 \\
1 & 2
\end{bmatrix}
=
\begin{bmatrix}
4 & 8 \\
6 & 12
\end{bmatrix}
$$

Matrix Math continued
========================================================
![matrix multiplication](mm.png)
*Creative Commons (https://en.wikipedia.org/wiki/Matrix_multiplication)*

Matrix Math continued
========================================================
Hadamard Product (Element-Wise Multiplication)

- Uses `*` operator in R
- 1 for 1 multiplication
- associative, distrubutive, and commutative

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\circ
\begin{bmatrix}
1 & 2 \\
1 & 2
\end{bmatrix}
=
\begin{bmatrix}
1 & 6 \\
2 & 8
\end{bmatrix}
$$

Matrix Math continued
========================================================
Warning

You usually want %*% when working with a matrix, and you will forget this.

In this sense, Matlab (or Octave) handles this better with * being the matrix product, and .* being the Hadamard product.

Matrix Math continued
========================================================
Matrix product with a vector

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\times
\begin{pmatrix}
20 \\ 30
\end{pmatrix}
$$


```r
X %*% c(20,30)
```

```
     [,1]
[1,]  110
[2,]  160
```

Matrix Math continued
========================================================
Hadamard product with a vector

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\circ
\begin{pmatrix}
20 \\ 30
\end{pmatrix}
$$


```r
X * c(20,30)
```

```
     [,1] [,2]
[1,]   20   60
[2,]   60  120
```

Matrix Math continued
========================================================
Matrix product by a matrix

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\times
\begin{bmatrix}
100 & 300 \\
200 & 400
\end{bmatrix}
$$


```r
X %*% matrix(seq(100,400,by = 100), ncol = 2)
```

```
     [,1] [,2]
[1,]  700 1500
[2,] 1000 2200
```

Matrix Math continued
========================================================
Hadamard product by a matrix

$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\circ
\begin{bmatrix}
100 & 300 \\
200 & 400
\end{bmatrix}
$$


```r
X * matrix(seq(100,400,by = 100), ncol = 2)
```

```
     [,1] [,2]
[1,]  100  900
[2,]  400 1600
```

Identity Matrix
========================================================
- n x n (must by square) matrix

$$
\begin{bmatrix}
1 & 4 & 7 \\
2 & 5 & 8 \\
3 & 6 & 9
\end{bmatrix}
\times
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}
=
\begin{bmatrix}
1 & 4 & 7 \\
2 & 5 & 8 \\
3 & 6 & 9
\end{bmatrix}
$$


```r
matrix(1:9, ncol = 3) %*% diag(3)
```

```
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
```

Ones Matrix
========================================================
$$
\begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}
$$


```r
matrix(1, nrow = 2, ncol = 3)
```

```
     [,1] [,2] [,3]
[1,]    1    1    1
[2,]    1    1    1
```

Transpose
========================================================
Columns become rows

$$
\begin{bmatrix}
1 & 4 \\
2 & 5 \\
3 & 6
\end{bmatrix}'
=
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6
\end{bmatrix}
$$

```r
X3 = matrix(1:6, nrow = 3)
t(X3)
```

```
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
```

Inverse
========================================================
$$
\begin{bmatrix}
1 & 3 \\
2 & 4
\end{bmatrix}
\times
\begin{bmatrix}
-2 & 1.5 \\
1 & -0.5
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
$$


```r
library(Matrix)
solve(X)
```

```
     [,1] [,2]
[1,]   -2  1.5
[2,]    1 -0.5
```

```r
X %*% solve(X)
```

```
     [,1] [,2]
[1,]    1    0
[2,]    0    1
```

Pseudo Inverse
========================================================
Moore-Penrose inverse

Best-fit when no solution exists


```r
library(MASS)
X5 = matrix(c(2,3), 2, 2)
ginv(X5)
```

```
           [,1]      [,2]
[1,] 0.07692308 0.1153846
[2,] 0.07692308 0.1153846
```

```r
X5 %*% ginv(X5)
```

```
          [,1]      [,2]
[1,] 0.3076923 0.4615385
[2,] 0.4615385 0.6923077
```

Name Dimensions
========================================================


```r
housing = matrix(c(1,1,1,1,2,4,800,1200,2200),
                 ncol = 3,
                 dimnames = list(
                   c('studio',
                     'starter',
                     'family'),
                   c('constant',
                     'beds', 'sqft')))
housing
```

```
        constant beds sqft
studio         1    1  800
starter        1    2 1200
family         1    4 2200
```

Practical Example
========================================================

```r
pricing_models = matrix(c(10000,20000,
                          5000,10000,
                          90,95),
                        ncol = 2,
                        byrow = TRUE,
                        dimnames =
                          list(c('base','bed','sqft'), NULL))
pricing_models
```

```
      [,1]  [,2]
base 10000 20000
bed   5000 10000
sqft    90    95
```

Practical Example continued
========================================================
Consider the simplicity (and computational efficiency) vs a loop approach


```r
housing %*% pricing_models
```

```
          [,1]   [,2]
studio   87000 106000
starter 128000 154000
family  228000 269000
```

Normal Equation
========================================================
* $\theta$ is the line of best fit
* X is the data
* y is the vector of outputs
* $X^{-1}$ is the inverse of a matrix

$$
\theta = (X' \times X)^{-1} \times X' \times y
$$

https://en.wikipedia.org/wiki/Linear_least_squares_(mathematics)

Normal Equation in R
========================================================
$$
\theta = (X' \times X)^{-1} \times X' \times y
$$


```r
actuals = c(75000, 120000, 180000)

theta = ginv(t(housing) %*% housing) %*% t(housing) %*% actuals

dimnames(theta) <- list(c('base', 'bed', 'sqft'),NULL)
theta
```

```
             [,1]
base  16067.76296
bed  -10302.48758
sqft     94.81868
```

Normal Equation Applied
========================================================
Known Actuals


```r
housing %*% theta
```

```
             [,1]
studio   81620.22
starter 109245.20
family  183458.90
```

Normal Equation Applied
========================================================
Prediction

```r
mansion = c(1, bed = 10, sqft = 8000)
mansion %*% theta
```

```
         [,1]
[1,] 671592.3
```

Resources and Additional Sources
========================================================
* https://cran.r-project.org/web/packages/MASS/index.html
* https://cran.r-project.org/web/packages/Matrix/index.html
* http://www.r-bloggers.com/faster-r-through-better-blas/
* http://blog.revolutionanalytics.com/2010/06/performance-benefits-of-multithreaded-r.html
* https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms
* https://en.wikipedia.org/wiki/Linear_algebra
* https://support.rstudio.com/hc/en-us/articles/200486468-Authoring-R-Presentations
