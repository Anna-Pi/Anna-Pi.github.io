---
layout: post
title: Structuring Data
image: /img/python_1.png
tags: [assignment]
---

# TNT - Assignment creating csv tsv json

Since I was ill (and still am a little...), and lost the last class, I got also lost in space ... The code here is by Bernhard - thank you so much for sharing!!! I haven't had the time and brain to look it through yet, but I tried it out and fixed some issues I hade (again mostly the encoding stuff). I am also not sure if my output is, what we want. But hopefully I will figure it out tomorrow.

And of course I am ashamed to post another persons code here, but I didn't want to have an empty post.

~~~~

import re, os, csv, json

source = "private"
target = "private"

lof = os.listdir(source)

list = []

for f in lof:
    if f.startswith("dltext"): # fileName test
        with open(source + f, "r") as f1:
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

## Writing the whole thing
# column names for the csv/tsv
csv_columns =  ['id', 'date', 'type', 'header', 'text']

# writing tsv
with open('dipatch.tsv', 'w') as f:
    writer = csv.DictWriter(f, delimiter ='\t',fieldnames=csv_columns)
    writer.writeheader()
    for data in list:
        writer.writerow(data)

# writing csv
with open('dipatch.csv', 'w') as f:
    writer = csv.DictWriter(f,fieldnames=csv_columns)
    writer.writeheader()
    for data in list:
        writer.writerow(data)

# writing json
with open("dispatch.json", "w") as f:
    json.dump(list, f)

~~~~
