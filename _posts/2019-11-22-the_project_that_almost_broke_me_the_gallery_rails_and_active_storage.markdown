---
layout: post
title:      "The Project That ALMOST Broke Me: The Gallery(Rails & Active Storage) "
date:       2019-11-22 00:47:11 -0500
permalink:  the_project_that_almost_broke_me_the_gallery_rails_and_active_storage
---


This blog post has been a long time coming for me and to say that this project was a struggle would be an understatement. I've always been an overachiever for dreams and ambitions that would make the most successful of entrepeneurs envious. I came into Flatiron School with all of my project ideas planned out. I was excited and laser-focused and when I set out to do something I get tunnel-visioned and the only thing on my mind is getting things the way I want want them to be. 

When I finally got to my Ruby on Rails project I was really looking forward to implementing what I learned and bringing my idea to life. I would be lying if I said that was the case and that everything I had envisioned went according to plan, far from it. This whole project was a learning experience for me which lead me to developing a love/hate relationship with Rails. There is so much that you can do with this Ruby framework and it gives you so many built-in tools that make your development process a lot easier, they call it Rails Magic and with that came the first of my problems.

It's not that I had I had a hard time learning the concepts. Rails was a very great experience for me overall. The problem was me: my overambitiousness was my own downfall yet my biggest achievement in this project. A gift and a curse if you will.


## The Idea(Plan A)
I originally came into this project wanting to do an airbnb style clone . The inspiration for this came from my experience going to an anime convention with a few friends of mine( geeky me😅). One of the people in the group I went with had booked a room at the hotel that the convention was booked at. As the group continued to grow their were more and more people staying in the room. This sparked the idea to create an app where people could plan out their conventions or trips together as a group. 

I spent the majority of my time during project week just trying to plan out the foundation of the application: the database model. The more I thought about it and spent time planning it out I realized that the minimal viable product(MVP) had way more involved in getting it to where I wanted it to be than I had time to do in the one week I had available to me for my project. 

This forced me to switch gears and to completely start my project over from scratch.

## The Runner Up(Plan B & C)
My Plan B actually ended up turning into Plan C. It was still an idea that I wanted to build out as one of my project ideas but being de-railed ended up pushing this project further up my to-do list. Even that didn't work out so well. I ended up going with an MVP of what I though was my MVP of this project: Plan C.

## The Gallery 
The Gallery was originally meant to be a form of social media application and instead turned into, well... A photo gallery. Implementing this idea lead me down a road I had never been down in my brief time working with Ruby on Rails: Active Storage. Active Storage is a built-in gem that was introduced in Rails 5.2. This gem gives you the ability to handle file uploads in your application, something that became the crown jewel in my Rails project. Of course this wasn't a requirement let alone a topic covered in the curriculum at Flatiron School. This was something I chose to learn and make use of in my application. This brough along it's own host of complications as I wasn't able to get as much help and feed back when it came to debugging.


## Working with Active Storage
To install active storage you need to include ```gem 'image_processing', '~> 1.2'``` in your Gemfile and run ```bundle install```. You will then need to run the command ```rails active_storage:install ```. This will create a migration file called ```CreateActiveStorageTables``` as well as a file called ```storage.yml``` which is housed in your ```config``` file. 

After installing and set up I included the the macro  ```has_one_attached :image``` in my Photo model. This sets up a one-to-one mapping between the records and the files where each instance will have only.

From there  you can add a ```file_field``` in your form similar to the one below.

```<%= form_for [@user, @photo] do |f| %>```

``` <%= f.label :image %>```

```<%= f.file_field :image %>```

``` <% end %>```

Don't forget to allow your image through:

```params.require(:photo).permit(:image, :caption)```

You can then work with your image like you would with any other attribute in your views. In my case I used it display the image on my ```show``` and ```index``` pages. 

```<%= image_tag(@photo.image) if @photo.image.attached? %>```

## The Problems
##### Problem #1
The biggest problem I ran into wasn't because of Active Storage but this.

![](https://user-images.githubusercontent.com/5590958/29259722-ab76a370-8092-11e7-8e24-71044ff7e53f.png)

Normally in Rails you'll receive your errors in a format like this.

![](https://i.stack.imgur.com/4Wvgd.png)

Unfortunately this was not the case for me and this made debugging my project a lot more difficult the way I had to go about working through the errors I would receive was reading them in the terminal. Though not impossible it was an inconvenience that that definitely slowed me down and made the debugging process a lot more difficult.

##### Problem #2

This problem came after I implemented Active Storage. This was where I my love/hate relationship with Rails began. Active Storage created an ```ActiveStorage::DiskController```, an ```ActiveStorage::RepresentationsController``` and an ```ActiveStorage::DiskController```. 

Normally this would not have been an issue for me but the problem came from how the controllers where being used. The actions being done with these controller where implicit, I had no control over these processes and they made debugging a lot more difficult for me. As part of the requirements for this project I had to implement nested resources for my routes which were being handled in my ```PhotosController```.  As I was defining these actions I was receiving ```#show``` requests for an a route that I had commented out. This lead me to disabling the routes that were being used until I had the rest of the application working. This ended up being the best solution for me in this situation.

## Lessons Learned 

I learned so much during this project and negatives aside I would do this again. Even though this project almost broke me I learned to persevere and overcome obstacles as the appea. This in turn helped me to improve on my problem solving skills and learning a new technology, all skills that are important in the career of a software engineer. As frustrating as the difficult moments where I proved to myself that I could do this and continue to move forward.

 




