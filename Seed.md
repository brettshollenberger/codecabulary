# seed.rb

	Movie.create([
		{name:  'Batman?', description: 'The Dark Knight Rises?'},
		{name: 'Man of Steel', description: 'A boring 90 minutes'},
	])
	
menu_items_spec.rb

	require 'spec_helper'
	
	describe Seeder do
		let(:seeder) { Seeders::MenuItems }
		
		it "seeds the database" do
			menu_item_count = MenuItem.count
			seeder.seed
			expect(MenuItem.count).to_not eq(menu_item_count)
		end
	end
	
lib/seeders/menu_items.rb

	module Seeders
		module MenuItems
			class << self
				def seed
					MenuItem.destroy_all
					menu_items.each do |name, array|
						item = MenuItem.new
						item.name = name
						item.category = array.shift
						item.price_in_cents = 1000
						item.description = "Yummy"
						item.save
					end
				end
				
				def menu_items {
					"Bonnie's Crab Cakes" => ['Seafood', 'lump crabmeat', 'breadcrumbs', 'seasoning'],
					"Item 2..."
				}
				end
			end
		end
	end
