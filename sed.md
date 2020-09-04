# `sed` (stream editor) cheatsheet

* Delete lines based on pattern
```
sed '/<regex pattern>/d'
```

* In-place edit
```
sed -i <sed pattern> <file name>
```