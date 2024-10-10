# Learning to navigate in the command line

This project is to help learners get comfortable with working with text in the command line.

We don't use these often in web development, but having a basic awareness of them could be useful in future work.

## The commands we'll see:

- `echo`: display specified string in the terminal. In more complex scripts, we can use variable replacement in these strings.
- `cat`: display the contents of the specified file in the terminal. **Be careful. This output could be very long**
- `sort`: sort the input in various ways, including randomly
- `uniq`: reduce repeating values in the input to a single value
- `wc`: "word count". counts the number of characters, words, and lines in the input
- `grep`: a tool for searching on "patterns" (called regular expressions) in the input
- `|`: "pipe". sends the output of one command to the next command as input

## Getting started

- clone this project and have a go
- `cd [project]`
- `ls files` to see the resources available

## Pizza toppings filtering

There is a list of pizza toppings in `files/toppings.txt`, but it has duplicates and some _unwanted_ toppings (which are listed in `files/bad-toppings.txt`). We can clean this up the terminal.

### Building it up, in steps:

- `cat files/toppings.txt` to see the list of all toppings
- `cat files/toppings.txt | wc -l` to see how many toppings there are (since it's "one per line")
- `cat files/toppings.txt | sort` to sort them alphabetically. We can see 3 duplicates
- `cat files/toppings.txt | sort | uniq` to remove _repeating_ lines. Why not use `uniq` directly? Because _repeating_ lines need to be next to each other, and not separated by other strings.
- `cat files/toppings.txt | sort | uniq | grep "pepper"` to see all the lines that have "pepper" in them
- `cat files/toppings.txt | sort | uniq | grep -e "hotdog" -e "pineapple"` to see all the lines that have "bad toppings" in them
- `cat files/toppings.txt | sort | uniq | grep -v -e "hotdog" -e "pineapple"` to _invert_ the search (think of `-v` like "not these things")
- `cat files/toppings.txt | sort | uniq | grep -v -f files/bad-toppings.txt` to use "bad-toppings.txt" for searching (each line is like a new `-e`)
- `cat files/toppings.txt | sort | uniq | grep -v -f files/bad-toppings.txt | wc -l` to confirm we only have 15 toppings now.

## How many unique words in Alice in Wonderland?

The book [Alice's Adventures in Wonderland by Lewis Carroll](https://www.gutenberg.org/ebooks/11) is available to read for free on [Project Gutenberg](https://www.gutenberg.org). We can use the terminal to analyze some stats about it.

- `cat files/alice.txt | wc` to see how many lines, words, and characters there are in the book
- `cat files/alice.txt | grep -E -o "[A-Za-z0-9]+" | sort | uniq | wc -l` to see how many unique words are in the book
- `cat files/alice.txt | grep -E -o "[A-Za-z0-9]+" | sort | uniq -c | sort -r | head -100` to see the top 100 most-used words in the book

## How many times does the word “Alice” appear in the book?

`cat files/alice.txt | grep -E -o "[A-Za-z0-9]+" | sort | uniq -c | sort -r | head -100` shows "Alice" as being displayed 401 times

- `cat files/alice.txt | grep -Ei -o "alice" | wc -l` shows a total of 403 matches

403? Where are the extra 2 coming from? If we view the list, we'll find that "ALICE" (in all-caps) is used the extra times.

## How many countries end with “a”?

We can also use similar commands to get stats about country names.

- `cat files/countries.txt | grep "a$" | wc -l` to see how many countries end in "a" (70)
- `cat files/countries.txt | grep -v "a$" | wc -l` to see how many countries _don't_ end in "a" (125)
- `cat files/countries.txt | grep -i "can"` to see how many countries have "can" in their name (4)
- `cat files/countries.txt | grep -i "stan"` to see how many countries have "stan" in their name (7)
- `cat files/countries.txt | grep -i "land"` to see how many countries have "land" in their name (10)

We didn't actually need `cat` for this, and could have passed the file to `grep` directly:

- `grep -Ei "can" files/countries.txt`

---

## Other commands to explore later

- `tr`: "transform": changes characters in input to other characters
- `cut`: "cut out" selected portions of each line of input. Kind of like "split"
- `paste`: merges lines of input. Kind of like "join"
- `bc`: "basic calculator". takes a string of numbers and operators (+,-,\*,/) and returns the value
- `cal`: displays months in text form
- `date`: displays date. can also be used to display relative date in past or future
- `uptime`: displays last time your computer was restarted
- `pbcopy`: (macOS Only) "Pasteboard Copy": Copies to the system clipboard
- `pbpaste`: (macOS Only) "Pasteboard Paste": Pastes clipboard into terminal for further processing/display

## Rot-13/Caesar Cypher

"Rot-13" (short for "Rotate by 13") is a simple "Caesar Cypher" where each letter is swapped with the one 13 character away. It definitely shouldn't be used for security purposes, but can be a fun way to hide spoilers when talking about movies/books/etc. in social media.

Using `tr` this is very easy to do — each letter in the first group is swapped with the same-index letter in the second group.

- `echo Tarrant Hightopp | tr "abcdefghijklmnopqrstuvwxyz" "nopqrstuvwxyzabcdefghijklm"`
- `echo Tarrant Hightopp | tr "[A-Za-z]" "[N-ZA-Mn-za-m]"`
- `pbpaste | tr "[A-Za-z]" "[N-ZA-Mn-za-m]" | pbcopy` takes the current clipboard, transforms it, and copies it back to the clipboard for pasting into emails and chat windows.

## Sum the expense column

For a CSV ("comma-separated values") file with a column of numbers that needs to be summed, we can use a couple short commands to get the numbers column, join it into an equation, and feed it to the `bc` command.

- `cat files/expenses.csv | cut -d, -f2 | paste -sd+ - | bc`

## One final command: `curl`

`curl` isn't scary, but it does have more complexity than we can explore here. As you gain more experience in web development, you might work with other devs that test API endpoints using it. It allows for "reading" data from a service, and "creating", "updating", and "deleting" data on the service.

The simplest version looks like this:

- `curl https://www.gutenberg.org/cache/epub/11/pg11.txt` to get the "Alice in Wonderland" text

That could also be piped to our other commands. So, instead of using the included "alice.txt" file, we could have done this:

- `curl https://www.gutenberg.org/cache/epub/11/pg11.txt | grep -E -o "[A-Za-z0-9]+" | sort | uniq | wc -l` to see how many unique words are in the book

## Final notes

There are over 1000 commands available in the terminal. It's impossible to know all of them, but being aware of just the small handful we've looked at — and how to join them for more complex functions, might be useful in your future work.
