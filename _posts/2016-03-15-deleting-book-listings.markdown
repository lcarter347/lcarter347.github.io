---
layout: post
title:  "Deleting Book Listings"
date:   2016-03-15 20:05:23 +0000
categories: buymybooks
---

# Deleting book listings

My next order of business was to implement the ability to delete listings. The main challenge here was that I wanted to confirm with the user before deleting.


I realized pretty quickly that I could do this with a javascript alert box, but this wasn’t very pretty or professional looking. I knew I could create a decent 
looking modal dialog with bootstrap, but when I tried this it didn’t work. The dialog wouldn’t pop up. After some digging, I finally realized this was because 
though I had bootstrap.js included in my project, I never included it in my html. Once I did this, it worked. (I should confess that while I have used Bootstrap’s 
css features before, this was my first experience using its javascript features. I had just assumed since bootstrap came with my flask template that the creator 
would have already done this, but I guess not.)


What proved more difficult was trying to find a way for the modal confirmation to submit a form with a hidden text field containing the primary key (id) of the 
book to be deleted. After a lot of trial and error, I finally figured out how to make it work:

+ I had to set the data-id attribute of the delete button to the id of the book (each listing has its own button). 

+ I created a jQuery event handler that is triggered by the modal dialogue popping up. Within this handler, I was able to access the data-id attribute 
and then set the value attribute of the hidden text field to this id.

The full code for this event handler is:

```
$('#confirmdelete').on('show.bs.modal', function(e) {
        var bookId = $(e.relatedTarget).data('id');
        document.getElementById("itemtodelete").setAttribute("value", bookId);
        
});
```

+ Finally, I was able to submit the form through the onClick attribute of the delete button in my modal dialogue. Phew.


I think my code is sound now. I just need to set up a cascading delete constraint in order to test it.