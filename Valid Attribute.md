Valid attribute doesn't look for the specific implementation of validations as shoulda does.

	it { should_not allow_value('').for(:name) }
	it { should_not allow_value(nil).for(:name) }
	it { should allow_value("Smurfette").for(:name) }