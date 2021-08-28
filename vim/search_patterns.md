# Search Patterns
from ![Vim WIKI](https://vim.fandom.com/wiki/Search_patterns)

When searching in Vim, you enter a search pattern. This tip provides a
tutorial introduction to using search patterns.

## Finding a whole word {#finding_a_whole_word}

In a program, you may want to search for an identifier named `i`.
However, entering the search `/i` will find every hit, including the
\"i\" in words like \"if\" and \"while\". In a pattern, `\<` represents
the beginning of a word, and `\>` represents the end of a word, so to
find only the whole word \"i\", use the pattern:

    \<i\>

In normal mode, press `/` to start a search, then type the pattern
(`\<i\>`), then press Enter.

If you have an example of the word you want to find on screen, you do
not need to enter a search pattern. Simply move the cursor anywhere
within the word, then press `*` to search for the next occurrence of
that whole word. Vim inserts `\<` and `\>` automatically (see
[searching](/VimTip1 "wikilink")).

The pattern `\<i` finds all words that start with \"i\", while `i\>`
finds all words that end with \"i\".

## Finding duplicate words {#finding_duplicate_words}

Sometimes words are accidentally duplicated in text (like this this).
The following pattern finds repeated words that are separated by
whitespace (spaces, tabs, or newlines):

    \(\<\w\+\>\)_s*\<\1\>

The pattern searches for `\<\w\+\>` (word beginning `\<`, word character
`\w`, one or more `\+` word characters, word end `\>`). That is, it
searches for a whole word. It then looks for any amount of whitespace
(`_s*`); `\s` matches space or tab, while `_s` matches space or tab or
newline (end-of-line character). Finally, the pattern looks for `\1`
which is the whole word that was found in the escaped parentheses.

## Finding this *or* that {#finding_this_or_that}

A search pattern can use `\|` to search for something *or* something
else. For example, to search for all occurrences of \"red\" or \"green\"
or \"blue\", enter the following search pattern (in normal mode, press
`/` then type the pattern, then press Enter):

    red\|green\|blue

To replace all instances of \"red\" or \"green\" or \"blue\" with
\"purple\", enter:

    :%s/red\|green\|blue/purple/g

However, the above pattern finds \"red\" in \"bored\", so the substitute
would change \"bored\" to \"bopurple\". If that is not what you want,
use the following pattern to find only the whole words \"red\" or
\"green\" or \"blue\":

    \<\(red\|green\|blue\)\>

In a pattern, `\<` and `\>` respectively specify the beginning and end
of a word, while $</code> and <code>$ respectively specify the beginning
and end of a group (the pattern `\<red\|green\|blue\>`, without escaped
parentheses, would find \"red\" occurring at the beginning of a word, or
\"green\" occurring anywhere, or \"blue\" occurring at the end of a
word).

After searching with the command `/\<`$red\|green\|blue$`\>` you could
change the whole words \"red\" or \"green\" or \"blue\" to \"purple\" by
entering the following (the search pattern is empty in this command, so
it uses the last search):

    :%s//purple/g

In a substitute, you can use `&` in the replacement to mean the \"whole
matched pattern\" (everything that was found). For example, the
following will insert quotes around all occurrences of the whole words
\"red\" and \"green\" and \"blue\":

    :%s/\<\(red\|green\|blue\)\>/"&"/g

If you do not want the whole matched pattern, you can use parentheses to
group text in the search pattern, and use the replacement variable `\1`
to specify the first group. For example, the following finds
\"color *x*\" and replaces it with \"colored *x*\" where *x* is the
whole word \"red\" or \"green\" or \"blue\":

    :%s/color \<\(red\|green\|blue\)\>/colored \1/g

## Finding two words in either order {#finding_two_words_in_either_order}

You can search for a line that contains two words, in any order. For
example, the following pattern finds all lines that contain both \"red\"
and \"blue\", in any order:

    .*red\&.*blue

In a pattern, `\&` separates alternates, each of which has to match at
the same position. The two alternates in this example are:

-   `.*red` (will match all characters from the beginning of a line to
    the end of the last \"red\"); and
-   `.*blue` (will match all characters from the beginning of a line to
    the end of the last \"blue\").

A line which contains both \"red\" and \"blue\" will match both
alternates, starting at the beginning of the line. The pattern
`.*red\&.*blue` finds the *last* alternate (but only if all alternates
match at the same position), so if you are [highlighting
matches](/VimTip14 "wikilink"), you will see text matched by `.*blue`
highlighted.

An alternative procedure is to use a pattern that explicitly finds
\"red\" followed by \"blue\", or \"blue\" followed by \"red\":

    \(red.*blue\)\|\(blue.*red\)

To search for lines that contain only the whole words \"red\" and
\"blue\", in either order, use one of the following patterns:

    .*\<red\>\&.*\<blue\>
    \(\<red\>.*\<blue\>\)\|\(\<blue\>.*\<red\>\)

## Finding trailing zeroes {#finding_trailing_zeroes}

The following pattern finds redundant trailing zeroes in numbers:

    \(\.\d\+\)\@<=0\+\>

The pattern does not find the \"0\" in \"1.0\", but it finds the
trailing \"00\" in each of the following numbers: 1.000 1.000200
1.0002000300. After searching, the command `:%s///g` would delete all
the redundant zeroes (the search pattern is empty, so it uses the last
search).

The pattern:

-   Contains `\@<=` which checks if the preceding atom matches just
    before what follows.
-   First searches for `0\+\>` (what follows).
-   Then checks if what is just before matches $\.\d\+$ (preceding
    atom).

The first search is `0\+\>` which finds one or more (`\+`) of `0`
followed by end of word (`\>`).

The check is $\.\d\+$`\@<=0\+\>` which verifies that the text
immediately before the trailing zeroes consists of a decimal point
(`\.`), then one or more decimal digits (`\d\+`). The escaped
parentheses $...$ make an \"atom\" from what is enclosed by the
parentheses.

## References

-   ```{=mediawiki}
    {{help|pattern}}
    ```

-   ```{=mediawiki}
    {{help|/\&}}
    ```
