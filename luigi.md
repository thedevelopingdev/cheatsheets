# Luigi workflow scheduler cheatsheet

* Running a Luigi workflow
```
luigi --module <module name> <task name> --[param] [value] --local-scheduler
```

## Task types
### `ExternalTask`
### `WrapperTask`