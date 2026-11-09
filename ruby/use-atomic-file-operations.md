# Use Atomic File Operations

I just rounded up work on my [hangman](https://github.com/42natus/hangman) project and in it, I implemented a save feature that allows you to save your progress in the game and reload it later. This involved me serializing the game object — its state at the point where the save was initiated — and storing the serialized object in a file that lived in a directory dedicated to storing the saved game(s). 

To create this directory the first time, I did this:

```ruby
Dir.mkdir('.saves') unless Dir.exist?('.saves')
```

to check if the directory existed already. 

After running RuboCop though, I found out that this was unnecessary and potentially unsafe. 

### How it's unnecessary

The `Dir#mkdir` method is an idempotent method, meaning no matter how many times it's called (with the same directory path), it always returns the same result as if it'd only been called once. Like how multiplying a number by 1 gives you the original number no matter how many times you multiply it by 1.

This means I could've used just

```ruby
Dir.mkdir('.saves')
```

and if the directory already existed, no harm no foul.

### How it's unsafe

It turns out file systems are volatile. Especially cos other running processes could make it so that the result of your existence check could be invalid by the time the control flow reaches the next line. Check out [Sources](#sources) for more on this.

### What to do instead?

To this effect, it's recommended, by the [Ruby Style Guide](https://rubystyle.guide/#atomic-file-operations), to use something like this:

```ruby
FileUtils.mkdir_p('.saves')
```

---

Honestly, I don't think this file existence volatility presents any real problem in my case but since common knowledge says to either *not do the existence check* or to *do it but still implement exception handling in case things go wrong anyway*, I'll heed convention.

---

### Sources

- https://stackoverflow.com/questions/673654/when-is-it-okay-to-check-if-a-file-exists

- https://stackoverflow.com/questions/265953/how-can-you-easily-check-if-access-is-denied-for-a-file-in-net

- https://rubystyle.guide/#atomic-file-operations
