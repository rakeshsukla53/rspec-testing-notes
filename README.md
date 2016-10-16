# Rspec Testing (50 new things)

# Predicate matchers using should and be

	describe 'zombie' do 
	  it 'it should be true' do
	    zombie.hungry?.should == true
	  end
	end

this line can be refactored into `zombie.hungry?.should be_true`

another `be` matcher example here

	describe Tweet do
	  it 'truncates the status to 140 characters' do
	    tweet = Tweet.new(status: 'Nom nom nom' * 100)
	    tweet.status.length.should be <= 140
	  end
	end

usage of `equality` and `should` condition

	describe Tweet do
	  it 'without a leading @ symbol should be public' do
	    tweet = Tweet.new(status: 'Nom nom nom')
	    tweet.status.should == 'Nom nom nom'
	  end
	end

# test pending 

If you want your test to be pending, it you can use the following two different ways

- use `xit` instead of `it`

		xit 'this test should be pending' do
		  # this test is pending
		end

- use `pending` keyword inside the it condition

		it 'it should be named ashed' do
		  pending
		end  

# describe class 

	describe Tweet do 
	end

- full name of the `Class` should be used  

# rspec installation and configuration

	group :development, :test do
	  gem 'rspec-rails'
	end

`rspec rails` should be in the development and test environment

- `rails generate rspec:install`

check out the `spec_helper.rb` file for the configuration info

# check element part of an array

`zombie.tweets.should include(tweet2)`

`tweet2` should be a part of `zombie.tweets` method

`include` can be used anywhere where you want to check if the `item` is a part of an array.

# check count using have method

`zombie.should have(2).weapons`

basically it is equivalent to `zombie.weapons.count.should == 2` 
it counts the number of the element in the array

`have(n)
have_at_least(n)
have_at_most(n)`

these are the different variations of have that can be used


# expect and change method 

`change` method is most fun one

`expect { zombie.save }.to change { zombie.count }.by(1)`

you can also chain the following methods

`by(n)
from(n)
to(n)`

`.from(1).to(2)`

# raise_error in RSPEC

you can use `expect` and `	raise_error` to check for any errors validation

		expect { zombie.save! }.to raise_error( ActiveRecord::RecordInvalid )

you can modify the behaviour using

`to
not_to
to_not`  are the different modifiers

# More Matchers

	respond_to(method_name)
	be_within(range).of(expected)
	exist
	satisfy { block }
	be_kind_of(class)
	be_an_instance_of(class)

	
# respond_to method in rspec

the `respond_to` method in rspec checks if the method is associated with the object

`zombie.should respond_to(:name)`

here we are checking if the zombie object responds back with the `name` method


# subject variable in rspec
	describe Zombie do 
	  it 'responds to name` do 
	    subject.should respond_to(:name)
	  end
	end

here subject refers to the `Zombie` class. `subject` uses whatever classname you have described so far.

`subject = Zombie.new` it automatically instantiate the Zombie object

this will only work, if the `describe` block has a `classname`


# curl braces in RSPEC

you can refactor lot of codes using curl braces in rspec

	describe Zombie do
	  it 'responds to name' { should respond_to(:name) } 
	end

this entire code can be refactored into

	describe Zombie do 
	  it { should respond_to(:name) }
	end

rspec is smart to completely understand this

# its in RSPEC

you can use `its` to mention the `class name` for example:

	describe Zombie do
	  its(:name) { should == 'Ash' }
	  its(:weapons) { should include(weapons) }
	  its('tweet.size') { should == 2 }
	end

# lazy evaluation in Rspec

	describe Zombie do 
	  let(:zombie) { Zombie.create } # lazy evaluation 
	  subject { zombie }
	  its(:name) { should be_nil }
	end

in `lazy evaluation` the object will not get created until or unless it is yet called

if you user `let!(:zombie) { Zombie.create }` now it will get created before the example is called!

# hooks and tags

use of `before ` and `after` 

	describe 'Zombie' do 
	  let(:zombie) { Zombie.new }
	  before { zombie.hungry! } # this will run before each example
	  
	  it 'is hungry' do 
	    zombie.should be_nil
	  end 
	end
you can use `before` and `after` with all the following variations

`before(:each)` # run before each example
`before(:all)` # run once before all
`after(:each)` # run after each example
`after(:all)` # run after all example

 when i say example, it means the `IT` statement




















 


















  
  























































































  

















