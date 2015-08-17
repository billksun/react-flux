A GHCJS binding to [React](https://facebook.github.io/react/) based on the
[Flux](https://facebook.github.io/flux/) design.  The flux design pushes state and complicated logic
out of the view, allowing the rendering functions and event handlers to be pure Haskell functions.
When combined with React's composable components and the one-way flow of data, React, Flux, and
GHCJS work very well together.

# Docs

The [haddocks](https://hackage.haskell.org/package/react-flux) contain the documentation.

# Build

I am currently using the latest git version of GHCJS with GHC 7.10.2.  To compile and build, I use:

~~~
echo "compiler: ghcjs" > cabal.config
cabal configure
cabal build
~~~

# TODO Example Application

The source contains an [example TODO
application](https://bitbucket.org/wuzzeb/react-flux/src/tip/example/).

~~~
cabal configure -fexample
cabal build
cd example
make
firefox todo.html
~~~

If you don't have closure installed, you can open `example/todo-dev.html`.

# Test Suite

To run the test suite, first you must build both the example application and the test-client.  (The
test-client is a react-flux application which contains everything not contained in the todo
example.)

~~~
echo "compiler: ghcjs" > cabal.config
cabal configure -fexample -ftest-client
cabal build
cd example
make
~~~

The above builds the TODO application, compresses it with closure, and builds the test client.
Next, install [selenium-server-standalone](http://www.seleniumhq.org/download/) (also from
[npm](https://www.npmjs.com/package/selenium-server-standalone-jar)).  Then, build the
[hspec-webdriver](https://hackage.haskell.org/package/hspec-webdriver) test suite using GHC (not
GHCJS).  I use stack for this, although you can use cabal too if you like.

~~~
cd test/spec
stack build
~~~

Finally, start selenium-server-standalone and execute the test suite.  It must be started from the
`test/spec` directory, otherwise it does not find the correct javascript files.

~~~
.stack-work/dist/x86_64-linux/Cabal-1.22.4.0/build/react-flux-spec/react-flux-spec
~~~
