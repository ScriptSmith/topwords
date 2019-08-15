# Top english words

A comprehensive list of the top 3 million+ english words in project gutenberg. Data is sourced from [Allison Parrish's](https://github.com/aparrish) awesome [gutenberg-dammit](https://github.com/aparrish/gutenberg-dammit) project.

## Usage

Use the word list:
```
$ head words.txt
the
of
and
to
a
in
that
i
he
```

Use the word count list:
```
$ head counts.txt
169852828 the
92493412 of
83626800 and
69017783 to
54796935 a
47554786 in
30598554 that
30324861 i
27900933 he
```

## Download

- [Download words](https://raw.githubusercontent.com/ScriptSmith/topwords/master/words.txt)
- [Download word counts](https://raw.githubusercontent.com/ScriptSmith/topwords/master/counts.txt)

or

Clone this repo:
```
git clone https://github.com/scriptsmith/topwords.git
cd topwords
```

## Recreating

Tools used:

- jq
- parallel
- grep
- sed
- GNU coreutils
	- tr
	- sort
	- uniq
    - cut

The following pattern was used to find words in the corpus:
```regex
[A-Za-z]+('[A-Za-z]+)?(?<!('s))
```

### Clone this repo

```
git clone https://github.com/scriptsmith/topwords.git
cd topwords
```

### Get the data

Download and extract the [guttenberg-dammit](https://github.com/aparrish/gutenberg-dammit) data. This is a free resource, so don't abuse it.

### Extract the words

Finds words from the 40000+ books with English as a primary language:

```
jq -r '.[] | select((.Language | length) == 1 and .Language[0] == "English") | "gutenberg-dammit-files/" + ."gd-path"' gutenberg-dammit-files/gutenberg-metadata.json | parallel "grep -ohPf pattern.txt {}" | tr '[:upper:]' '[:lower:]' > allwords.txt
```

### Sort and count words

If your temporary directory can't store more than 60GiB, change the value of `TMP_DIR`

```
TMP_DIR=/tmp
sort -T $TMP_DIR allwords.txt | uniq -c | sed 's/^\s*//' | sort -nr > counts.txt
```

### Remove word counts

```
cut -d ' ' -f2 counts.txt > words.txt
```
