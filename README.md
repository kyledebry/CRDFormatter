# CRDFormatter
CRDFormatter is a script for formatting EQUIP files into human-readable formats created by Kyle DeBry.

## Contact me:
[debry.1@osu.edu](mailto:debry.1@osu.edu)

---

## Download CRDFormatter:
   [CRDFormatter](https://github.com/kyledebry/CRDFormatter/blob/master/CRDFormatter.jar?raw=true)

---

## Description
CRDFormatter is a little program written by me to allow data from QuarkNet cosmic ray detectors, output by EQUIP, to be used by humans and analyzed however you want, with whatever tools you want.

The data output from EQUIP looks like this:
   ```
*Col: 0      1  2  3  4  5  6  7  8  9        A          B      C D  E F*
   C469D655 B4 00 37 00 00 00 37 00 C45DD4A4 123002.019 170915 A 11 0 +0069
   C469D655 00 00 00 00 39 00 00 00 C45DD4A4 123002.019 170915 A 11 0 +0069
   C469D656 00 00 00 00 00 00 00 2D C45DD4A4 123002.019 170915 A 11 0 +0069
   C469D656 00 00 00 33 00 31 00 00 C45DD4A4 123002.019 170915 A 11 0 +0069
   C469D656 00 38 00 00 00 00 00 00 C45DD4A4 123002.019 170915 A 11 0 +0069
   C469D6CC A1 00 24 00 25 00 23 00 C45DD4A4 123002.019 170915 A 11 0 +0069
   ```
and is largely written in hexadeximal (base 16), making it difficult for humans to work with.

The most important columns here are:
- Columns 1 through 8, which, when nonzero, represent that one of the detectors has had the beginning of a pulse (cols 1, 3, 5, 7) or the end of a pulse (cols 2, 4, 6, 8). In the snippet above, in the first row detectors 0, 1, and 3 have started a pulse, and in the second row detector 2 has started a pulse, and then over the next three rows each of them deactivate. The last row is the start of a new detection event.
- Column A, which is the time of day the event occurred, taken from GPS. In this example, all of the events occurred near 12:30pm (and 2.019 seconds).
- Column B, which goves the date (DDMMYY). In this case, it is 17 Sept 2015.
- Columns C, D, and E give error information (in this case everything is fine).
However, this is difficult to read, and almost impossible to analyze in its current form. That's where CRDFormatter comes in.
### Using the program:
