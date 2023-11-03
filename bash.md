# Bash scripting

## Arrays

```bash
myArray=("element1" "element2" 3 4 5)

# the @ symbol gets all elements
# for item in $myArray only gets the first element
for item in ${myArray[@]}; do
  echo $item
done
```

