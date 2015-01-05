# Null Object Pattern

Well-meaning code can often get itself into quite a mess when nil values are involved. Nil is like the concept `null`, which means "the absence of a value" or "the value is unknown." The problem lies in methods that expect a certain input type, but get nil instead:

	def log(msg, logger=nil)
		if logger && logger.info logger.info msg
	end
	
The `log` method wants a logger, but if no logger is provided, it ends up with `nil`. The problem here is that the expected `logger` would respond to the `info` method, and `nil` doesn't have an `info` method--we end up with a loud `NoMethodErrror`, but we didn't want our logger to break--we just wanted it to do nothing.

The problem with `nil` is that it is too generic for many use cases. We don't want a general absence of value as an input--we want the concept of "no logger" as an input. I'm thinking of `NoLogger` as `/dev/null`--the UNIX Null I/O Device, which takes messages as input like a standard I/O device, and promptly discards them. Its stream is `NoStream`. It outputs to `Nowhere`. These may seem like funny questions to ask a non-existent device, but they are precisely the type of questions you would ask about `/dev/null`. You would not ask `/dev/null` what its account balance is, but you might rightly ask the `NoBankBalance` class.

I'm talking about modeling different concepts of nothing because they are different. We always have domain logic for the "happy path," where everyone has money in the bank and I/O devices to write to, but we often fail to treat the absence of these objects as paradigms to describe in and of themselves. `NoDevice` takes messages and discards them. `NoBalance` has a `balance` of 0, and `pretty_print`s its statement to `$0.00`. These are distinct concepts within their domain for fleshing out the edge cases.