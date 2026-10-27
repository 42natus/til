# Quickly Check the Creation Date of a Repo

In creating this repo, I was curious about which repo came up with this idea first (between [thoughtbot/til](https://github.com/thoughtbot/til) and [jbranchaud/til](https://github.com/jbranchaud/til)). I searched both of the repos' main page on GitHub for anywhere that showed the creation date of the repo but found nothing like that. 

Eventually, I found that you can simply change the `github.com` part of the repo's address in the search bar to `api.github.com/repos` and hit `Enter`. This retrieves a JSON payload that contains the key `"created_at"` with the repo's date and time of creation as its value.

For example, to check this repo's ([42natus/til](https://github.com/42natus/til)) date of creation on GitHub, you'd edit the url in the search bar from this

```
https://github.com/42natus/til
```

to

```
https://api.github.com/repos/42natus/til
```

and then you can just hit `Ctrl+F` and search for `created_at`. That's it!

---

[source](https://stackoverflow.com/questions/23611669/how-to-find-the-created-date-of-a-repository-project-on-github)
