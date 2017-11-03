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

```
const meow = '~ bunny, dog, cat';
const hash = {bunny: 5, dog: 'moo', cat: []};
```

Any of these words can have modifiers attached to them:

MODIFIERS:

```
':': 'ALIAS',
'?': 'MAYBE',
'.': 'PROP'
```

example:

`alias`
I'm getting 'bunny' as my data source but the way I'm using it is totally different so I'm going to rename it as 'fur':

```
const meow = '~ bunny:fur, dog, cat';
const hash = {fur: 5, dog: 'moo', cat: []};
```

What comes out is going to be named 'fur'.

`prop`
If I've got a property on 'bunny' that I need, I could do:

```
const meow = '~ bunny.fur.color:furColor, dog, cat';
const hash = {furColor: 'red', dog: 'moo', cat: []};
```


`maybe`

`prop`
If I want to check that bunny exists first:

```
const meow = '~ bunny?.fur.color:furColor, dog, cat';
const hash = {furColor: 'red', dog: 'moo', cat: []};
```

MORE COMMANDS
`{`

```
const meow = '~ bunny & dog }';
```


```
const meow = '~ bunny & dog, cat }';
```


`>`

```
const meow = '~ bunny & dog, cat > cow';
```


`>`

```
const meow = '{ bunny & dog, cat > cow';
```

```
const meow = '{ bunny, dog, cat > cow';
```


`%`

   ```
   const meow = '{ bunny & dog, cat % isHappy > cow';
   ```

   ```
   const meow = '~ bunny & dog, cat % isHappy > cow';
   ```


`*`

   ```
   const meow = '~ bunny & dog, cat * toHappy > cow';
   ```

`|`

   ```
   const meow = '~ bunny | dog, cat % toHappy > cow';
   ```



