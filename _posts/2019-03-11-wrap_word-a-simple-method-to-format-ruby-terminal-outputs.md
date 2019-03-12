---
layout: post
title: "wrap_word: A Simple Ruby Method for Formatting CLI Output in Terminal"
date: 2019-03-11 16:24:59 -0500
categories: jekyll update
---

# The Problem:

Last week I was working on a project [(found here for those interested)](https://github.com/EliLauff/b_corporation_searchable_database) that is designed to demonstrate the use of ActiveRecord in modeling Many to Many relationships in Ruby. A main feature of my end product is an info page output in terminal that allows users to get a brief summary of a company that meets their search parameters. Before implementing a formatting method, this was the output in OSX terminal:

![Ew. Gross.](/photos/Before_Photo.png)
_*Almost* literally unreadable._

Obviously, I wasn't happy that one of my main features had "business" being truncated into "busine" and "ss". After a brief foray into Google, I decided to create a simple method of my own to force clean word wrapping in long strings. Because my project handled long-form input strings from a database with 5000+ entries, I couldn't manually edit each string. I needed a one-size-fits-all solution.

# The Solution:

My improvised solution took form in a method I call `wrap_word`. The goal of `wrap_word` is to take in a string and return it with carriage returns added at a predetermined given width. The carriage returns will only be added in between complete words and will take into account if the input string has carriage returns present already.

```ruby
def wrap_word(input_string, given_width = [default value])
    array_of_characters = input_string.split("")
    output_string = []
    counter_variable = 0
    array_of_characters.each do |character|
    #first check if the original character is a carriage return.
    #If so, reset the counter variable.
        if character == "\n"
            counter_variable = 0

    #if not, check if the counter variable is greater than the desired width,
    #also checking if the current character from the given string is a space.
    #if so, replace it with a carriage return.
        elsif counter_variable >= given_width && character == " "
            character = "\n"
            counter_variable = 0
        end
        output_string << character
        counter_variable += 1
    end
    return output_string.join("").to_s
end
```

The default value for `given_width` will change depending on the size of text you're using in Terminal as well as the resolution of your display. A simple trick for finding the default value for `given_width` follows:

1. Copy a string the width of your terminal window:
   ![Copy String](/photos/Copy_str.png)

2. Enter irb and run str.length on the string:
   ![str.length](/photos/Str_length.png)

3. Subtract about 15% from the value to allow for margins, and you have your default value! In my example, the total width was 119 characters (my terminal was greatly magnified!) and I ended up using a value of 105 for my default value. Use yours in the method definition.

# The Result:

I used this code to replace any longform string that would be output to my terminal. Whenever using `puts [string]` on a long string, the new syntax would be `puts wrap_word([string])`. Referring to the prior example of my project, the company descriptions output by `wrap_word` fit nicely on my terminal window:

![After Photo](/photos/after_photo.png)
_Clean and simple!_

This code could be improved upon by finding a more simple or automatic solution to the default value for `given_width`. Feel free to copy this code and improve upon it! After creating `wrap_word` I found that someone had created a ruby gem that seems to tackle the same problem from a different approach. Take a look at it [here](https://github.com/pazdera/word_wrap).
