<!---
{
  "id": "b668e389-98db-4777-bc4c-190de535836c",
  "depends_on": ["e46ffb8b-00d6-44a2-ad40-552ea03b4e3a"],
  "author": "Stephan Bökelmann",
  "first_used": "2025-06-03",
  "keywords": ["shell", "pipelines", "grep", "wc", "sort", "uniq", "filters", "stdin", "stdout", "command chaining", "tr"]
}
--->

# Interacting with the Shell: Pipelines and Filters

> In this exercise you will learn how to chain commands together using pipelines to process data in streams. Furthermore we will explore several important text-processing tools like `grep`, `wc`, `sort`, `uniq`, and `tr` that are fundamental to working efficiently in the shell.

## Introduction

In the previous exercise, you have learned that many shell commands read from **stdin** and write to **stdout**. You also learned how to redirect output into files and how to group commands using parentheses.

But the true power of the Unix philosophy lies in the ability to chain small programs together to accomplish complex tasks. This is possible because most shell commands are designed to work with text streams. The key tool that allows us to connect these commands is the **pipeline operator**: `|`.

The pipeline redirects the `stdout` of one command directly into the `stdin` of the next command, without writing intermediate results to disk. This allows highly efficient data processing with minimal overhead.

In this exercise, we will introduce a few classic **filter programs** that operate on these data streams:

* `grep`: Searches for lines that match a given pattern. It takes a search pattern (often a regular expression) and filters the input, printing only lines that contain the pattern.
* `wc`: Counts lines, words, and bytes (or characters). It is useful for getting quick summaries of file content or pipeline output.
* `sort`: Sorts lines of text alphabetically or numerically. Sorting is often used before applying `uniq` to ensure duplicates are grouped together.
* `uniq`: Removes or counts duplicate lines from sorted input. Without prior sorting, `uniq` only removes consecutive duplicates.
* `tr`: Translates or deletes characters from the input stream. For example, it can replace spaces with newlines to split text into words.

Each of these programs does one thing well, and together they form the foundation of many advanced shell workflows.

### Why Filters Matter

Historically, computers had very limited memory and disk space. The ability to process data line-by-line, without loading everything into memory, made Unix systems highly efficient. Even today, this approach scales surprisingly well for large data sets or log files.

By mastering pipelines and filters, you are learning a style of programming that emphasizes:

* **Modularity**: Each program is simple and focused.
* **Composability**: Programs can be combined easily.
* **Efficiency**: Streaming avoids unnecessary disk I/O.

### Further Readings and Other Sources

* [The Art of Unix Programming, Chapter 7](http://www.catb.org/~esr/writings/taoup/html/ch07s02.html)
* [POSIX Shell Command Language](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18)
* [YouTube: Linux Command Line Basics - Pipes and Redirection](https://www.youtube.com/watch?v=3tq4t23BzXc)

## Tasks

### Task 1: Prepare some data files

We will first create some simple text files to work with:

```bash
echo -e "apple\napple\nbanana\napple\norange\nbanana\napple" > fruits.txt
echo -e "the quick brown fox\njumps over the lazy dog\nthe quick blue hare\nthe slow red turtle" > sentences.txt
```

Use `cat` to inspect the files:

```bash
cat fruits.txt
cat sentences.txt
```

### Task 2: Use `grep` to filter text

`grep` searches for lines containing a specific substring or pattern.

```bash
grep apple fruits.txt
grep quick sentences.txt
grep -i Quick sentences.txt  # -i ignores case
```

### Task 3: Use `wc` to count lines, words, and characters

`wc` stands for "word count". It can count the number of lines, words, and characters in a file or input stream.

```bash
wc fruits.txt
wc -l fruits.txt  # only count lines
wc -w sentences.txt  # only count words
```

### Task 4: Chain commands using pipelines

Use `grep` and `wc` together to count occurrences:

```bash
grep apple fruits.txt | wc -l
```

Use pipelines to combine multiple filters:

```bash
grep quick sentences.txt | wc -w
```

### Task 5: Sort and remove duplicates

`sort` orders lines alphabetically (or numerically with options):

```bash
sort fruits.txt
```

`uniq` filters out duplicate lines, but only if duplicates are adjacent. That's why `sort` is usually combined with it:

```bash
sort fruits.txt | uniq
```

Count frequency of each unique line using `uniq -c`:

```bash
sort fruits.txt | uniq -c
```

### Task 6: Complex pipeline

Here we use `tr` (translate) to convert spaces into newlines, so every word appears on its own line. This allows us to process words rather than lines:

Count how many unique words are in `sentences.txt`:

```bash
cat sentences.txt | tr ' ' '\n' | sort | uniq | wc -l
```

Count how often each word appears:

```bash
cat sentences.txt | tr ' ' '\n' | sort | uniq -c | sort -nr
```

`tr` works character-by-character, not word-by-word. In this example, `tr ' ' '\n'` replaces each space character with a newline, effectively splitting the text into words.

## Questions

1. What does the `|` operator do?
2. What is the difference between `>` and `|`?
3. What does `grep` do?
4. What does `uniq -c` do?
5. Why do we often combine `sort` and `uniq`?
6. In Task 6: why did we use `tr ' ' '\n'`?
7. What is the role of `wc` in a pipeline?

## Advice

You are now entering the world where the shell becomes truly powerful. Instead of writing complex programs, you can often solve problems by cleverly chaining existing tools. Pipelines allow you to think in terms of small, testable transformations, each feeding into the next. As you get more comfortable, you will find that many data-processing tasks can be done entirely in the shell. When you are ready, you might want to revisit this exercise after studying regular expressions, which will make tools like `grep` even more powerful. You may also consult the exercise \[e46ffb8b-00d6-44a2-ad40-552ea03b4e3a] for a refresher on redirection and files.
