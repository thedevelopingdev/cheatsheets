# VSCode cheatsheet

## Configuration

- menubar
- titlebar
- activity bar


## Vim keybindings

#### Remap `j` to `gj` and `k` to `gk` (visual line movement)

```json
{
  "vim.normalModeKeyBindingsNonRecursive": [
      {
          "before": ["j"],
          "after": ["g", "j"]
      },
      {
          "before": ["k"],
          "after": ["g", "k"]
      }
  ]
}
```
