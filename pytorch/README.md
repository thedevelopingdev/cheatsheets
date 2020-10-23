# PyTorch cheatsheet

## Torch data types[^datatypes]

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


[^datatypes]: PyTorch documentation, `torch.tensor`. ([original](https://pytorch.org/docs/stable/tensors.html))
