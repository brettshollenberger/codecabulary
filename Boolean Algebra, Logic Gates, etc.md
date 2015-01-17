# Boolean Algebra, Logic Gates, etc

Every computer, phone, and tablet is based on a set of chips that store and process information. These chips are made of the same building blocks: elementary logic gates.

Some very simple chips are called Boolean gates. Boolean gates implement Boolean functions--functions which take binary inputs (true/false, 1/0) , and return binary outputs. 

#### Specifications of Boolean Functions

Boolean functions can be specified with truth tables, which map all possible combinations of inputs to their respective outputs.

| x | y | z | f(x, y, z) |
|:--|:--|:--|:-----------:| 
| 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 0 |

Boolean functions can also be illustrated using Boolean expressions--algebraic expressions which resolve all cases to the outputs outlined in the truth table. For example, the truth table above can be described by the expression:

$$	f(x, y, z) = (x + y) \bullet \neg z $$

A particular expression is also said to be the "canonical expression." We can arrive at the canonical expression by consulting the truth table for all results that are true (1), and `or`ing the expressions together.

$$ f(x, y, z) = \neg x y \neg z + x \neg y \neg z + xy \neg z $$ 

#### Two-input Boolean Functions

For all boolean functions of `n` binary variables, `2^(2^n)` boolean functions can be defined. For two-input boolean functions, that implies 16 possible boolean functions. 

For `x = 0 0 1 1`, `y = 0 1 0 1`:

| Name        | Function               | Output    |
| :-----------| :--------------------- | :-------- |
| Constant 0  | $$ 0 $$                | `0 0 0 0` |
| And         | $$ x \bullet y $$      | `0 0 0 1` |
| x and not y | $$ x \bullet \neg y $$ | `0 0 1 0` |
| x           | $$ x $$                | `0 0 1 1` |
| not x and y | $$ \neg x \bullet y $$ | `0 1 0 0` |
| y           | $$ y $$                | `0 1 0 1` |
| Xor         | $$ x \bullet \neg y + \neg x \bullet y $$ | `0 1 1 0` |
| x or y      | $$ x + y $$            | `0 1 1 1` |
| Nor         | $$ \neg (x + y) $$     | `1 0 0 0` |
| Equivalence | $$ x \bullet y + \neg x \bullet \neg y $$     | `1 0 0 1` |
| Not y       | $$ \neg y $$           | `1 0 1 0` |
| If y then x | $$ x + \neg y $$       | `1 0 1 1` |
| Not x       | $$ \neg x $$           | `1 1 0 0` |
| If x then y | $$ \neg x + y $$       | `1 1 0 1` |
| Nand        | $$ \neg (x \bullet y) $$ | `1 1 1 0` |
| Constant 1 | $$ 1 $$                 | `1 1 1 1` |

#### Nand

`Nand` has an interesting property: `and`, `or`, and `not` can each be defined in terms of `nand` and `nand` alone. 

##### Nand And

$$ x \bullet y = Nand(\neg(x \bullet y), \neg(x \bullet y)) $$

$$ true \bullet true = Nand(\neg(true \bullet true), \neg(true \bullet true)) $$
$$ true \bullet true = Nand((false), (false)) $$
$$ true \bullet true = true $$

$$ true \bullet false = Nand(\neg(true \bullet false), \neg(true \bullet false)) $$
$$ true \bullet false = Nand((true), (true)) $$
$$ true \bullet false = false $$

$$ false \bullet false = Nand(\neg(false \bullet false), \neg(false \bullet false)) $$
$$ false \bullet false = Nand((true), (true)) $$
$$ false \bullet false = false $$

##### Nand Or

$$ x + y = Nand(\neg(x \bullet x), \neg(x \bullet x)) $$

$$ true + true = Nand(\neg(true \bullet true), \neg(true \bullet true)) $$
$$ true + true = Nand(false, false) $$
$$ true + true = true $$

$$ true + false = Nand(\neg(true \bullet true), \neg(false \bullet false)) $$
$$ true + false = Nand(false, true) $$
$$ true + false = true $$

$$ false + false = Nand(\neg(false \bullet false), \neg(false \bullet false)) $$
$$ true + false = Nand(true, true) $$
$$ true + false = false $$

##### Nand Not

$$ \neg x = \neg(x \bullet x) $$

$$ \neg true = \neg(true \bullet true) $$
$$ \neg true = \neg(true) $$

$$ \neg false = \neg(false \bullet false) $$
$$ \neg false = \neg(false) $$

#### Composite Gates

Composite gates are more complex gates defined in terms of simpler gates (`and`, `or`, and `not`). 
na
##### Xor

`Xor` can be defined, for example, using:

$$ Or(And(x, Not(y)), And(y, Not(x))) $$



