# Test Babel CLI `--ignore` flag

With the update of Babel to version `7.0.0` it seems like the `--ignore` flag of `@babel/cli` gets ignored (hehe). This is a repository that can reliably reproduce that bug.

To try it out clone the repository.

```
git clone git@github.com:sto3psl/test-babel-ignore.git
```

Install the dependencies `@babel/cli` and `babel-cli`.

```
npm install
```

Now you are ready to go. There are 4 npm scripts defined in `package.json`. 2 using `@babel/cli` and 2 using `babel-cli`.

## Test 1 - `babel-cli@6.26.0`

```
❯ npm run start-old

> test-babel-ignore@1.0.0 start-old /Users/fabian/code/test-babel-ignore
> ./node_modules/babel-cli/bin/babel.js src --out-dir lib-old --ignore ignore.js

src/index.js -> lib-old/index.js
src/index.test.js -> lib-old/index.test.js
```

Expected result: 

```
❯ ls lib-old
index.js      index.test.js
```

Actual result:

```
❯ ls lib-old
index.js      index.test.js
```

Everything works fine and `index.ignore.js` got ignored.

This also works with ignoring a list of patterns:

```
❯ npm run ignore-old

> test-babel-ignore@1.0.0 ignore-old /Users/fabian/code/test-babel-ignore
> ./node_modules/babel-cli/bin/babel.js src --out-dir lib-old --ignore ignore.js,test.js

src/index.js -> lib-old/index.js
```

Expected result: 

```
❯ ls lib-old
index.js
```

Actual result:

```
❯ ls lib-old
index.js
```

## Test 2 - `@babel/cli@7.0.0`

```
❯ npm run start

> test-babel-ignore@1.0.0 start /Users/fabian/code/test-babel-ignore
> ./node_modules/@babel/cli/bin/babel.js src --out-dir lib --ignore ignore.js

Successfully compiled 3 files with Babel.
```

Expected result: 

```
❯ ls lib
index.js      index.test.js
```

Actual result:

```
❯ ls lib
index.ignore.js index.js        index.test.js
```

All 3 files get compile even tough `@babel/cli` should ignore `index.ignore.js`. And lists also don't work:

```
❯ npm run ignore

> test-babel-ignore@1.0.0 ignore /Users/fabian/code/test-babel-ignore
> ./node_modules/@babel/cli/bin/babel.js src --out-dir lib --ignore ignore.js,test.js

Successfully compiled 3 files with Babel.
```

Expected result: 

```
❯ ls lib
index.js
```

Actual result:

```
❯ ls lib
index.ignore.js index.js        index.test.js
```
