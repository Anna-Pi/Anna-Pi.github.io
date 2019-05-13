---
layout: post
title: WebScraping
image: /img/perseus.jpeg
tags: [assignment]
---

# What did I do to download the data?

As always, if something is a while ago, and not well documented, it tends to get forgotten. Especially, if I would have been the one to remember.
However, I try to recall my steps:

1. We opened the website: http://www.perseus.tufts.edu/hopper/collection?collection=Perseus:collection:RichTimes

1. By right-clicking my mouse we were able to open the Source Code of the Website.

1. By analytically screening through the website, we managed to find in which way the documents are referred to. (In a systematic way of course)

1. Since we learned how to use regular expressions, we could copy the whole source code into a sublime text, and then use a proper regular expression to select each reference to the document we wanted to. 

1. Again, we used regular expressions to modify this reference in a way, that we get the whole link. To then be able to do webscraping with wget.

1. wget is nice. One does not need to put each and every single link in the terminal, but can refer to a text document (in the follwoing called: links.txt), where each link is saved, to then download all. You can even give the destination (e.g. folderYouWantToSaveTo). The `-nc` in the end is helpful if you might not download the data all at once, but at several timepoints. This will make sure, that you don't scrape everything again, but only the stuff you have not saved yet.

1. The command is as follows: `wget -i links.txt -P ./folderYouWantToSaveTo/ -nc`



![Perseus](/img/perseus.jpeg "Perseus")




