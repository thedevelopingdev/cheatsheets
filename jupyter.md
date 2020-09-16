# Jupyter notebook cheatsheets

## Installing Jupyter notebook extensions and Vim bindings

- install and enable jupyter extensions

```sh
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install
```

- install the vim binding extension

```sh
# install the extension
mkdir -p $(jupyter --data-dir)/nbextensions
cd $(jupyter --data-dir)/nbextensions
git clone https://github.com/lambdalisue/jupyter-vim-binding vim_binding

# activate the extension
jupyter nbextension enable vim_binding/vim_binding
```

- enable the code freezing extension

```sh
jupyter nbextension enable freeze/main
```
