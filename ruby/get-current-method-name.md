# Get Current Method Name

In creating my own custom enumerables (a project in The Odin Project) I saw this fun little thing: `__method__` in the [docs for `Object#to_enum`](https://docs.ruby-lang.org/en/3.4/Object.html#method-i-to_enum) and, apparently, all it does is return the name of the current method as a symbol.

This could be very useful for me in terms of...well, uniformity, I guess, cos now, in my custom `Array#each`, aptly named `Array#my_each`, instead of this:

```ruby
class Array
  def my_each
    return to_enum(:my_each) unless block_given?

    for i in 0...self.size
      yield self[i]
    end

    self
  end
end
```

I could just do this:

```ruby
class Array
  def my_each
    return to_enum(__method__) unless block_given?

    for i in 0...self.size
      yield self[i]
    end

    self
  end
end
```

Neat, no? 

And now, in my other custom enumerables, rather than re-typing *their* names whenever I wanna pass them as the `method` argument to `Object#to_enum`, I could just use `__method__` and it all stays uniform.

---

### Sources

- https://docs.ruby-lang.org/en/3.4/Kernel.html#method-i-__method__
