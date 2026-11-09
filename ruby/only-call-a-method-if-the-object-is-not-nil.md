# Only Call a Method if the Object is not `nil`

I was learning about pattern matching in Ruby today (via The Odin Project) and in an [example](https://www.theodinproject.com/lessons/ruby-pattern-matching#find-pattern), I saw this weird thing, `&.`, used like this:

```ruby
match&.fetch(:first_language)
```

The lesson went on to explain that it's called the "**safe search navigator**" and was used to make sure that the `fetch` method could be safely called on `match`. I was intrigued by it and so I did a bit more Googling and I found that it's also called the "**safe navigation operator**" and, sometimes, the "**lonely operator**" (the reason for that name is beyond me, though).

It's used when you're unsure whether or not an object is `nil` but you want to call a method on it if it isn't.

For example, let's say I've got this:

```ruby
def get_name(stranger: false)
  if stranger
    puts 'What\'s your name?'
    name = gets.chomp
  end
end

name = get_name #=> nil

name_as_sym = name.to_sym #=> undefined method 'to_sym' for nil (NoMethodError)
```

The code — yes, I know it's contrived, just stick with me here — fails with a `NoMethodError`. Nasty stuff.

Buuuuut if I'd used the safe search navigator like so:

```ruby
name_as_sym = name&.to_sym #=> nil
```

I'd have gotten `nil` back as the return value. No nasty error. And that's what it's for. *It only calls a method on an object when it's safe to do so, i.e, when it's **not** `nil`*.

For completion's sake, here's what the code would have to look like for `name` not to be `nil`:

```ruby
def get_name(stranger: false)
  if stranger
    puts 'What\'s your name?'
    name = gets.chomp
  end
end

name = get_name(stranger: true) 
# What's your name?
# Fortunatus
#=> "Fortunatus"

name_as_sym = name&.to_sym 
#=> :Fortunatus
```

### Is there more to it?

Why, yes, there is. See, as with methods, you can chain `&.`s. 

Let's say I want to check for the presence of a letter 'x' in the returned name — for *whatever* reason. Following the previous example, where `name` is `"Fortunatus"`, I could do something like this: 

```ruby
name.index('x')
```

That'd return `nil` — which is expected, right? 

But what if we did something like this:

```ruby
name&.index('x').to_i #=> 0
```

That'd return `0`. Depending on what I wanted to achieve with such silly code, that could be a problem. The returned `0` could be misconstrued as *"The letter 'x' is at index 0"*. 

Using the safe search navigator, though, I could work around this to ensure clarity, like so:

```ruby
name&.index('x')&.to_i #=> nil
```

Now, it'd be clear that the letter 'x' doesn't appear in `name`.

Again, this is a silly example cos the whole thing just mimics the behaviour of calling the `String#index` method by itself, I know, but it's the quickest example I could think of to demonstrate using `&.` in a long chain, *okay?!*

Anyhoo, for better examples and more insight into long chains of `&.`, check out [this page from thoughtbot](https://thoughtbot.com/blog/ruby-safe-navigation).

---

### Sources

- https://stackoverflow.com/questions/36087221/ruby-null-conditional-operator

- https://medium.com/jungletronics/safe-navigation-operator-97cd9350ccea
