---
layout: post
title: "Port NMatrix to JRuby (GSoC2016)"
date: 2016-10-24 12:00
comments: true
author: Prasun Anand
categories: [GSOC,GSOC2016,NMatrix,Linear Algebra,JRuby]
---

## **Introduction**

I worked on "Port NMatrix to JRuby" in the context of the Google Summer of Code (GSoC) 2016 and I am pleased to announce that **Nmatrix can now be used in JRuby**.

With JRuby NMatrix, a linear algebra library, wraps Apache Commons Maths for its core functionalities. NMatrix supports dense matrices for double and a Ruby object data type. The performance of JRuby with Apache Commons Maths is outstanding (see below for performance comparisons) even without making use of JRuby threading capabilities.

I have also ported the [mixed-models](https://github.com/agisga/mixed_models) gem to JRuby which uses NMatrix heavily at its core. This allowed us to test NMatrix-JRuby with real-life data.

This blog post summarizes my work on the project with Sciruby, and reports the final status of the project.

## **GSoC application**

The original GSoC proposal, plan and application can be found [here](https://github.com/prasunanand/resume/blob/master/gsoc2016_application.md).

## **Code Commits**

[https://github.com/prasunanand/nmatrix/commits/jruby_port](https://github.com/prasunanand/nmatrix/commits/jruby_port)


## **Performance**
We have benchmarked some of the NMatrix functionalities. The following plots compare the performance between NMatrix-JRuby, NMatrix-MRI and NMatrix-MRI using LAPACK/ATLAS libraries.


Note:

1. Addition and subtraction are not supported by LAPACK/ATLAS.
2. NMatrix-MRI relies on LAPACK/ATLAS for calculating determinants and LU Decomposition(lud).


![Alt Matrix Addition](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/plots/add.png?raw=true "Fig.3. Matrix Addition")
![Alt Matrix Subtraction](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/plots/subt.png?raw=true "Fig.4. Matrix Subtraction")
![Alt Matrix Multiplication](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/plots/mult.png?raw=true "Fig.5. Matrix Multiplication")
![Alt Gamma operator](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/plots/gamma.png?raw=true "Fig.6. Gamma Operator")
![Alt Determinant](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/plots/det.png?raw=true "Fig.7. Determinant")
![Alt LU Facorization](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/plots/lud.png?raw=true "Fig.8. LU Facorization")

**Result:**

1. For, two dimensional matrices, NMatrix-JRuby is currently slower than NMatrix-MRI for matrix multiplication and matrix decomposition functionalities(calculating determinant and factorizing a matrix). NMatrix-JRuby is faster than NMatrix-MRI for other functionalities of a two dimensional matrix like addition, subtraction, trigonometric operations, etc.

2. NMatrix-JRuby is a clear winner when we are working with matrices of arbitrary dimensions.


## **Storing n-dimensional matrices as flat arrays**

The major components of NMatrix are shape, elements, dtype and stype. Any nmatrix when initialized, stores the elements as a flat array. ArrayRealVector class is used to store the elements.

`@s` stores elements, `@shape` stores the shape of the NMatrix, while `@dtype` and `@stype` store the data type and storage type respectively. Currently, we have nmatrix-jruby implemented only for double and Ruby object data type.


NMatrix-MRI uses `@s` which is an object containing elements, stride and offset (as in C, we need to deal with the memory allocation for the arrays).

![NMatrix](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/nmatrix.png?raw=true "Fig.1. NMatrix")

## **Slicing and Rank**

Implementing slicing was the toughest part of NMatrix-JRuby implementation.
`NMatrix@s` stores the elements of a matrix as a flat_array. The elements along any dimension are accessed with the help of the stride. `NMatrix#get_stride` calculates the stride with the help of the dimension and shape and returns an array.

```ruby
def get_stride(nmatrix)
  stride = Array.new()
  (0...nmatrix.dim).each do |i|
    stride[i] = 1;
    (i+1...dim).each do |j|
      stride[i] *= nmatrix.shape[j]
    end
  end
  stride
end
```

`NMatrix#[]` and `NMatrix#[]=` are thus able to read and write the elements of a matrix. NMatrix#MRI uses the `@s` object which stores the stride when the nmatrix is initialized.

`NMatrix#[]` calls the `#xslice` operator which calls  `#get_slice` operator that use the stride to determine whether we are accessing a single element or multiple elements. If  there are multiple elements, `#dense_storage_get` returns an NMatrix object with the elements along the dimension.

NMatrix-MRI differs from NMatrix-JRuby implementation as it makes sure that memory is properly utilized as the memory needs to be properly **garbage collected**.

```ruby
def xslice(args)
  result = nil

  if self.dim < args.length
    raise(ArgumentError,"wrong number of arguments\
       (#{args} for #{effective_dim(self)})")
  else
    result = Array.new()
    slice = get_slice(@dim, args, @shape)
    stride = get_stride(self)
    if slice[:single]
      if (@dtype == :object)
        result = @s[dense_storage_get(slice,stride)]
      else
        s = @s.toArray().to_a
        result = @s.getEntry(dense_storage_get(slice,stride))
      end
    else
      result = dense_storage_get(slice,stride)
    end
  end
  return result
end
```

`NMatrix#[]=` calls the `#dense_storage_set` operator which calls `#get_slice` operator that use the stride to find out whether we are accessing a single element or multiple elements. If there are multiple
 elements `#set_slice` recursively sets the elements of the matrix then returns an NMatrix object with the elements along the dimension.

```ruby
def dense_storage_set(slice, right)
    stride = get_stride(self)
    v_size = 1

    if right.is_a?(NMatrix)
      right = right.s.toArray.to_a
    end

    if(right.is_a?(Array))
      v_size = right.length
      v = right
      if (dtype == :object)
        # nm_register_values(reinterpret_cast<VALUE*>(v), v_size)
      end

      (0...v_size).each do |m|
        v[m] = right[m]
      end
    else
      v = [right]
      if (@dtype == :object)
        # nm_register_values(reinterpret_cast<VALUE*>(v), v_size)
      end
    end
    if(slice[:single])
      # reinterpret_cast<D*>(s->elements)[nm_dense_storage_pos(s, slice->coords)] = v;
      pos = dense_storage_pos(slice[:coords],stride)
      if @dtype == :object
        @s[pos] = v[0]
      else
        @s.setEntry(pos, v[0])
      end
    else
      v_offset = 0
      dest = {}
      dest[:stride] = get_stride(self)
      dest[:shape] = shape
      # dest[:elements] = @s.toArray().to_a
      dense_pos = dense_storage_pos(slice[:coords],stride)
      slice_set(dest, slice[:lengths], dense_pos, 0, v, v_size, v_offset)
    end
  end
```

## **Enumerators**

NMatrix-MRI uses the C code for enumerating the elements of a matrix. However, the NMatrix-JRuby uses pure Ruby code. Currently, all the enumerators for dense matrices with real data-type have been implemented and they are properly functional. Enumerators for objects have not been currently implemented.

```ruby
def each_with_indices
  nmatrix = create_dummy_nmatrix
  stride = get_stride(self)
  offset = 0
  #Create indices and initialize them to zero
  coords = Array.new(dim){ 0 }

  shape_copy =  Array.new(dim)
  (0...size).each do |k|
    dense_storage_coords(nmatrix, k, coords, stride, offset)
    slice_index = dense_storage_pos(coords,stride)
    ary = Array.new
    if (@dtype == :object)
      ary << self.s[slice_index]
    else
      ary << self.s.toArray.to_a[slice_index]
    end
    (0...dim).each do |p|
      ary << coords[p]
    end

    # yield the array which now consists of the value and the indices
    yield(ary)
  end if block_given?
  nmatrix.s = @s

  return nmatrix
 end
```

## **Two-Dimensional Matrices**

Linear algebra is mostly about two-dimensional matrices. In NMatrix, when performing calculations in a two-dimensional matrix, a flat array is converted to a two-dimensional matrix. A two-dimensional matrix is stored as a BlockRealMatix or Array2DRowRealMatrix.
**Each of them has its own advantages.**

**Getting a 2D-matrix**

![Alt Getting a 2D-matrix](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/matrixGenerate.png?raw=true "Fig.2. Getting a 2D-matrix")

```java
public class MatrixGenerator
{
  public static double[][] getMatrixDouble(double[] array, int row, int col)
  {
    double[][] matrix = new double[row][col];
    for (int index=0, i=0; i < row ; i++){
        for (int j=0; j < col; j++){
            matrix[i][j]= array[index];
            index++;
        }
    }
    return matrix;
  }
}
```

**Flat a 2D-matrix**

```java
public class ArrayGenerator
{
  public static double[] getArrayDouble(double[][] matrix, int row, int col)
  {
    double[] array = new double[row * col];
    for (int index=0, i=0; i < row ; i++){
        for (int j=0; j < col; j++){
            array[index] = matrix[i][j];
            index++;
        }
    }
    return array;
  }
}
```

**Why use Java method instead of Ruby method?**

1. Memory Usage and Garbage Collection: A scientific library is memory intensive and hence, every step counts.
 JRuby interpreter doesn't need to dynamically guess the data type and uses less memory, i.e around one-tenth of it. If the memory is properly utilized; when the GC kicks in, it has to clear less used memory space and improves the speed.

2. Speed: Using java method greatly improves the speed; around 1000 times, when compared to using ruby method.


## **Operators**

All the operators from NMatrix-MRI have been implemented except moduli. The binary operators were easily implemented through Commons Math API and Java Math API.

```ruby
def +(other)
  result = create_dummy_nmatrix
  if (other.is_a?(NMatrix))
    #check dimension
    raise(ShapeError, "Cannot add matrices with different dimension")\
    if (@dim != other.dim)
    #check shape
    (0...dim).each do |i|
      raise(ShapeError, "Cannot add matrices with different shapes") \
      if (@shape[i] != other.shape[i])
    end
    result.s = @s.copy.add(other.s)
  else
    result.s = @s.copy.mapAddToSelf(other)
  end
  result
end
```

Trigonometric, Exponentiation and Log operators with a singular argument i.e. matrix elements have been implemented using `#mapToSelf` method that takes Univariate function as an argument. `#mapToSelf` maps every element of ArrayRealVector object to the Univariate operator, which is passed to it and returns `self` object.

```ruby
def sin
  result = create_dummy_nmatrix
  result.s = @s.copy.mapToSelf(Sin.new())
  result
end
```

NMatrix#method(arg) has been implemented using Bivariate functions provided by Commons-Maths API and Java Maths API.

```ruby
def gamma
  result = create_dummy_nmatrix
  result.s = ArrayRealVector.new MathHelper.gamma(@s.toArray)
  result
end
```

```java
import org.apache.commons.math3.special.Gamma;

public class MathHelper{
  ...
  public static double[] gamma(double[] arr){
    double[] result = new double[arr.length];
    for(int i = 0; i< arr.length; i++){
      result[i] = Gamma.gamma(arr[i]);
    }
    return result;
  }
  ...
}
```

## **Decomposition**

NMatrix-MRI relies on LAPACK and ATLAS for matrix decomposition and solving functionalities. Apache Commons Math provides a different set of API for decomposing a matrix and solving an equation. For example, `#potrf` and other LAPACK specific functions have not been implemented as they are not required at all.

Calculating determinant in NMatrix is tricky where a matrix is reduced either to a Lower or Upper matrix and the Diagonal elements of the matrix are multiplied to get the result. Also, the correct sign of the result (whether positive or negative) is taken into account while calculating the determinant. However, NMatrix-JRuby uses Commons Math API to calculate the determinant.

```ruby
def det_exact
  if (@dim != 2 || @shape[0] != @shape[1])
    raise(ShapeError, "matrices must be square to have a determinant defined")
    return nil
  end
  to_return = LUDecomposition.new(self.twoDMat).getDeterminant()
end
```

Given below is the code, that shows how Cholesky decomposition has been implemented by using Commons Math API. Similarly, LU Decomposition and QR factorization have been implemented.

**Cholesky Decomposition**

```ruby
  def factorize_cholesky
    cholesky = CholeskyDecomposition.new(self.twoDMat)
    l = create_dummy_nmatrix
    twoDMat = cholesky.getL
    l.s = ArrayRealVector.new(ArrayGenerator.getArrayDouble\
        (twoDMat.getData, @shape[0], @shape[1]))

    u = create_dummy_nmatrix
    twoDMat = cholesky.getLT
    u.s = ArrayRealVector.new(ArrayGenerator.getArrayDouble\
      (twoDMat.getData, @shape[0], @shape[1]))
    return [u,l]
  end
```

Cholesky Decomposition for an NMatrix-JRuby requires the matrix to be square matrix.

**LUDecomposition**

```ruby
  def factorize_lu with_permutation_matrix=nil
    raise(NotImplementedError, "only implemented for dense storage")\
       unless self.stype == :dense
    raise(NotImplementedError, "matrix is not 2-dimensional")\
       unless self.dimensions == 2
    t = self.clone
    pivot = create_dummy_nmatrix
    twoDMat = LUDecomposition.new(self.twoDMat).getP
    pivot.s = ArrayRealVector.new(ArrayGenerator.getArrayDouble\
    (twoDMat.getData, @shape[0], @shape[1]))
    return [t,pivot]
  end
```

**QRFactorization**

```ruby
  def factorize_qr
    raise(NotImplementedError, "only implemented for dense storage")\
       unless self.stype == :dense
    raise(ShapeError, "Input must be a 2-dimensional matrix to have\
       a QR decomposition") unless self.dim == 2
    qrdecomp = QRDecomposition.new(self.twoDMat)

    qmat = create_dummy_nmatrix
    qtwoDMat = qrdecomp.getQ
    qmat.s = ArrayRealVector.new(ArrayGenerator.\
      getArrayDouble(qtwoDMat.getData, @shape[0], @shape[1]))

    rmat = create_dummy_nmatrix
    rtwoDMat = qrdecomp.getR
    rmat.s = ArrayRealVector.new(ArrayGenerator.\
      getArrayDouble(rtwoDMat.getData, @shape[0], @shape[1]))
    return [qmat,rmat]

  end
```

**NMatrix#solve**

The solve method currently uses LUDecomposition and Cholesky Decomposition for solving the equations.

```ruby
  def solve(b, opts = {})
    raise(ShapeError, "Must be called on square matrix")\
       unless self.dim == 2 && self.shape[0] == self.shape[1]
    raise(ShapeError, "number of rows of b must equal number\
       of cols of self") if self.shape[1] != b.shape[0]
    raise(ArgumentError, "only works with dense matrices") if self.stype != :dense
    raise(ArgumentError, "only works for non-integer, non-object dtypes")\
       if integer_dtype? or object_dtype? or b.integer_dtype? or b.object_dtype?

    opts = { form: :general }.merge(opts)
    x    = b.clone
    n    = self.shape[0]
    nrhs = b.shape[1]

    nmatrix = create_dummy_nmatrix
    case opts[:form]
    when :general, :upper_tri, :upper_triangular, :lower_tri, :lower_triangular
      #LU solver
      solver = LUDecomposition.new(self.twoDMat).getSolver
      nmatrix.s = solver.solve(b.s)
      return nmatrix
    when :pos_def, :positive_definite
      solver = Choleskyecomposition.new(self.twoDMat).getSolver
      nmatrix.s = solver.solve(b.s)
      return nmatrix
    else
      raise(ArgumentError, "#{opts[:form]} is not a valid form option")
    end

  end
```

**NMatrix#matrix_solve**

Given, we need to solve a system of linear equations:

                        AX = B
where A is an m×n matrix, B and X are n×p matrices, we need to solve this equation by iterating through B.

NMatrix-MRI implements this functionality using `NMatrix::BLAS::cblas_trsm` method. However, for NMatrix-JRuby,  `NMatrix#matrix_solve` is the analogous method used.

```ruby
  def matrix_solve rhs
    if rhs.shape[1] > 1
      nmatrix = NMatrix.new :copy
      nmatrix.shape = rhs.shape
      res = []
      #Solve a matrix and store the vectors in a matrix
      (0...rhs.shape[1]).each do |i|
        res << self.solve(rhs.col(i)).s.toArray.to_a
      end
      #res is in col major format
      result = ArrayGenerator.getArrayColMajorDouble \
         res.to_java :double, rhs.shape[0], rhs.shape[1]
      nmatrix.s = ArrayRealVector.new result

      return nmatrix
    else
      return self.solve rhs
    end
  end
```

Currently, Hessenberg transformation for NMatrix-JRuby has not been implemented.

## **Other dtypes**

We have tried implementing float dtypes using jblas FloatMatrix.  jblas was used instead of Commons Math as the latter uses Field Elements for Floats and it had some issues with Reflection and TypeErasure. However, it resulted in errors due to precision.


## **Code Organisation and Deployment**

To minimise conflict with the MRI codebase all the jruby front end code has been placed in `/lib/nmatrix/jruby` directory. `lib/nmatrix/nmatrix.rb` decides whether to load `nmatrix.so` or `nmatrix_jruby.rb` after detecting the Ruby Platform.

The added advantage is that the ruby interpreter must not decide which function to call at run-time. The impact on performance can be seen when programs which intensively use NMatrix for linear algebraic computations(e.g. mixed-models) are run.

## **Test Report**

After the port; this is the final report that summarizes the number of tests that successfully pass:

**NMatrix**

|Spec file|Total Tests|Success|Failure|Pending|
|------------|:------------:|:-----------:|:-------------:|:-------------:|
|00_nmatrix_spec|188|139|43|6|
|01_enum_spec|17|8|09|0|
|02_slice_spec|144|116|28|0|
|03_nmatrix_monkeys_spec|12|11|01|0|
|elementwise_spec|38|21|17|0|
|homogeneous_spec.rb|07|06|01|0|
|math_spec|737|541|196|0|
|shortcuts_spec|81|57|24|0|
|stat_spec|72|40|32|0|
|slice_set_spec|6|2|04|0|

<br>
Why some tests fail?

1.  Complex dtype has not been implemented.
2.  Sparse matrices (list and yale) have not been implemented.
3.  Decomposition methods that are specific to LAPACK and ATLAS have not been implemented.
4.  Integer dtype is not properly assigned to Floor, Ceil and Round methods.

**Mixed-Models**

|Spec file|Total Test|Success|Failure|Pending|
|------------|:------------:|:-----------:|:-------------:|:-------------:|
|Deviance_spec|04|04|0|0|
|LMM_spec|195|195|0|0|
|LMM_categorical_data_spec.rb|48|45|3|0|
|LMMFormula_spec.rb|05|05|0|0|
|LMM_interaction_effects_spec.rb|82|82|0|0|
|LMM_nested_effects_spec.rb|40|40|0|0|
|matrix_methods_spec.rb|52|52|0|0|
|ModelSpecification_spec.rb|07|07|0|0|
|NelderMeadWithConstraints_spec.rb|08|08|0|0|

<br>


## **Future work**

NMatrix on JRuby offers comparable speeds to MRI. For specific computations it will be possible to leverage the threading support of JRuby and speed up things using multiple cores.

Adding new functionality to NMatrix-JRuby will be easy from here. Personally, I am interested to add OpenCL support to leverage the GPU computational capacity available on most machines today.

## **Conclusion**
The main goal of this project was to bring  **Scientific Computation to JRuby**, to gain from the performance JRuby offers.

By the end of GSoC, we have been able to successfully create a linear algebra library, NMatrix for JRuby users, which they can easily run on their machines unless they want to use Complex numbers.

We have mixed-models gem simultaneously ported to JRuby. Even here, we are very close to MRI, considering the performance .


## **Acknowledgements**

I would like to express my sincere gratitude to my mentor Pjotr Prins for the continuous support through the summers, and for his patience, motivation, enthusiasm, and immense knowledge. I could not have imagined having a better advisor and mentor, for this project.

I am very grateful to Google and the Ruby Science Foundation for this golden opportunity.

I am very thankful to Charles Nutter, John Woods, Sameer Deshmukh, Kenta Murata and Alexej Gossmann, who mentored me through the project. It has been a great learning experience.

I thank my fellow GSoC participants Rajith, Lokesh and Gaurav who helped me with certain aspects of my project.