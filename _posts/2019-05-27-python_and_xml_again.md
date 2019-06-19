---
layout: post
title: Structuring Data
image: /img/python_1.png
tags: [assignment]
---

# TNT - Assignment creating csv tsv json


Inspired by Bernhard - thank you
~~~~

import re, os, csv, 

source = "private"
target = "private"

listOfFiles = os.listdir(source)

list = []

for f in listOfFiles:
    if f.startswith("dltext"): # fileName test
        with open(source + f, "r") as f1: # open the specific file, hence the f to read it, as f1
            text = f1.read()

            # looking for the date
            date = re.search(r'<date value="([\d-]+)"', text).group(1) # searching with regular expressions, getting a list therefore the group()

            # splitting the issue into articles/items
            split = re.split("<div3 ", text)

            c = 0 # item counter
            for s in split[1:]: # looping through the elements of this split
                c += 1
                s = "<div3 " + s 
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

                # creating a dict
                dict = {
                    'id': fName,
                    'date': date,
                    'type': unitType,
                    'header': header,
                    'text': text
                }
                # appending the dict to a list
                list.append(dict)

    # w
    # column names for the csv/tsv
    csv_columns =  ['id', 'date', 'type', 'header', 'text']

    # writing tsv
    with open('dipatch.tsv', 'w') as f:
    writer = csv.DictWriter(f, delimiter ='\t',fieldnames=csv_columns)
    writer.writeheader()
    for data in list:
        writer.writerow(data)

    #   writing csv
with open('dipatch.csv', 'w') as f:
    writer = csv.DictWriter(f,fieldnames=csv_columns)
    writer.writeheader()
    for data in list:
        writer.writerow(data)


~~~~
