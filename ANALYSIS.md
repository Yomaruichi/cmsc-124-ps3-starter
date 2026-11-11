# Environments and the Toy Evaluator

An example form is `(let ((x 2)) (let ((f (lambda (y) (+ x y)))) (let ((x 100)) (f 1))))`.
Our evaluator returns `3` because the closure uses the binding of `x` that existed when
the lambda was evaluated rather than when `f` was called.

First, `eval-expr` recognizes the outer `let` form and calls `eval-let`. The `2` evaluates
to itself in the numberp branch. Next, extend-env places `(x . 2)` in front of the  empty
environment nil. The evaluator then evaluates the let body using this new environment,
where `x` is bound to `2`.

When `eval-expr` encounters `(lambda (y) (+ x y))`, the lambda branch passes the current
defining environment to `make-closure`. Inside make-closure, the line
`(list :closure params body env)` captures and stores that environment inside the parameters
and body. At this exact moment, the environment contains `((x . 2))`. It does not yet
contain `f`, because `eval-let` evaluates its initializer form before adding the new symbol
binding to the environment.

The next `let` introduces `(x . 100)` into the environment used at the call site. When
evaluating `(f 1)`, the application branch evaluates `f` to retrieve its closure and evaluates
`1` as the argument. Inside `apply-closure`, `(cadddr closure)` retrieves the saved environment
`((x . 2))`. The call `(extend-env params args captured-env)` prepends `(y . 1)`. Finally,
evaluating `(+ x y)`in this extended environment looks up these bindings and returns `3`.

If `apply-closure` instead extended the caller’s environment, lookup would find the active binding
`(x . 100)`. The scope-discrimination case would return `101`. This would give the toy language
dynamic scope, because a function’s free variables would resolve through its calling context
instead of its lexical defining environment.

Common Lisp has separate namespaces for variables and functions. A variable containing a function
therefore requires `funcall` to get that stored value. Our toy language uses one namespace. Its
application branch evaluates the head with `(eval-expr head env)`, so a symbol such as `f` is
looked up through the same environment as any other variable. The resulting closure is passed
directly to `apply-closure`, allowing `(f 1)` without a `funcall` form.
