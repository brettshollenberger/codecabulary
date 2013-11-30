# Vim Commands

Vim is built on two ideas:

	1) Modal editing - Vim is entirely keyboard-driven, which allows its users to become blazing fast with it, but also means that keys will have to perform multiple duties. Vim accomplishes this by allowing the editor to shift between a number of modes; command mode, insert mode, and visual mode are the most basic. 
	2) Operators - Operators operate over portions of text
	
#### Command Mode:

Command mode allows us to issue different types of commands in the document:

#### Motions
	
The most basic Vim motions are left, down, up, and right, represented by the keys on the home row `H`, `J`, `K`, and `L` respectively.

	Goto Line - `12G` (Goto line 12)
	Goto first line - `gg`
	Goto last line - `G`
	
#### Search and Replace

Search and replace uses regular expressions in the following format:

	:%s/search/replace/options
	
E.g.

	:%s/def/function/g
	
The `c` option is unique to Vim, and stands for confirm. It will ask you before performing each substitution.

1) /search over text
2) Make a change
3) Press `n` to get the next instance
4) Press `.` to repeat the same change