cmdcheck
========

Simple test framework to record and check command behaviors

Basic usage
-----------

To record a command:

```
printf 'foo\nbar\nbaz\n' | cmdrecord grep-b.t -- grep b
```

This command is going to run `grep b` and record its behavior. The result is a
directory called `grep-b.t` which contains a file for standard input, output.
In addition a file called `TESTRECIPE` contains the details of the test case.

To check a test case:

```
cmdcheck grep-b.t
```

Testing cmdrecord itself
------------------------

One can record the behavior of the first example using the following command

```
printf 'foo\nbar\nbaz\n' | cmdrecord cmdrecord-grep-b.t -- ../cmdrecord grep-b.t -- grep b
```

However the default behavior records the value of all environment variables which on top information leakage is a just a lot of noise.
So instead one uses the `--env` option to first reset the environment variables using `empty` and then we specify `PATH` to `/usr/bin:/bin`.
Finally the tested `cmdrecord` is given `--env empty` to ease the reproducibility.

```
printf 'foo\nbar\nbaz\n' | cmdrecord cmdrecord-grep-b.t --env empty,PATH=/usr/bin:/bin -- ../cmdrecord grep-b.t --env empty -- grep b
```
