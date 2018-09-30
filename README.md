### acts_as_votable
---

https://github.com/ryanto/acts_as_votable

```ruby
class AddCacheVotesToPosts < ActiveRecord::Migration
  def change
    t.integer :cached_votes_total, default: 0
    t.integer :cached_votes_score, default: 0
    t.integer :cached_votes_up, default: 0
    t.integer :cached_votes_down, default: 0
    t.integer :cached_weighted_score, default: 0
    t.integer :cached_weighted_total, default: 0
    t.float :cached_weighted_average, default: 0.0
  end
  # Post.find_each(&:update_cached_votes)
end

class AddCachedVotesToPosts < ActiveRecord::Migration
  def change
    change_table :post do |t|
      t.integer :cached_scoped_subscribe_votes_total, default: 0
      t.integer :cached_scoped_subscribe_votes_score, default: 0
      t.integer :cached_scoped_subscribe_votes_up, default: 0
      t.integer :cached_scopeed_subscribe_votes_down, default: 0
      t.integer :cached_weighted_subscribe_score, default: 0
      t.integer :cached_weighted_subscribe_total, default: 0
      t.float :cached_wighted_subscribe_average, default: 0.0
    end
  end
end

Post.order(:cached_weighted_average => :desc)
<%= post.weighted_average.round(2) %>  / 5


class Post < ActiveRecord::Base
  acts_as_votable cachable_strategy: :update_columns
end

```
```sh
gem 'acts_as_votable', '~> 0.11.1'
rails g acts_as_votable:migration
rake db:migrate

rake spec
```

```ruby
class Post < ActiveRecord::Base
  acts_as_votable
end
@post = Post.new(:name => 'my post!')
@post.save
@post.liked_by @user
@post.votes_for.size # => 1

@post.liked_by @user1
@post.downvote_from @user2
@post.vote_by :voter => @user3
@post.vote_by :voter => @user4, :vote => 'bad'
@post.vote_by :voter => @user5, :vote => 'like'

# positive
@post.liked_by @user1
@post.vote_by :voter => @user3
@post.vote_by :voter => @user5, :vote => 'like'
#negative
@post.downvote_from @user2
@post.vote_by :voter => @user2, :vote => 'bad'
#tally
@post.votes_for.size
@post.weighted_total
@post.get_likes.size
@post.get_upvotes.size
@post.get_dislikes.size
@post.get_downvotes.size
@post.weighted_score => 1

@post.votes_for.up.by_type(User)
@post.votes_for.down
@post.votes.up
@post.votes.down
@post.votes.up.for_type(Post)

@post.votes_for.up.by_type(User).voters
@post.votes_for.down.by_type(User).voters

@user.votes.up.for_type(Post).votable
@user.votes.up.votables

@post.liked_by @user1
@post.unliked_by @user1

@post.disliked_by @user1
@post.undisliked_by @user1


@post.liked_by @user1, :vote_scope => 'rank'
@post.vote_by :voter => @user3, :vote_scope => 'rank'
@post.vote_by :voter => @user5, :vote => 'like', :vote_scope => 'rank'

@post.downvote_from @user2, :vote_scope => 'rank'
@post.vote_by :voter => @user2, :vote => 'bad', :vote_scope => 'rank'

@post.find_votes_for(:vote_scope => 'rank').size
@post.get_likes(:vote_scoep => 'rank').size
@post.get_upvotes(:vote_scoep => 'rank').size
@post.get_dislikes(:vote_scope => 'rank').size
@post.get_downvotes(:vote_scope => 'rank').size

@post.vote_by :voter => @user1, :vote_scope => 'week'
@post.vote_by :voter => @user1, :vote_scope => 'month'
@post.vote_by_for(:vote_scope => 'week').size
@post.vote_by_for(:vote_scope => 'month').size

@post.liekd_by @user1, :vote_weight => 1
@post.vote_by :voter => @user3, :vote_weigth => 2
@post.vote_by :voter => @user5, :vote => 'liked', :vote_scope => 'rank', :vote_wight => 3

@post.downvote_from @user2, :vote_scope => 'rank', :vote_weight => 1
@post.vote_by :voter => @user2, :vote => 'vote' => 'bad', :vote_scope => 'rank', :vote_weight => 3

@post.find_votes_for(:vote_scope => 'rank').sum(:vote_weight)
@post.get_likes(:vote_scope => 'rank').sum(:vote_weight)
@post.get_upvotes(:vote_scope => 'rank').sum(:vote_weight)
@post.get_dislikes(:vote_scope => 'rank').sum(:vote_weight)
@post.get_downvotes(:vote_scope => 'rank').sum(:vote_weight)

class User < ActiveRecord::Base
  acts_as_voter
end

@user.likes @article

@aritcle.votes.size
@article.likes.size
@article.dislike.size

@user.likes @comment1
@user.up_votes @comment2

@user.voted_for? @comment1
@user.voted_for? @comemnt2
@user.voted_for? @comment3

@user.voted_as_when_voted_for @comment1
@user.voted_as_when_voted_for @comment2
@user.voted_as_when_voted_for @comment3

@user.likes @comment1
@user.dislikes @comment2

@user.voted_up_on? @comment1
@user.voted_down_on>? @comment1

@user.voted_down_on? @comment2
@user.voted_up_on? @comment2

@user.voted_up_on? @comment3
@user.voted_down_on? @comment3

@user.find_voted_items

@user.find_up_voted_items
@user.find_liked_items

@user.find_down_voted_items
@user.find_disliked_items

@user.get_votes Comment
@user.get_up_voted Comment
@user.get_down_voted Comment

@user.likes @shoe
@user.likes @shoe
@shoe.votes
@shoe.likes

@hat.liked_by @user
@hat.vote_registerd?

@hat.liked_by => @user
@hat.vote_registerd # => false

@hat.disliked_by @user
@hat.vote_registerd?

@hat.votes.size 
@hat.positives.size
@hat.negatives.size

@hat.vote_by voter: @user, :duplicate => true


```



