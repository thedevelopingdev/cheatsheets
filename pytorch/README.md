# PyTorch cheatsheet

## Torch data types

- Sources:
  - Torch documentation, `torch.tensor`. ([original](https://pytorch.org/docs/stable/tensors.html))

`dtype` is a tensor _attribute_ that corresponds to the type of data stored by
the Tensor object. It makes it easy to handle tensor types without concern of
the backend (i.e. CPU or GPU).

### Floating point numbers


### Complex numbers

### Integral numbers

### Booleans

|Data type|dtype|CPU tensor|GPU tensor|
|:--------|:---:|:--------:|:--------:|
|Boolean|`torch.bool`|`torch.BoolTensor`|`torch.cuda.BoolTensor`|


## Tensor creation

#### Create a tensor of all 1's (`torch.float32`)

```python
dim0, dim1, dim2 = 4, 6, 7
my_tensor = torch.ones(dim0, dim1, dim2, dtype=torch.float64)
# 
```

## Retrieving tensor metadata

#### Tensor type

```python
# get the tensor dtype
dtype = tensor.dtype

# get the CPU/GPU class type
cls_type = tensor.type()
```

