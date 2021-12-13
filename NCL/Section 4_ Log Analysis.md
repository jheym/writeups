# Basic text processing in Linux

## Intro

By themselves, these linux tools can't accomplish much. But when used together, the possibilities for what we can accomplish with data processing are endless. Naturally, these tools are used by experts in fields from data science to artificial intelligence. Familiarizing yourself with these tools will unlock a new level of competency for you, and give you a leg up on the competition. Other data filtering tools not mentioned here but are worth mentioning are `sed`, `grep`, and `awk`.

## Prerequisites

- Understand how piping commands works in linux
- Know how to use the `cat` command

## The `cut` command [\[man cut\]](https://www.man7.org/linux/man-pages/man1/cut.1.html)

- **Example**: `cut -d " " -f 1`
    In this example, the cut command defines a delimiter `-d " "` and selects the first field `-f 1.` In other words, the cut operation will find the first instance of a space and then cut everything after that space, for every line. The way field parameter works is that each delimiter (`" "` in this case) creates a new field and you can decide which fields to keep. `> out.txt` will cat the output of the previous piped commands into a file called out.txt instead of ouputting to the shell.

## The `sort` command [\[man sort\]](https://man7.org/linux/man-pages/man1/sort.1.html)

If using the sort command without any arguments, its default sort rules are as follows:

- numbers > letters
- lowercase > uppercase

### Example

If we have a list like

```
Alice
alice
52
2aflaed241f29
245
2a5
1200
```

The default sort rules would output this

```
1200
245
2a5
2aflaed241f29
52
alice
Alice
```

Notice that the numbers don't seem to be in order. The default sort compares each character individually. To sort numbers by integer order, you can use `sort -n`.

## The `uniq` command [\[man uniq\]](https://www.man7.org/linux/man-pages/man1/uniq.1.html)

The running some text through the default `uniq` command will output the text file without duplicate lines.
Another useful argument, `uniq -c` will output each line prepended with a count of how many times that line occurred. This is useful if you want to analyze the frequency of certain lines in your data.

## The `wc` command [\[man wc\]](https://www.man7.org/linux/man-pages/man1/wc.1.html)

Wc stands for wordcount, but the `wc` command can count more than just words. In many cases with text processing and analysis, we want a line count. `wc -l` will do just that. Check out the man link above for more things wc can do.

## Putting it all together
Here's the structure of a sample `access.log` file from an IIS webserver
```
02:49:12 127.0.0.1 GET / 200
02:49:35 127.0.0.1 GET /index.html 200
03:01:06 127.0.0.1 GET /images/sponsered.gif 304
03:52:36 127.0.0.1 GET /search.php 200
04:17:03 127.0.0.1 GET /admin/style.css 200
05:04:54 127.0.0.1 GET /favicon.ico 404
05:38:07 127.0.0.1 GET /js/ads.js 200
```
If we wanted to get the frequency of each http status code we would use a command like
`cat access.log | cut -d " " -f 5 | sort -n | uniq -c`
Which would return
```
5 200
1 304
1 404
```
So there are 5 occurences of 200 status codes, 1 occurance of a 304, and 1 occurance of a 404.