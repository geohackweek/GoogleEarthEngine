---
title: "GEE Access and JavaScript Cheat Sheet"
teaching: 0
exercises: 0
questions:
- How do I get an account?
- What are some JavaScript basics?
objectives:
- Gain access to Google Earth Engine
- Have a JavaScript resource
keypoints:
- "Anyone can sign up for Google Earth Engine, just apply to be a trusted tester."
- "The Code Editor is a user friendly access point for Earth Engine that uses the JavaScript API."
- " JavaScript is a programming language that is also widely used in web development."
---
## Getting an Earth Engine Account

In order to use Google Earth Engine, you need to sign up for the platform. Use the steps below to sign up.

### Directions:

Go the the [GEE sign up page](https://signup.earthengine.google.com/#!/) and enter the email you want to use for your GEE account. A gmail is best if you have one. 

On the next page, enter in the information requested about your email, your affiliation, etc. Where it asks what you want to accomplish, you can mention you are at Geohackweek at the University of Washington.

Check your email, including your spam folder, for a link from the Google Developer's Team. The confirmation email will have directions on how to access the Code Editor.

Not sure if you have access? Use [this link](https://code.earthengine.google.com/) to check. If you didn't get access you will get an authorization error that says your account isn't registered when you click on the link and try to sign in. If you do have access, the link will open up the Javascript API. You can use this link in the future to get into the JavaScript API.

## Get Access to Module Scripts

Google Earth Engine allows you to have shared group folders/repositories for scripts. We've organized the code presented in this tutorial this way. Get access to the shared code repository in the code editor by:

- Join our Google group by clicking this link (don't worry about posting permissions; we won't  be posting anything. This allows you access to the repository): [https://goo.gl/maZwVa](https://goo.gl/maZwVa)
- Accept the shared repository by clicking this link: [https://code.earthengine.google.com/?accept_repo=geohackweek2017](https://code.earthengine.google.com/?accept_repo=geohackweek2017)

Now, in the code editor, go to the **Scripts tab** in the top left panel, scroll down and expand the "Shared" section, and a directory called *geohackweek2017* should appear with the full scripts from each episode. When Jill and I update them, the updates will be reflected in these versions of the code. 

### Getting help with access
If you have any trouble getting access to Google Earth Engine or the shared repository, you can slack jdeines or catherinekuhn with questions, or catch Jill at Geohackweek on Monday and Tuesday. 

## Javascript Tips

JavaScript, not to be confused with Java, is a dynamic, general-purpose programming language often used in web development alongside HTML and CSS. Google Earth Engine (GEE) can also be accessed through a JavaScript API, which is what we are using at GeoHack.

### JavaScript API Basics
It's not necessary to formally learn JavaScript before learning how to work with the GEE JavaScript API. Here are a few basics useful for GEE, reproduced from the [Earth Engine 101 Beginner's Curriculum](https://docs.google.com/document/d/1ZxRKMie8dfTvBmUNOO0TFMkd7ELGWf3WjX0JvESZdOE/edit).

{% highlight javascript %}
// Line comments start with two forward slashes. Like this line.

/* Multi-line comments start with a forward slash and a star,
and end with a star and a forward slash. */
{% endhighlight %}

Variables are used to store objects and are defined using the keyword **var**.
{% highlight javascript %}
var the_answer = 42;

// string objects start and end with a single quote
var my_variable = 'I am a string';

// string objects can also use double quotes, but don't mix and match
var my_other_variable = "I am also a string";
{% endhighlight %}

Statements should end in a semi-colon, or the editor complains.
{% highlight javascript %}
var test = 'I feel incomplete...'
var test2 = 'I feel complete!';
{% endhighlight %}

Passing function parameters and using lists
{% highlight javascript %}
// Parentheses are used to pass parameters to functions
print('This string will print in the Console tab.');

/* Square brackets are used for items in a list.
The zero index refers to the first item in a list*/
var my_list = ['eggplant','apple','wheat'];
print(my_list[0]); // would print 'eggplant'
{% endhighlight %}

Using dictionaries
{% highlight javascript %}
// Curly brackets (or braces) can be used to define dictionaries (key:value pairs).
var my_dict = {'food':'bread', 'color':'red', 'number':42};

// Square brackets can be used to access dictionary items by key.
print(my_dict['color']);

//Or you can use the dot notation to get the same result.
print(my_dict.color);
{% endhighlight %}

Functions can be defined as a way to reuse code and make it easier to read.
{% highlight javascript %}
var my_hello_function = function(string) {
  return 'Hello ' + string + '!';
};
print(my_hello_function('world'));
{% endhighlight %}


### Other JavaScript Resources
JavaScript uses camelCase. JavaScript (according to W3 academy) is easy to learn. Like other programming languages, you can use style guides to learn how to write standard, reproducible (marketable!) code.

For in-depth industry guidance, Google publishes their own definitive [JavaScript style guide](http://google.github.io/styleguide/jsguide.html).

Dana Tomlin has also created a short [JavaScript Quick Start Guide](https://drive.google.com/file/d/0B3H1GYZLzLKCckwwVjZfVmdPNDA/view) which only takes a few minutes to work through but hits some of the basics. You can find it clicking that link or by going to the GEE homepage, clicking on the EDU tab in the upper left, and scrolling down to the Geospatial Software Design Exercises section.

<br>
<img src="../fig/00_spaceland.png" border = "10">
<br><br>
