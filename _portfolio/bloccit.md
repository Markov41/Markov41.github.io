---
layout: post
title: Bloccit
feature-img: "img/sample_feature_img.png"
thumbnail-path: "img/Bloccit-Landing.png"
short-description: A Reddit-inspired forum.

---
Bloccit is my first project using Ruby on Rails and is inspired by one of the world's most popular websites, Reddit. Users have the ability to sign up, sign in and out, and can be upgraded to moderator or admin status to have advanced privledges regarding the editing and deletion of posts and topics. The project is currently utilizing seeded random data, allowing the developer to have a glimpse into what a lively internet forum would look.

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
