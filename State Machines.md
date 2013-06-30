#State Machines

State machines represents a very big and complex topic in the world of computer science; navigating to the state machine gem's ReadMe file can make grown men cry. But state machines don't have to the be that complicated!

Take a simple example of moderating a comment for a blog post. The user is able to flag an inappropriate comment. That causes the comment to appear in a review section for the site admin. The site admin then decides whether he or she wishes to approve or remove the comment. Only approved comments are shown. 

First, install the state machine gem:

  gem 'state_machine'
  bundle
  
Don't forget to add <code>state</code> to your model (string).
  
Then edit your model to include states: 

  state_machine :initial => :approved do
    state :approved
      state :flagged
      state :removed

    event :flag do
      transition :approved => :flagged
    end

      event :approve do
          transition all => :approved
        end

      event :remove do
        transition :flagged => :removed
      end
    end
    
  Using the all transition means that you can approve a comment no matter what state your record is.
  
  Congratulations, you just created a state machine! Now you could perform actions such as:
    
    comment.approve
    comment.flag
    comment.remove
