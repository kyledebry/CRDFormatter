# CRDFormatter
CRDFormatter is a script for formatting EQUIP files into human-readable formats created by Kyle DeBry.

## Contact me
[debry.1@osu.edu](mailto:debry.1@osu.edu)

---

## Download CRDFormatter
   [CRDFormatter](https://github.com/kyledebry/CRDFormatter/blob/master/CRDFormatter.jar?raw=true)

---

## Instructions
### Description of EQUIP Files
CRDFormatter is a little program written by me to allow data from QuarkNet cosmic ray detectors, output by EQUIP, to be used by humans and analyzed however you want, with whatever tools you want.

The data output from EQUIP looks like this:
   ```
Cols: 0        1  2  3  4  5  6  7  8  9        A          B      C D  E F
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
The program is very simple to use, and there is no user input required other than choosing the correct EQUIP file.

1. Download [CRDFormatter.jar](https://github.com/kyledebry/CRDFormatter/blob/master/CRDFormatter.jar?raw=true) and double-click to run the program.
2. A dialog opens asking you to choose an EQUIP text file to format. Navigate to the location of the desired file on your system.
3. Select the EQUIP file, and click `Format EQUIP File`.

![CRDFormatter file selection dialog][file-select]

That's it! Now, navigate through your file system to the location of the EQUIP file on your computer. In the same folder, you should now find two `.csv` files with the same name as the EQUIP file, but the file name of one will end with `_FULL_EVENTS.csv` and the other with `_SIMPLIFIED.csv`.

### Full Events file:
The `_FULL_EVENTS.csv` file has the least amount of processing done on the data. It simply reports the most important information in human-readable format. Here is an example, from the same sample file as the snippet above:

![Full Events file][full-events]

The first three rows contain some information about CRDFormatter and when this file was generated.

On row 5, the data begin. The formatted version of the file contains:
- the date that the even was measured (for data collection runs that may last for days or weeks)
- the time the event occurred past midnight in seconds
- the time the event occurred, in milliseconds, after midnight UTC on January 1, 1970 (also known as [Unix time or epoch time](https://en.wikipedia.org/wiki/Unix_time)
- the number of the detector which went off first (the trigger number)
- a flag for GPS signal (1 = valid, 0 = invalid)
- a flag for the trigger rate (1 = valid, 0 = invalid, often due to saturation of the detector or board)

These data can then be analyzed in any program. `CSV ` files can be opened in any spreadsheet program, such as Microsoft Excel or Google Sheets.

### Simplified file:
The `_SIMPLIFIED.csv` file does not contain each event separately, but rather generates two histograms from the data. The first has a row for each second of the run, and the number of events that occurred during that second, while the second has a row with how many events were recorded in each minute of the run.

These two data sets can easily be plotted to very quickly gain a sense of the nature of the run, and are also often used in further analyses. The generation of these two lists is a result of the difficulty of creating historgrams in programs such as Excel, which has no native histogram functionality.

Below is the `_SIMPLIFIED.csv` file from the sample dataset:

![Simplified file][simplified]

As in the full events file, the first three rows are information about CRDFormatter and when the file was generated. Rows 5-8 contain information about when the data was collected, including the [epoch time](https://en.wikipedia.org/wiki/Unix_time) of the start of the run.

The first dataset, in columns A, B, and C, is a binning of the events into 1-second bins. These data include:
- the [epoch](https://en.wikipedia.org/wiki/Unix_time) time of the end of the bin
- the number of seconds between the beginning of the run and the start of bin
- the number of events in that bin

The second dataset, in columns E, F, and G, bins the data into 1 minute long bins. Similar to the first dataset, it contains:
- the [epoch](https://en.wikipedia.org/wiki/Unix_time) time of beginning of the end of the bin
- the number of minutes between the beginning of the run and the start of bin
- the number of events in that bin

Also shown in the image above are two graphs which show how easy it is to gain a sense of the data, and any changes that occurred during the run. The top graph is a plot of the minute-long binning of the data, while the lower graph contains a scatterplot of the second-long binning of the data, as well as a 60s moving average, which resembles the shape of the first graph.

Depending on the length of the run and the rate at which events occurred, either the second or the minute graphs may be more useful. If another binning is required, the full event dataset can be used in programs like Excel to generate different histograms or analyses with a bit of extra work.

[file-select]: https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/Select%20File.PNG "CRDFormatter file selection dialog"
[full-events]: https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/Full%20Events.PNG "Full events file"
[simplified]: https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/Simplified%20Charts.PNG "Simplified file"
