`cog`: A single component. Every component has its own scope which is the script you are working in.

`chains`: array of components

`gears`: placeholders that swap out components dynamically

`aliases`: represent paths and API's. You can concatenate them together the way 'defines' work in the "C" language. You can precede any of your aliases with any alias that is the base directory. In your deployment process or in your code you can refactor and just change a single spot and since all the file paths are built by concatenating aliases together, you can change a base directory or move something around and you don't have to change where those files are located in any of your code.


```
Muta.cog({

       display: '<slot name="page"></slot>',

       aliases: {

           TRAIT: 'traits',
           FETCH: 'TRAIT fetch.js',
           APP_ROOT: './',
           BUBBLES_VIS: 'bubbles.js',
           API: 'http://xrdcwpwebtlm03.hca.corpad.net:81/api/',
           AUTH_API: 'API auth',
           COMMENTS_API: 'API comments',
           WORDS_API: 'API words',
           AUTH_PAGE: 'auth.js',
           MAIN_PAGE: 'main.js'
       },

       wires: {
           pageView: 'AUTH_PAGE'
       },

       gears: {
           page: 'pageView'
       }

   });
   ```

- State / Actions
Any method is automatically scoped to its component instance. Every component also has its own concept of state and action variables.
The difference between referencing an action and a state: You can give them the same name, but to write to an action you have to put a '$' in front of of it.

`states`:

```
states: {pageView: 'AUTH_PAGE'}
```

We have declared a state which is the current page view, and we set it to AUTH_PAGE which is an alias.

`actions` / `wires`:

```
wires: {
           pageView: 'AUTH_PAGE'
       },
```


is equivalent to:


```
states: {pageView: 'AUTH_PAGE'}
```

```
actions: {pageView: '> pageView'}
```

'> pageView': This is using a pseudo language called Meow for data flow that says when someone invokes the action pageView, it pushes the value to a state pageView. Wires do this for you.

`gears`:
```
gears: {page: 'pageView'}
```

'page' is the name of the slot, so it defines a 'page' component and its URL is going to be derived from the state of the page view.

What happens in summary is the auth page loads into the display.

If we go to AUTH_PAGE (auth.js):

```
Muta.cog({

       display: '<div class="page-auth"> Authorizing... </div> ',

       traits: [
           {url: 'FETCH', api: 'AUTH_API', response: 'authResponse', auto: true}
       ],


       buses: [
           'authResponse | *toNextPage'
       ],

       states: {

           authResponse: {}

       },

       toNextPage: function(x){
           console.log('next:', x);
           if(x)
               this.cog.scope.find('$pageView').write('MAIN_PAGE');
       }

   });
```

 It has a display for authorizing.
 All of your interesting code is probably either going to be put in components or traits.

`traits`
 (Entity Component System design pattern)
 A behavior that let you dynamically compose the behaviors you want on something.
 Examples: Validation traits, Layout Traits, Data flow traits

 In this instance, the framework itself has standard library that dynamically loads everything it needs when it needs it. To do network calls it calls a Fetch trait which does Window.Fetch API and will call APIS, and pump responses into the data points that it can reactively watch.


```
 traits: [
               {url: 'FETCH', api: 'AUTH_API', response: 'authResponse', auto: true}
           ]
```


All traits have a URL that tell you the file its based on. Everything else is options you want to configure.

In this example it calls the Auth API, when it returns with a response, it sends it to the $authResponse action.

Read the next page state, and write it into my pageView action, which changes pages. (Advance to the next page once we are authorized.) Auto means that it automatically fires.

If you put parameters in one of those Fetch traits you can say if the parameters ever change, automatically call something. If you have errors or status you can change those data points.
Watch and set things up so they run off of it.

`relays`

Let's look at the beginning of the FETCH alias pointing to fetch.js:


```
Muta.trait({

       relays: [
           {state: 'params'},
           {action: 'response'}
       ],

       buses: [
           'params | *makeRequest'
       ],

       prep: function(){

           this.api = this.cog.aliasContext.resolveUrl(this.config.api);

       },
       ...
```



Relays take the configuration properties you have passed in, and they wire things up to create a local binding that you can work with. If it's a state it watches it and pipes the values down. If it's an action, it will push values back up.

You can define a params property and if you give me that I will bind to it as a state. If you give me a response I will consider that as an action I can write back out to.

`buses`


```
buses: [
              'params | *makeRequest'
          ],
```


A bus is a way to set up data flow that you are explicitly wiring. This one says watch params and when it changes call a function makeRequest.

