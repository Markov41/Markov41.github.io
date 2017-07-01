---
layout: post
title: BlocJams
feature-img: "img/sample_feature_img.png"
thumbnail-path: "img/BlocJamsAlbum.png"
short-description: BlocJams - Play your music anywhere!

---
Bloc Jams is a website inspired by existing sites such as Spotify, designed to allow users to browse through a list of albums with the ability to play songs directly through the website. The Bloc Jams project was created by using Angular, and also includes Jquery, as well as Buzz, a Javascript HTML5 Audio library, which have been incorporated throughout the environment. 

As my first real project, Bloc Jams represents the foundation of my programming abilities which I have since built upon. Featuring easy navigation and smooth animations, Bloc Jams is a visually appealing website. Functional play and pause buttons, along with song navigation and volume controls make for a user-friendly experience that visitors have become accustomed to. The ability to play a song and still be able to browse other sections of the website is a very convenient feature to avid listeners.

A key component to the feature above is learning the proper usage of templates by using `$stateProvider`

{% highlight javascript %}

$stateProvider
            .state('landing', {
                url: '/',
                controller: 'LandingCtrl as landing',
                templateUrl: '/templates/landing.html'
            })

{% endhighlight %}

Located in app.js, this acts as the home state that the site will initially display to the user. By providing addition templates, multiple pages can function via one index.html file.


One of the first steps in creating an Angular website is to create controllers. 

{% highlight javascript %}

 (function() {
     function LandingCtrl() {
         this.heroTitle = "Turn the Music Up!";
     }

     angular
         .module('blocJams')
         .controller('LandingCtrl', LandingCtrl);
 })();

{% endhighlight %}

The above controller sets the foundation for the LandingCtrl. 

Tasked to add functionality to the player bar, I created an additional controller and template. The `PlayerBarCrtl.js` allowed me to access the objects in which albums were located and route them directly to the user's views and controls. In `player_bar.html`, by referencing `ng-controller="PlayerBarCtrl as playerBar"` in `<section>`, the webpage is told to use the PlayerBarCtrl controller for this template.

Additionally, a service was introduced, called `SongPlayer.js` which controlled all of the features of the music player. By implementing functions which referenced the `currentBuzzObject` from the Buzz library, features such as volume control, next/previous/pause/play buttons, and song navigation were introduced. For example, to skip to the previous song in the current album, the following function was written:

{% highlight javascript %}

SongPlayer.previous = function() {
             var currentSongIndex = getSongIndex(SongPlayer.currentSong);
             currentSongIndex--;

             if (currentSongIndex < 0) {
                 stopSong(song);
             } else {
                 var song = currentAlbum.songs[currentSongIndex];
                 setSong(song);
                 playSong(song);
             }
         };

{% endhighlight %}
