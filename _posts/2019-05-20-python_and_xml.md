---
layout: post
title: Removing xml code with Python AND splitting the document
image: /img/python_1.png
tags: [assignment]
---

# TNT - Assignment - Part I



~~~~
 # At the beginning the following packages need to get imported.
import os, sys # os allows you to use the underlying operating system
import re # allows you to use regular expressions

 # The directory needs to be given.
path = "private" # I hid it, you don't need to know my directories ;) (it is important here to have the " " and I gave the full path - just to be sure
listOfFiles = os.listdir(path) # this function of the os lets you create a list of each file which is in this directory

 # This for loop loops through the list of fileNames in the given folder. I created an additional list, where I point to each individual file, to then open it, read it, remove the xml code with regular expressions, and write the new text into a new xml file.
for fileName in listOfFiles:
    
    newList = (path+fileName)  # to create a new list with paths to the actual files, which we then want to open 
    with open(newList, "r") as f1:  # with as is a good command as we learnt in the latest python tutorial, which allows you to open files in an easier way
        data = f1.read() # is basically our file saved to a variable (?or something like that,  thats the thing we are operating on)
        newData = re.sub("<[^<]+>", "", data) # we don't change our data, but create a new one, where we use re to substitute everything in < > by "nothing"
    newFileName = newList + "_modified.xml" # here we give the modified data a new name, which is the old name + _modified to not lose the overview
    with open(newFileName, "wt") as f9: # here we then write our new data to the file
        f9.write(newData)

~~~~


# TNT - Assignment - Part II

~~~~
import re, os  #same thing here

source = "private"
target = "private/Richmond_Articles/"   # we give a target folder right away (in the last exercise I created a folder manually)

lof = os.listdir(source) 
counter = 0 # general counter to keep track of the progress - makes sense, when handling such an amount of data

for f in lof: # for loop which loops through every file in our list of files
    if f.startswith("dltext"): # fileName test - to check that we don't do the operation on a random file, which might just be in the wrong folder or smth.       
        with open(source + f, "r") as f1:  # I deleted the encoding thing, because this always gave me an error
            text = f1.read()

            # try to find the date
            date = re.search(r'<date value="([\d-]+)"', text).group(1)  #I still struggle with the .group() command, I think its a list which gives you every value #you were searching for, and the number in the brackets is the index?

            # splitting the issue into articles/items
            split = re.split("<div3 ", text) # I must admit I didn't see that <div3 is the place where we wanted to split

            c = 0 # item counter
            for s in split[1:]: #also some sort of list and entries in the list, of what got splitted
                c += 1 
                s = "<div3 " + s # a step to restore the integrity of items
                #input(s)

                # try to find a unitType (wish I wouldn't have missed last class)
                try:
                    unitType = re.search(r'type="([^\"]+)"', s).group(1)
                except:
                    unitType = "noType"
                    print(s)

                # try to find a header
                try:
                    header = re.search(r'<head>(.*)</head>', s).group(1)
                    header = re.sub("<[^<]+>", "", header) # we want to get rid of the header, if there is a header...
                except:
                    header = "NO HEADER"   # this output happened quiet often when I run it. I think I don't have all the articles....
                    print("\nNo header found!\n")

                text = re.sub("<[^<]+>", "", s)
                text = re.sub(" +\n|\n +", "\n", text)
                text = re.sub("\n+", ";;; ", text)

                # generating necessary bits 
                fName = date+"_"+unitType+"_"+str(c)

                itemID = "#ID: " + date+"_"+unitType+"_"+str(c)  # this was done for the name-giving I think?
                dateVar   = "#DATE: " + date
                unitType = "#TYPE: " + unitType
                header = "#HEADER: " + header
                text = "#TEXT: " + text

                # creating a text variable
                var = "\n".join([itemID,dateVar,unitType,header,text])
                #input(var)

                # saving
                with open(target+fName+".txt", "w") as f9: # encoding="utf8"
                    f9.write(var)
 
        # count processed issues and print progress counter at every 100         (when I looked at the console I didn't see that, just the "NO HEADER")
        counter += 1
        if counter % 100 == 0:
            print(counter)

~~~~
