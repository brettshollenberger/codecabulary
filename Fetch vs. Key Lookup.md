# Fetch vs. Key Lookup

If you're building data off of data provided by another company or software, you might not know how rigorous they've been with their collection process, so you should make sure you _are_ rigorous in ensuring you don't store garbage data.

In this case, looking up data on an imported will return `nil` values for keys that don't exist, for instance:

	auth[:user][:first_name]
	nil
	
Whereas `fetch` will raise a `KeyError`, forcing you to catch the error before you store it:

	auth[:user].fetch(:first_name)
	> KeyError
	
This will keep us from making assumptions about data we don't have, and causing bugs in seemingly unrelated parts of our code.