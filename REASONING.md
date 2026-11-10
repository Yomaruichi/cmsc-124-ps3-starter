# Reasoning for the Predictions Table

Write one entry under every heading below, then commit this file together with
`predictions.tsv`, before you run a case or open `tests/expected.tsv`.

Each entry names the rule that produces the four predicted fields for that
case. One to three sentences is enough. The grader checks structure only. It
does not judge quality, and it sets no word minimum.

After you run the cases, add a short correction under each case you predicted
wrongly. Say what you predicted, what CLISP printed, and which rule you
misread. Those entries belong in the correction commits.

## P01
`'(+ 1 (* 2 3))` heads with the function `+`, the inner multiplication returns `6`,
then `+` adds `1` into it which returns a `7`. That is an `integer`, so the length
is `none` and the head is a `function`.

## P02
`'(+ 1 2)` heads with the function `, the quote makes the list unevaluated so it
returns as `(+,1,2)`, that is a `cons` with length `3` and the head is a `function`.

## P03
`nil` is an atom which returns a `null`. It is an empty sequence so its
length is `0` and since it doesn't have a head the operator is `none`.

## P04
`t` is an atom which returns the symbol `t`. Since it isn't a list it will have
length of `none` and since it doesn't have a head the operator is `none`.

## P05
Replace this line with the rule behind this case's four fields.

## P06
Replace this line with the rule behind this case's four fields.

## P07
Replace this line with the rule behind this case's four fields.

## P08
Replace this line with the rule behind this case's four fields.

## P09
Replace this line with the rule behind this case's four fields.

## P10
Replace this line with the rule behind this case's four fields.

## P11
Replace this line with the rule behind this case's four fields.

## P12
Replace this line with the rule behind this case's four fields.

## P13
`(mapcar #'length '((1 2) () (3)))` heads with the function `mapcar`, it applies the
function length to each item in the unevaluated list which returns a list of the
length of each inner list `(2,0,1)`. It is a list so its a `cons` with length `3`
and the head is a `function`.

## P14
`(mapcar (lambda (n) (* n n)) '(1 2 3))` heads with the function `mapcar`, the lambda
squares each element in (1 2 3) which is list because of the quote. This returns as
`(1,4,9)` which is a list so its a `cons` with length `3` and the head is a `function`.

## P15
`(lambda (n) (* n 2))` heads with the macro `lambda`. Evaluating it creates a function
object, written as `#<function>`. Since lambda creates a function, its kind is a
`function` with length `none` and it is a type of `macro`. The body  (* n 2) runs when
the function is called.

## P16
`(let ((factor 2))`
    `(let ((f (lambda (n) (* n factor))))`
        `(let ((factor 10))`
            `(funcall f 3))))))`
heads with the special operator `let`, the lambda captures the outer binding where factor
is 2, the binding factor 10 does not affect it since its initialized later on after the
lambda. Calling the function f with 3 returns `6` which is an `integer` with `none` length
and `let` is a `special-operator`.