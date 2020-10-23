# PyTorch cheatsheet

## Torch data types

- Sources:
  - Torch documentation, `torch.tensor`. ([original](https://pytorch.org/docs/stable/tensors.html))

`dtype` is a tensor _attribute_ that corresponds to the type of data stored by
the Tensor object. It makes it easy to handle tensor types without concern of
the backend (i.e. CPU or GPU).

### Floating point numbers

|Data type|dtype|CPU tensor|GPU tensor|Notes|
|:--------|:---:|:--------:|:--------:|:----|
|16-bit ("binary16")|`torch.float16`, `torch.half`|`torch.HalfTensor`|`torch.cuda.HalfTensor`|1 sign, 5 exponent, 10 significand bits; lower range but higher precision|
|16-bit ("brain floating point")|`torch.bfloat16`|`torch.BFloat16Tensor`|`torch.cuda.BFloat16Tensor`|1 sign, 8 exponent, 7 significand bits; higher range but lower precision|
|32-bit|`torch.float32`, `torch.float`|`torch.FloatTensor`|`torch.cuda.FloatTensor`||
|64-bit|`torch.float64`, `torch.double`|`torch.DoubleTensor`|`torch.cuda.DoubleTensor`||

### Complex numbers

|Data type|dtype|CPU tensor|GPU tensor|Notes|
|:--------|:---:|:--------:|:--------:|:----|
|32-bit|`torch.complex32`||||
|64-bit|`torch.complex64`||||
|128-bit|`torch.complex128`, `torch.cdouble`||||

### Integral numbers

|Data type|dtype|CPU tensor|GPU tensor|Notes|
|:--------|:---:|:--------:|:--------:|:----|
|Unsigned 8-bit|`torch.uint8`|`torch.ByteTensor`|`torch.cuda.ByteTensor`||
|Signed 8-bit|`torch.int8`|`torch.CharTensor`|`torch.cuda.CharTensor`||
|Signed 16-bit|`torch.int16`, `torch.short`|`torch.ShortTensor`|`torch.cuda.ShortTensor`||
|Signed 32-bit|`torch.int32`, `torch.int`|`torch.IntTensor`|`torch.cuda.IntTensor`||
|Signed 64-bit|`torch.int64`, `torch.long`|`torch.LongTensor`|`torch.cuda.LongTensor`||

### Booleans

|Data type|dtype|CPU tensor|GPU tensor|Notes|
|:--------|:---:|:--------:|:--------:|:----|
|Boolean|`torch.bool`|`torch.BoolTensor`|`torch.cuda.BoolTensor`||


## Tensor creation

#### Create a tensor of all 1's (`torch.float32`)

```python
dim0, dim1, dim2 = 4, 6, 7
my_tensor = torch.ones(dim0, dim1, dim2, dtype=torch.float64)
```

## Retrieving tensor metadata

#### Tensor type

```python
# get the tensor dtype
dtype = tensor.dtype

# get the CPU/GPU class type
cls_type = tensor.type()
```

#### Tensor shape/size

```python
shape = tensor.shape
dim0, dim1 = shape[0], shape[1]
```

