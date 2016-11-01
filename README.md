# Rspec Testing 

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


# initialize as RSPEC project

- if you want to initialize a project with `rspec` then type `rspec --init` 
- using rspec --init will setup RSpec within a ruby project

`rails g rspec­:install` this will install rspec into the current ruby project


# include matcher


# expect and change examples in RSPEC

        describe Zombie do
          it 'gains 3 IQ points by eating brains' do
            zombie = Zombie.new
            # zombie.iq.should == 0
            # zombie.eat_brains
            expect{zombie.eat_brains}.to change{zombie.iq}.from(0).to(3)
            # zombie.iq.should == 3
          end
        end

# use to should have 

        describe Zombie do
          it 'increases the number of tweets' do
            zombie = Zombie.new(name: 'Ash')
            zombie.tweets.new(message: "Arrrgggggggghhhhh")
            # zombie.tweets.count.should > 0
            zombie.should have(1).tweets
          end
        end

# expect and raise_error conditions

        describe Object, "#non_existent_message" do
          it "should raise" do
            expect{Object.non_existent_message}.to raise_error(NameError)
          end
        end
        
        #deliberate failure
        describe Object, "#public_instance_methods" do
          it "should raise" do
            expect{Object.public_instance_methods}.to raise_error(NameError)
          end
        end

# adding implicit subject

        describe Zombie do
          it 'should not be a genius' do
            # zombie = Zombie.new
            subject.should_not be_genius
          end
        end

- there is no need to initialize the object, since subject does that

# how to use IT'S

    describe Zombie do
      # it 'should have an iq of zero' do
      #  subject.iq.should == 0
      its(:iq) { should == 0 }
      end
    end

IT'S - This is basically `it` and `subject`. You can do everything in one line



# Its  vs it

when you to pass a function or method to your subject then you should use `its` otherwise `it` 

    describe Zombie do
      it { should_not be_genius }
      its(:iq) { should == 0 }
          
      context "with high IQ" do
        subject { Zombie.new(iq: 3) }
        it { should be_genius }
        its(:brains_eaten_count) { should == 1 }
      end  
      
    end

this will implicitly takes the subject defined defined above for `it`

# Subject vs Let 

    describe Zombie do
      let(:tweet) { Tweet.new }
      let(:zombie) { Zombie.new(tweets: [tweet]) }
      subject { zombie }
      its(:tweets) { should include(tweet) }
      its(:latest_tweet) { should == tweet } 
    end

subject block is now referencing the `Let` block defined above!

`newer` syntax


    describe Zombie do
      let(:tweet) { Tweet.new }
      subject(:zombie) { Zombie.new(tweets: [tweet]) }
      its(:tweets) { should include(tweet) }
      its(:latest_tweet) { should == tweet } 
    end


# lazy evaluation 

    describe Zombie do
      let(:tweet) { Tweet.new }
      let(:zombie) { Zombie.new(tweets: [tweet]) }
      subject { zombie }
      it 'creates a zombie' { Zombie.count == 1 } 
    end

step by step evaluation:

1 - examples begins to run 
2 - needs to know its subject 
3 - zombie gets created!! 

the above tests is going to `FAIL`

if you want to create a new zombie before every example then you need to do this


      let!(:tweet) { Tweet.create(tweets: [tweet]) }

this will create a new zombie object every time before the evaluation

# using BEFORE and AFTER in RSPECS

    describe Zombie do
      let(:zombie) { Zombie.new }
      subject { zombie }
      before { zombie.eat_brains }
      
      it 'is not a dummy zombie' do
        zombie.should_not be_dummy
      end
    
      it 'is a genius zombie' do
        zombie.should be_genius
      end
    end


    describe Zombie do
      let(:zombie) { Zombie.new }
      before { zombie.iq = 0 }
      subject { zombie }
    
      it { should be_dummy }
    
      context 'with a smart zombie' do
        before { zombie.iq = 3 }  
        it { should_not be_dummy }
      end
    end

if you're defining a new `context` then the before conditions should go inside it

# difference between Describe and Context in Rspec

There is not much difference between describe and context. The difference lies in readability. I tend to use context when 
I want to separate specs based on conditions. describe I use to separate methods being tested or behavior being tested.
One main thing that changed in the latest RSpec is that context can no longer be used as a top-level method. 

    describe Zombie do
      let(:zombie) { Zombie.new }
      subject { zombie }
        
      context 'with a dummy zombie' do
        before { zombie.iq = 0 }
        it { should be_dummy }
      end
    
      context 'with a smart zombie' do
        before { zombie.iq = 3 }
        it { should_not be_dummy }
      end
      
    end

# defining shared examples in rspecs

    shared_examples_for 'the brainless' do
      it { should be_dummy }
      it { should_not be_genius }
    end
    
    describe Zombie do
      let(:zombie) { Zombie.new }
      subject { zombie }
      it_behaves_like 'the brainless'
    end
    
    describe Plant do
      let(:plant) { Plant.new }
      subject { plant }
      it_behaves_like 'the brainless'
    end


# METADATA WITH FILTER 

    describe Zombie do
      let(:zombie) { Zombie.new }
      subject { zombie }
    
      context 'with a dummy zombie' do
        before { zombie.iq = 0 }
        it { should be_dummy }
      end
    
      context 'with a smart zombie', focus: true do
        before { zombie.iq = 3 }
        it { should_not be_dummy }
      end
    end

Add the focus tag to the 'with a smart zombie' context block. This way we can run $ rspec --tag focus and just run these examples.

Run the rspec command that will run only the specs tagged with dumb in the spec/models/zombie_spec.rb file.
`rspec --tag­ dumb spec/­models/zom­bie_spec.rb`

`rspec --tag­ ~dumb­ spec/­models/zom­bie_spec.rb` if you want to skip all the rspecs tagged with `dumb`











































































































































































 


















  
  























































































  

















