# sass2scss

[![Build Status](https://travis-ci.org/mgreter/sass2scss.svg?branch=master)](https://travis-ci.org/mgreter/sass2scss)

C++ tool/library to convert indented sass syntax to the newer scss syntax. This
was initially a port of a previous perl implementation. Its primary intention
was to bring indented sass syntax support to [`libsass`](https://github.com/sass/libsass).
`sass2scss` is included in `libsass` since [version 2.0](https://github.com/sass/libsass/releases/tag/v2.0).

## Unit Tests

I added around 60 unit tests for `sass2scss` to the `libsass` perl binding
[`CSS-Sass`](https://github.com/sass/perl-libsass/blob/master/t/06_sass_to_scss.t).

## Command Line Utility

```
sass2scss [options] < file.sass
```

```
-p, --pretty       pretty print output
-c, --convert      convert src comments
-s, --strip        strip all comments
-k, --keep         keep all comments
-h, --help         help text
-v, --version      version information
```

The source Sass is read from stdin and the resulting SCSS is printed to stdout.

`--pretty` can be repeated up to 3 times to add even more linefeeds (lf).

- 0: Write everything on one line (`minimized`)
- 1: Add lf after opening bracket (`lisp style`)
- 2: Add lf after opening and before closing bracket (`1TBS style`)
- 3: Add lf before/after opening and before closing (`allman style`)

The `lisp style` is the only output style that should not alter the line count
of the input file. This is the best option if you still want to use `source-maps`,
since it should only change the source by a few inserted chars. So far
`sass2scss` does not produce source-maps and `libsass` will not be able to
produce 100% accurate `source-maps` for indented sass syntax input files!

## Compile and Install

If you want to use this lib from another C project, you can just add the source
files to your project. To compile and install the utility we have added a very
simple [Makefile][1]. On unix systems this normally involves the following steps:

```bash
git clone https://github.com/mgreter/sass2scss
# alternatively you may fetch and extract an archive from github
# wget https://github.com/mgreter/sass2scss/archive/master.tar.gz && tar master.tar.gz
cd sass2scss # adjust name if you downloaded an archive 
make # or i.e. `mingw32-make` or `dmake` on windows
./sass2scss --help # binary is built in same directory
sudo cp -a sass2scss /usr/local/bin # to install it system wide
```

[1]: https://www.gnu.org/software/make/manual/make.html


## Use examples

The original Sass file, called `styles.sass`:

```sass
#main
  // This is the best color since it has a specific meaning
  color: rebeccapurple
  font-family: "ヒラギノ角ゴ Pro W3", "Hiragino Kaku Gothic Pro", Osaka
  font-size: 75%
```

For example with default options, running

```sh
sass2scss < styles.sass > styles.scss
```

Would result the file `styles.scss` to look like:

```scss
#main { color: rebeccapurple;font-family: "ヒラギノ角ゴ Pro W3", "Hiragino Kaku Gothic Pro", Osaka;font-size: 75%; }
```

When adding options for pretty printing (`-p`) and keeping comments (`-k`):

```scss
#main {
  // This is the best color since it has a specific meaning
  color: rebeccapurple;
  font-family: "ヒラギノ角ゴ Pro W3", "Hiragino Kaku Gothic Pro", Osaka;
  font-size: 75%; }
```

With two pretty (`-p -p`) and comment conversion (`-c`) options, the output becomes:

```scss
#main {
  /* This is the best color since it has a specific meaning */
  color: rebeccapurple;
  font-family: "ヒラギノ角ゴ Pro W3", "Hiragino Kaku Gothic Pro", Osaka;
  font-size: 75%;
}
```

## License

Licensed under [the MIT License](./LICENSE).

Copyright (c) Marcel Greter (Original author)
Copyright (c) Rastislav @raisty (Reincarnation)
