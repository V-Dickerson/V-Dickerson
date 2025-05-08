---
layout: post
title:  "A Deep Dive into Databases, Part 1: Prologue"
date:   2025-05-08 06:05:00 -0500
categories: jekyll update
---

<p class="lead">
Within my work, I've been working with databases daily for a little over the last year now.
</p>

---

Learning about the possibility of highly-available data in the cloud was a **GAMECHANGER** for me.  
However, I didn't always know about them.

When I was a kid, databases seemed unreachable — I would work on a project, make something cool, and get right up to the point where a database would be needed to handle a problem (e.g. store user account information across devices), but I didn't have the language to express what I didn't know.  
Databases were just this mystical big-tech buzz word to me.

---

I eventually got to the point of learning to build APIs and host a server.  
I wrote an application that communicated with a Flask API that would return predictions from a machine learning model I trained to predict whether or not a link was a phishing attempt.

> At its launch, it worked in a very inoptimal way — it would load Parquet files from local storage and choose a random value.

---

Then, leveraging projects and my technical experience from classes so far, I got a job where I had the opportunity (as well as the daily requirement) to work with databases.  
At that point in my degree, I still hadn't come into contact with a database — my first time would be in the first semester of my Junior year in my Intro to Software Engineering class.

Since then, I've become fairly proficient at knowing how to create, read, update, or delete relevant data.

However, lately I've been looking to learn more about the type of **enterprise backend systems** that I would like to design in my career.  
I was recommended *Designing Data Intensive Applications* by **Martin Kleppman**.

---

Starting this reading, I realized how much I didn't know — in the same way as I didn't know that databases weren't solely relegated to the YouTubes and Mojangs (the 2 largest companies in my head as a kid) of the world, I realized that I could not explain how the databases that I work with on a daily basis operate under-the-hood.

After reading a good portion of *Designing Data Intensive Applications*, I have a much better idea of what I don't know now.

> I've only developed with B-Trees in my Sophomore year Data Structures & Algorithms course,  
> and the concept of memory pages recalls flashbacks from the horrors of my Intro to Computer Systems course.

---

## Goal

My goal is to start simple, and document my learning process from 0 to 100 — implementing more and more features of a database as I learn.

> Will I be able to store and retrieve persistent memory across tables?  
> Will I be able to expose a connectable interface to my database?  
> Will I implement sharding?  
> Will I ever not shudder at introductory concepts from Computer Systems?

Find out with me! 🙂

To christen my journey, I'll start with **"Level 0"** as described in DDIA and explain how it works.

---

## Level 0: Bash Database

{% highlight bash %}
#!/bin/bash
db_set () {
    echo "$1,$2" >> database
}
db_get () {
    grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}
{% endhighlight %}

This script is written in **bash**, the language natively used to interact with Linux terminals.

### Breaking down each part:

---

#### `db_set`:

- `echo`: outputs something  
- `"$1,$2"`: defines a string made from 2 input variables, then separates them by a comma  
- `>>`: puts the output of the left side into the file on the right  
- `database`: just the name of the file

Combined:
```bash
echo "$1,$2" >> database
````

Outputs the 2 provided input variables separated by a comma into a file named `database`.

**Example:**

```bash
db_set Vanessa '{"age": 21, "major": "Computer Science", "favorite_color": "Lilac"}'
```

Now, the file named `database` contains:

```
Vanessa,{"age": 21, "major": "Computer Science", "favorite_color": "Lilac"}
```

---

#### `db_get`:

* `grep a b`: searches for all instances of `a` in `b`
* `"^$1,"`: RegEx pattern matching the string starting with the variable followed by a comma
* `|`: pipe operator — passes output from one command to the next
* `sed`: applies a pattern to an input
* `-e`: specifies an expression to apply with sed
* `sed -e "s/$1,//"`: removes the key and comma from each matching line
* `tail -n 1`: selects the last line only

Final logic:

```bash
grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
```

This:

1. Finds all lines in the file `database` where the line starts with the given key.
2. Removes that key and its comma.
3. Returns only the most recent value.

---

This is an example of **key-value storage**, and it works because:

* every `db_set` operation adds a new line with an updated value
* `db_get` finds all lines with that key, strips the key, and picks the last one

---

This is not particularly helpful to the type of database I intend to make,
but it is **certainly a start**!

**Stay tuned for part 2!** :)