### Meow Commands
* `&` - And Read
* `>` - Write
* `|` - Then Read
* `@` - Event
* `~` - Watch Together (default)
* `#` - Hook
* `*` - Method
* `%` - Filter
* `{` - Watch Each

### Meow Word Modifiers
Word modifiers are appended to the end of a word in data flow sentence
* `:` - Alias
* `?` - Optional
* `!` - Required



The language parses statements down into a series commands, with a word/words that go with each command.

There is no complexity to the syntax. It is designed to be super basic.

"Phrases" a word or words in a command

Meow can be used in different places. Actions, if you are listening to an event, or you can use them in buses to transform data, calculated properties.

It's a way to set up data pipelines. Some do a lot, some do just a little bit.

*Phrase Commands

The default command if there is no character at the start it assumes you want `~` Watch Together.

Example:
I have 3 states in my application : bunny, dog and cat.
I want to watch all 3 of them. When all 3 have values that have been set in the framework, then I will continue on and I will bundle them all up at the next RequestAnimationFrame (which is the framework's default scheduler) and it will give the next piece of the pipe a hash that will look like

const