## Local Test

```
bundle install
bundle exec jekyll serve
```

## Gotchas

1. Wrong Ruby version

Error message:
```
/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/rubygems.rb:283:in `find_spec_for_exe': Could not find 'bundler' (2.7.1) required by your /Users/.../Gemfile.lock. (Gem::GemNotFoundException)
```

Solution:

I've had ruby 3.3.0 installed in rbenv and need to activate it. Check if this command is in `~/.zshrc`

```
eval "$(rbenv init - zsh)"
```

Then

```
source ~/.zshrc
```
