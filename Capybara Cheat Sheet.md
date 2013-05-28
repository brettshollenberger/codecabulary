# Capybara Cheat Sheet

Capybara comes with a [Domain-Specific Language (DSL)](google.com) for writing descriptive [acceptance tests](google.com). A domain-specific language is a language created to solve a particular set of problems--in the case of specs, the DSL describes the expected functionality of an application in various situations. 

There are several DSLs dedicated to writing specs in Ruby, including [RSpec](https://github.com/brettshollenberger/ruby_wiki/blob/master/Setting%20Up%20RSpec.md) and Test Unit. Capybara's DSL solves an even more granular problem: Writing acceptance tests.

For that reason, Capybara's DSL extends the most popular testing DSLs, Cucumber, [RSpec](https://github.com/brettshollenberger/ruby_wiki/blob/master/Writing%20Specs%20in%20RSpec.md), Test Unit, and Mini Test. 

Since Capybara's DSL is an extension, the underlying spec DSL can also be used in writing acceptance tests, although the Capybara language will be more useful for writing acceptance tests. 

#### Navigating
    visit('/projects')
    visit(post_comments_path(post))

#### Clicking links and buttons
    click_link('id-of-link')
    click_link('Link Text')
    click_button('Save')
    click('Link Text') # Click either a link or a button
    click('Button Value')

#### Interacting with forms
    fill_in('First Name', :with => 'John')
    fill_in('Password', :with => 'Seekrit')
    fill_in('Description', :with => 'Really Long Text…')
    choose('A Radio Button')
    check('A Checkbox')
    uncheck('A Checkbox')
    attach_file('Image', '/path/to/image.jpg')
    select('Option', :from => 'Select Box')

#### Scoping
    within("//li[@id='employee']") do
      fill_in 'Name', :with => 'Jimmy'
    end
    within(:css, "li#employee") do
      fill_in 'Name', :with => 'Jimmy'
    end
    within_fieldset('Employee') do
      fill_in 'Name', :with => 'Jimmy'
    end
    within_table('Employee') do
      fill_in 'Name', :with => 'Jimmy'
    end

#### Querying
    page.has_xpath?('//table/tr')
    page.has_css?('table tr.foo')
    page.has_content?('foo')
    page.should have_xpath('//table/tr')
    page.should have_css('table tr.foo')
    page.should have_content('foo')
    page.should have_no_content('foo')
    find_field('First Name').value
    find_link('Hello').visible?
    find_button('Send').click
    find('//table/tr').click
    locate("//*[@id='overlay'").find("//h1").click
    all('a').each { |a| a[:href] }

#### Scripting
    result = page.evaluate_script('4 + 4');

#### Debugging
    save_and_open_page

#### Asynchronous JavaScript
    click_link('foo')
    click_link('bar')
    page.should have_content('baz')
    page.should_not have_xpath('//a')
    page.should have_no_xpath('//a')

#### XPath and CSS
    within(:css, 'ul li') { ... }
    find(:css, 'ul li').text
    locate(:css, 'input#name').value
    Capybara.default_selector = :css
    within('ul li') { ... }
    find('ul li').text
    locate('input#name').value