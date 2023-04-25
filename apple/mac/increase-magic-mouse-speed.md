# How to increase the magic mouse speed

```bash
// Check the current scaling of your mouse
defaults read -g com.apple.mouse.scaling
// For example, the output could be "3"

// To increase the mouse speed, set a higher value than the current scaling value, for example:
defaults write -g com.apple.mouse.scaling 5

// Test if the mouse speed has indeed increased.
```

## References
- [Magic mouse 2 moves very slow](https://discussions.apple.com/thread/25315959)


