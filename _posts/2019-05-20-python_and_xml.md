---
layout: post
title: Removing xml code with Python
image: /img/python_1.png
tags: [assignment]
---

# TNT - Assignment - Part I

># Special Thanks to Regina Jonach for sharing her solution online, which helped me tinkering
># my solution such that it worked out eventually :)```

># At the beginning the following packages need to get imported.
>  import os, sys
>  import re```
>
># The directory needs to be given.
>   path = "/home/sailormoon/Documents/STS/Individual/TNT/WebScraping/Richmond/"
>   listOfFiles = os.listdir(path)```
>
># This for loop loops through the list of fileNames in the given folder. I created an
>  additional list, where I point to each individual file, to then open it, read it, remove   
>  the xml code with regular expressions, and write the new text into a new xml file.```
> for fileName in listOfFiles:
>        newList = (path+fileName)
>        with open(newList, "r") as f1:
>            data = f1.read()
>            newData = re.sub("<[^<]+>", "", data)
>        newFileName = newList + "_modified.xml"
>        with open(newFileName, "wt") as f9:
>            f9.write(newData)```


![Learn Python](/img/python_1.png "Code")


