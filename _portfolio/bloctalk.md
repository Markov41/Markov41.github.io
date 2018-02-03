---
layout: post
title: Bloccit
feature-img: "img/sample_feature_img.png"
thumbnail-path: "img/Bloccit-Landing.png"
short-description: A Reddit-inspired forum.

---
Bloccit is my first project using Ruby on Rails and is inspired by one of the world's most popular websites, Reddit. Users have the ability to sign up, sign in and out, and can be upgraded to moderator or admin status to have advanced privledges regarding the editing and deletion of posts and topics. 

The project is currently utilizing seeded random data, allowing the developer to have a glimpse into what a lively internet forum would look:

{% highlight ruby %}

module RandomData
    
    def self.random_name
        first_name = random_word.capitalize
        last_name = random_word.capitalize
        "#{first_name} #{last_name}"
    end
    
    def self.random_email
        "#{random_word}@#{random_word}.#{random_word}"
    end
    
    def self.random_paragraph
        sentences = []
        rand(4..6).times do
            sentences << random_sentence
        end
        sentences.join(' ')
    end
    
    def self.random_sentence
        strings = []
        rand(3..8).times do
            strings << random_word
        end
        
        sentence = strings.join(' ')
        sentence.capitalize << '.'
    end
    
    def self.random_word
        letters = ('a'..'z').to_a
        letters.shuffle!
        letters[0,rand(3..8)].join
    end
    
end

{% endhighlight %}

In addition to being a fully functional internet forum, Bloccit is also backed by TDD using Rspec. Shown below is an example which ensures that guests, members, moderations, and admins all have specific privledges available to them:

{% highlight ruby %}

require 'rails_helper'
 include SessionsHelper

 RSpec.describe CommentsController, type: :controller do
   let(:my_user) { create(:user) }
   let(:other_user) { create(:user) }
   let(:my_topic) { create(:topic) }
   let(:my_post) { create(:post, topic: my_topic, user: my_user) }
   let(:my_comment) { Comment.create!(body: 'Comment Body', post: my_post, user: my_user) }
 
   context "guest" do
     describe "POST create" do
       it "redirects the user to the sign in view" do
         post :create, params: { post_id: my_post.id, comment: {body: RandomData.random_paragraph} }
         expect(response).to redirect_to(new_session_path)
       end
     end
 
     describe "DELETE destroy" do
       it "redirects the user to the sign in view" do
         delete :destroy, params: { post_id: my_post.id, id: my_comment.id }
         expect(response).to redirect_to(new_session_path)
       end
     end
   end
 
   context "member user doing CRUD on a comment they don't own" do
     before do
       create_session(other_user)
     end
 
     describe "POST create" do
       it "increases the number of comments by 1" do
         expect{ post :create, params: { post_id: my_post.id, comment: {body: RandomData.random_sentence} } }.to change(Comment,:count).by(1)
       end
 
       it "redirects to the post show view" do
         post :create, params: { post_id: my_post.id, comment: {body: RandomData.random_sentence} }
         expect(response).to redirect_to [my_topic, my_post]
       end
     end
 
     describe "DELETE destroy" do
       it "redirects the user to the posts show view" do
         delete :destroy, params: { post_id: my_post.id, id: my_comment.id }
         expect(response).to redirect_to([my_topic, my_post])
       end
     end
   end

{% endhighlight %}