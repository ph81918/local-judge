# Local Judge

Given source code, Makefile (or build commands), input files, and answer files then judge the program locally.

```
source code ━━━━━━┓
                  ┃ [build]
                  ▼
[run] input ━▶ program ━▶ output
                            ┃
                            ▼
answer ━━━━━━━━━━━━━━━▶ [compare] ━▶ correctness, diff result
```

## Features

+ Automatically build the source code into executable
+ Automatically run the executable for each input and compare output with answer
+ Customization friendly
+ Without any dependencies but standard build-in python packages

## Environment (Recommended)

+ Ubuntu 18.04
+ python 3.6

## Usage

### Configuration

+ `judge.conf`: be placed in the root of your program
+ Content:
    + `BuildCommand`: how to build the executable
    + `Executable`: the name of the executable
    + `Inputs`: input files (can use wildcard)
    + `TempOutputDir`: the temporary directory to place output files
    + `DiffCommand`: how to find differences between output and answer
    + `DeleteTempOutput`: whether to delete the temporary output after finding the differences (true or false)
    + `AnswerDir`: the directory where contains the answer files corresponding to the input files
    + `AnswerExtension`: the extension of the answer files
+ Example config file:
    ```conf
    [Config]
    BuildCommand = make clean && make
    Executable = scanner
    Inputs = input/*.txt
    TempOutputDir = /tmp/output
    DiffCommand = git diff --no-index --color-words
    DeleteTempOutput = true
    AnswerDir = answer
    AnswerExtension = .out
    ```

### Commands

```bash
usage: judge.py [-h] [-c CONFIG] [-v VERBOSE]

optional arguments:
  -h, --help            show this help message and exit

  -c CONFIG, --config CONFIG
                        the config file, default: `judge.conf`

  -v VERBOSE, --verbose VERBOSE
                        the verbose level, default: `0`
                        `0`: suppress the diff results
                        `1`: show the diff results
```

## Examples

```bash
$ cd examples/wrong/
$ python3 ../../judge/judge.py 
=======+========================================================================
Sample | Accept
=======+========================================================================
  xxxx | ✘
=======+========================================================================
    gg | ✔
=======+========================================================================
     a | ✔
=======+========================================================================
     b | ✔
=======+========================================================================
Total score: 75

[INFO] set `-v 1` to get diff result.
For example: `python3 judge/judge.py -v 1`



$ python3 ../../judge/judge.py -v 1
=======+========================================================================
Sample | Accept
=======+========================================================================
  xxxx | ✘
-------+------------------------------------------------------------------------
diff --git a/tmp/output/xxxx_1579351349.out b/../answer/xxxx.out
index 4f6ff86..3a2e3f4 100644
--- a/tmp/output/xxxx_1579351349.out
+++ b/../answer/xxxx.out
@@ -1 +1 @@
4294967295-1

=======+========================================================================
    gg | ✔
=======+========================================================================
     a | ✔
=======+========================================================================
     b | ✔
=======+========================================================================
Total score: 75
```