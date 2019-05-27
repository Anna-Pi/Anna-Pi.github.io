---
layout: post
title: Removing xml code with Python AND splitting the document
image: /img/python_1.png
tags: [assignment]
---

# TNT - Assignment - Part I

Programming needs a lot of patience, which I sometimes don't have. I really don't like that about me, and I try to get more patient with programming and myself. However, for this exercise it didn't work out. After trying it out myself, I got tempted by solutions I found (from my collegues and from the lecturer). Therefore, a special thank you to Regina J. and of course our lecturer. However, I went through the codes to trying to comprehend each step!

~~~~
 # At the beginning the following packages need to get imported.
import os, sys
import re

 # The directory needs to be given.
path = "private"
listOfFiles = os.listdir(path)

 # This for loop loops through the list of fileNames in the given folder. I created an additional list, where I point to each individual file, to then open it, read it, remove the xml code with regular expressions, and write the new text into a new xml file.
for fileName in listOfFiles:
    
    newList = (path+fileName)
    with open(newList, "r") as f1:
        data = f1.read()
        newData = re.sub("<[^<]+>", "", data)
    newFileName = newList + "_modified.xml"
    with open(newFileName, "wt") as f9:
        f9.write(newData)

~~~~


# TNT - Assignment - Part II

~~~~
import re, os

source = "private"
target = "private/Richmond_Articles/"

lof = os.listdir(source)
counter = 0 # general counter to keep track of the progress

for f in lof:
    if f.startswith("dltext"): # fileName test        
        with open(source + f, "r") as f1:  #, encoding="utf8"
            text = f1.read()

            # try to find the date
            date = re.search(r'<date value="([\d-]+)"', text).group(1)

            # splitting the issue into articles/items
            split = re.split("<div3 ", text)

            c = 0 # item counter
            for s in split[1:]:
                c += 1
                s = "<div3 " + s # a step to restore the integrity of items
                #input(s)

                # try to find a unitType
                try:
                    unitType = re.search(r'type="([^\"]+)"', s).group(1)
                except:
                    unitType = "noType"
                    print(s)

                # try to find a header
                try:
                    header = re.search(r'<head>(.*)</head>', s).group(1)
                    header = re.sub("<[^<]+>", "", header)
                except:
                    header = "NO HEADER"
                    print("\nNo header found!\n")

                text = re.sub("<[^<]+>", "", s)
                text = re.sub(" +\n|\n +", "\n", text)
                text = re.sub("\n+", ";;; ", text)

                # generating necessary bits 
                fName = date+"_"+unitType+"_"+str(c)

                itemID = "#ID: " + date+"_"+unitType+"_"+str(c)
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

        # count processed issues and print progress counter at every 100        
        counter += 1
        if counter % 100 == 0:
            print(counter)

~~~~
