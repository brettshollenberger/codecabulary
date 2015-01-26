# Proofs of ALU Operations

The Arithmetic Logic Unit presented in The Elements of Computing Systems is simple and foundational--it aims to allow more complex operations to be built from it. 

For this reason, it focuses on the most basic of mathematical operations: constants (1, 0, -1), identity operations (x, y), boolean algebra (x and y, x or y, not x, not y), addition (x + y), subtraction (x-y, y-x), incrementation and decrementation (x+1, y+1, x-1, y-1), and arithmetic negation (-x, -y). 

The ALU uses six selector bits to build up to these operations. This document proves that the selector bits enable these functions. 

##### The Constant Zero
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 0 | 1 | 0 | 1 | 0 | 0 |

$$ x = 0, y = 0, f=+$$
$$ f = (0 + 0) = 0 \blacksquare$$

This solution is trivial. If `zx` and `zy` and `f` is addition, `0 + 0` leaves us with the result `0`.

##### The Constant One
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 |

$$ x = -1, y = -1, f=+, no=true$$
$$ f = !(-1 + -1) = !(-2)_{10} $$
$$ !(11..10)_2= (00..01)_2 = 1 \blacksquare $$

* If `zx` and `zy` then `x` and `y` = `0`.
* If `nx` and `ny` then `x` and `y` are _bitwise negated_ (not arithmetically negated). In a bitwise negation, all the bits are flipped, and with every bit flipped in `00..00`, we end up with `11..11`, which in base 10, is -1.
* `-1+(-1) = -2` in base 10.
* Then `no=1`, so the result is also bitwise negated (`11..10` = `00..01` = `1` in base 10). 

##### The Constant Negative One
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 1 | 1 | 0 | 1 | 0 | -1 |

$$ x = -1, y = 0, f=+$$
$$ f = (-1 + 0) = -1 \blacksquare $$

Using the previous two proofs, this proof be trivial.

##### Identity Function X
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 0 | 1 | 1 | 0 | 0 | x |

$$ x = x, y = -1, f=\bullet $$
$$ f =  x \bullet (11..11)_{2} $$
$$ f = x \blacksquare $$

This is the first function to make use of the boolean `and` function, and it might be counter-intuitive if you think of the functions in base 10. Of course, there are multiple ways to solve this problem (the most obvious being x + 0), but we can show off the boolean and logic straightforwardly in this function.

* `x` is neither negated nor zeroed--it is `x`.
* Since `y = (11..11)`, every bitwise `and` operation will pass through the value of that bit in `x`

This solution is quite a bit nicer than the `x + 0` solution, since it is actually an identity function and more closely states its intent.

##### Identity Function Y
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 1 | 0 | 0 | 0 | 0 | y |

$$ x = -1, y = y, f=\bullet $$
$$ f =  y \bullet (11..11)_{2} $$
$$ f = y \blacksquare $$

This proof is the same as the identity proof for `x`.

##### Bitwise Not X
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 1 | 1 | 1 | 0 | 0 | !x |

$$ x = \neg x, y = -1, f=\bullet $$
$$ f =  \neg x \bullet (11..11) $$
$$ f = \neg x \blacksquare $$

This proof is the same as the previous two identity proofs, but allowing `x` to be negated first.

##### Bitwise Not Y
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 1 | 0 | 1 | 0 | 0 | !y |

$$ x = -1, y = \neg y, f=\bullet $$
$$ f =  \neg y \bullet (11..11) $$
$$ f = \neg y \blacksquare $$

##### Arithmetic Negation of X (-x)
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 0 | 1 | 1 | 1 | 1 | -x |

$$ x = x, y = -1, f=+, no=true $$
$$ f = \neg (x-1) $$
$$ f = \neg x + 1 \blacksquare $$

A particularly attractive feature of representing signed boolean numbers using 2's complement is that numbers can easily be arithmetically negated with the function `(!x + 1)`. This function is a simple rearrangement of that function.

##### Arithmetic Negation of Y (-y)
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 1 | 0 | 0 | 1 | 1 | -x |

$$ x = -1, y = y, f=+, no=true $$
$$ f = \neg (y-1) $$
$$ f = \neg y + 1 \blacksquare $$

##### Incrementing X (x+1)
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 1 | 0 | 1 | 1 | 1 | y+1 |

$$ x = \neg x, y = -1, f=+, no=true $$
$$ f = \neg(\neg x - 1)$$
$$ f = x + 1 \blacksquare $$

##### Incrementing Y (y+1)
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 1 | 1 | 1 | 1 | 1 | x+1 |

$$ x = -1, y = \neg y, f=+, no=true $$
$$ f = \neg(\neg y - 1)$$
$$ f = y + 1 \blacksquare $$

##### Decrementing X (x-1)
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 0 | 1 | 1 | 1 | 0 | x-1 |

$$ x = x, y = -1, f=+ $$
$$ f = x - 1 \blacksquare $$

##### Decrementing Y (y-1)
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 1 | 1 | 0 | 0 | 1 | 0 | x-1 |

$$ x = -1, y = y, f=+ $$
$$ f = -1 + y $$
$$ f = y - 1 \blacksquare $$

##### Addition (x + y)
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 0 | 0 | 0 | 1 | 0 | x+y |

$$ x = x, y = y, f=+ $$
$$ f = x + y $$

##### X - Y
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 1 | 0 | 0 | 1 | 1 | x-y |

$$ x = \neg x, y = y, f = +, no = true $$
$$ f = \neg(\neg x + y) $$
$$ f = x - y $$

##### Y - X
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 0 | 0 | 1 | 1 | 1 | x-y |

$$ x = x, y = \neg y, f = +, no = true $$
$$ f = \neg(\neg y + x) $$
$$ f = y - x $$

##### X & Y
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 0 | 0 | 0 | 0 | 0 | x&y |

$$ x = x, y = y, f = \bullet $$
$$ f = x \bullet y $$

##### X | Y
| zx | nx | zy | ny | f | no | out |
| :--| :--| :--| :--| :--| :--| :--|
| if zx then x=0 | if nx then x=!x | if zy then y=0 | if ny then y=!y | if f then out=x+y else out=x&y | if no then out=!out | f(x,y) |
| 0 | 1 | 0 | 1 | 0 | 1 | x|y |

$$ x = \neg x, y = \neg y, f = \bullet, no=true $$
$$ f = \neg(\neg x \bullet \neg y) $$
$$ f = x + y $$

The proof of this function is probably the least obvious. It relies on one of the major postulates of boolean algebra: Demorgan's Laws. Demorgan's Laws describe how `not` distributes over boolean functions:

* Flip all values to their inverses
* Flip all ANDs to ORs
* Flip all ORs to ANDs

$$ \neg (x + y) = \neg x \bullet \neg y $$

This example should make sense in plain English. "Neither x or y" is the same as "not x and not y." 

"We did not eat bacon or eggs" is the same as "we did not eat bacon and we did not eat eggs."

$$ \neg(x \bullet y) = \neg x + \neg y $$

This example is less common to hear in English, although if we use a real example, it makes sense. If you said grandma was "alive and well," and she happened to be alive, but sick--technically, you would be a liar. She is not alive and well. And the statement "not alive and well" is true, because she is either not alive or not well.

$$ \neg (\neg x \bullet \neg y) = x + y $$

All the double-negatives in this example are probably driving you crazy, but fortunately, we have some words in English that lend themselves to making this type of statement without it becoming too much of a mouthful.

"I am not unhappy and unwell," means, that I am either happy or well. I could be either unhappy _or_ unwell, which would make this statement rather devious and annoying--the type of thing I hope you never hear from your boss, particularly while she is sick.

This is the example from our X + Y question, and we need to write it this way as a little trick to allow ORs in our ALU.