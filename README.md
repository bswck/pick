# pick
A simple line picker CLI tool written in Rust. 🦀

> [!Warning]
> 🚧 This project is currently in the **Work in Progress** state. 🚧
>
> Hit the `👁 Watch` button to know when it's ready to be tried out!

# Tutorial
Let's consider a simple file `foobar.txt` (lines on the left are not part of the file):

https://github.com/bswck/pick/blob/a2269a78f299fe5cbc4e743422cfcd03cbc5f0f6/assets/foobar.txt#L1-L5

`pick` enables you to manipulate the stream of data and pick lines and characters of interest.
Lines and characters are counted from `1`. An alternative implementation that counts from `0` will be available as `pick0`.

In the further part of thid document, you will see sections like this:
> [!Note]
> **Meaning**: _Explanation..._

They are there to help you understand what the prompts above these sections are supposed to do.

## Line Selection
### Single Line Selection
[_(Jump to foobar.txt)_](#tutorial)

`💲 cat foobar.txt | pick 1`
> [!Note]
> **Meaning**: _Pick the first line from the `cat foobar.txt` output._

Output:
```
123456789
```

### Multiple Line Selection
[_(Jump to foobar.txt)_](#tutorial)

Lines picked with `pick` are separated by the newline character (`\n`) by default:

```shell
💲 cat foobar.txt | pick 1,2
```
> [!Note]
> **Meaning**: _Pick lines 1 and 2 from the `cat foobar.txt` output and connect them with a newline character._

Output:
```
123456789
foo
```
You can specify a separator for the selected lines:

```shell
💲 cat foobar.txt | pick 1,2 --sep=" "
```
> [!Note]
> **Meaning**: _Pick lines 1 and 2 from the `cat foobar.txt` output and connect them with a space character ` `_.

Output:
```
123456789 foo
```
This saves you from using `xargs` on the `pick` command output.

### Slicing
[_(Jump to foobar.txt)_](#tutorial)

Slicing syntax in `pick` is similar to the Rust range syntax.
It is left-inclusive and right-exclusive by default.
Slices can generally be expressed in the following pseudo-pattern:

```
START..[=]STOPsSTEP
```

#### Slice Creation Examples
- `1..2`
  <br />Every line from 1 to 2 right-exclusive.
  <br />**Selects only line 1**.
- `1..5s2`
  <br />Every second line from 1 to 5 right-exclusive (starts from the first specified line, `1`, not `3`).
  <br />**Selects only lines 1, 3**.
- `1..=5s2`
  <br />Every second line from 1 to 5 right-inclusive (starts from the first specified line, `1`, not `3`).
  <br />**Selects only lines 1, 3, 5**.
- `1..-1`
  <br />Every line from the first character to the last one right-exclusive.
  <br />**Selects only lines 1, 2, 3, 4**.
- `1..=-1`
  <br />Every line from the first character to the last one right-inclusive.
  <br />**Selects lines 1, 2, 3, 4, 5**.
- `1..-1s3`
  <br />Every third line from the first character to the last one right-exclusive (starts from the first specified line, `1`, not `4`).
  <br />**Selects only lines 1, 4**.

Due to the nature of shells, you can use variables when creating your slices:
```shell
START_IDX=1
END_IDX=5

cat foobar.txt | pick $START_IDX..-1
```
> [!Note]
> **Meaning**:
> - _Globally set `START_IDX` variable to `1` and `END_IDX` to `5`._
> - _Pick lines from 1 to 5 (`-1` in this context means the last line of the output) right-exclusive from the `cat foobar.txt` output._

#### Slice Usage Examples
[_(Jump to foobar.txt)_](#tutorial)

```shell
💲 cat foobar.txt | pick 1..5
```
> [!Note]
> **Meaning**: _Pick lines from 1 to 5 right-exclusive from the `cat foobar.txt` output and connect them with a space character `\n`._

Output:
```
123456789
foo
bar
biz
```

You can make your slice right-inclusive with the `=` character before the stop:

```shell
💲 cat foobar.txt | pick 1..=5
```
> [!Note]
> **Meaning**: _Pick lines from 1 to 5 right-inclusive from the `cat foobar.txt` output and connect them with a space character `\n`._

Output:
```
123456789
foo
bar
biz
THE END.
```

## Character Selection
[_(Jump to foobar.txt)_](#tutorial)

More to come here later.

