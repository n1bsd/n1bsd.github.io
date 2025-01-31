+++
slug = 'bear2hugo'
title = 'bear2hugo.py - Migrating from Bear Blog to Hugo'
date = 2024-08-16T19:10:00+00:00
draft = false
tags = ["Hugo", "Python"]
+++

I've written the following small Python script to migrate a [Bear Blog](https://bearblog.dev/) to [Hugo](https://gohugo.io/):

```
import csv
import os

with open('post_exports.csv') as csv_file:
    csv_reader = csv.reader(csv_file, delimiter=',')
    os.makedirs("./pages", exist_ok = True)
    os.makedirs("./posts", exist_ok = True)
    for row in csv_reader:
        dir = ""
        if row[11] == "True":
            dir = "pages/"
        else:
            dir = "posts/"
        with open(dir + row[4]+'.md', 'w') as post:
            publish = row[9]
            if publish == 'False':
                draft = "true"
            else:
                draft = "false"
            post.write("+++\n")
            post.write("slug = '" + row[4] + "'\n")
            post.write("title = '" + row[3] + "'\n")
            post.write("date = " + row[6] + "\n")
            post.write("draft = " + draft + "\n")
            post.write("tags = " + row[8] + "\n")
            post.write("+++\n")
            post.write(row[12])
```

It's not really sophisticated but it does the job. Here's all you need to know:

* Create a new directory, e.g. _~/bear2hugo/_
* Create a new file _bear2hugo.py_ in this directory and copy the above content into this file
* Download the export file from Bear Blog via the menu item "Export all blog data"
* Place the resulting file _post_exports.csv_ together with _bear2hugo.py_ in _~/bear2hugo/_
* Execute the script: _# python3 bear2hugo.py_
* It will create two new directories _pages_ and _posts_
* It will parse through CSV file and create a Mardown file for each post and page inside the corresponding directories
* You can now copy the files into your _content/_ and _content/blog/_ directory and use them with Hugo
