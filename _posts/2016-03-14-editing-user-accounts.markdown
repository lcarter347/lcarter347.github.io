---
layout: post
title:  "Editing User Accounts"
date:   2016-03-14 18:32:45 +0000
categories: buymybooks
---

# Editting user accounts with jQuery

I decided to add the ability to edit a user’s account to the BuyMyBooks site. I figured for the basic user account editing I would have buttons to toggle between edit and view mode, and that when you went into edit mode, the fields that contained your info would switch to input fields with that info already entered. I knew from my limited experience with it from a past class that jQuery could do this, and since I haven’t really had much experience with jQuery, I decided to use it for this purpose.


Toggling between viewing the info and a text field where you can edit it turned out to be fairly easy. I basically just had to alternate between the .hide() and .show() methods on button click events. What was a little more difficult was dealing with the situation where the text field wasn’t big enough to fit the pre-filled data. I eventually solved this by creating a function called resizeInput() that contained this jQuery call: `$(this).attr('size', $(this).val().length);`.
I call this function on all textfields on the keyup event, which is occurs when the user releases a key on the keyboard. This makes the size of the input field dynamic, responding to the size of user input. But then I realized this makes the text field go off the page if the user types enough, so I had to include a check to make sure that if the length of the input got too long, the text field would not continue to grow (I found that a cap of 60 kept the text field from encroaching on the done button).


Once I had that sorted out, actually implementing the update function was pretty straight forward: I just created hidden forms and made the buttons’ onClick attributes submit these forms. From there I just had to write the python code to update the database.