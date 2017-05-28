# CRDFormatter
CRDFormatter is a script for formatting EQUIP files into human-readable formats created by [Kyle DeBry](https://github.com/kyledebry).

---

## Contents
- [Download CRDFormatter](#download)
- [Instructions](#instructions)
    - [Demo files](#i_demo)
    - [Description of EQUIP files](#i_desc)
    - [Using the program](#i_using)
    - [Full Events file](#i_full)
    - [Simplified file](#i_simplified)
    - [Common errors](#i_errors)
        - [FileNotFoundException](#i_e_file)
        - [Other errors](#i_e_other)
- [Contact Me](#contact)
- [Copyright Stuff](#copyright)

---

## <a name="download"></a>Download CRDFormatter
   [CRDFormatter](https://github.com/kyledebry/CRDFormatter/blob/master/CRDFormatter.jar?raw=true)

---

## <a name="instructions"></a>Instructions
### <a name="i_demo"></a>Demo files
In the instructions below, a specific EQUIP file is used as a demonstration. If you would like, you can download the file here to follow along:
- [`EQUIP_17SEP2015_082846.txt`](https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/EQUIP_17SEP2015_082846.txt)


Here are the files which result from running CRDFormatter on the demo file:
- [`EQUIP_17SEP2015_082846_FULL_EVENTS.csv`](https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/EQUIP_17SEP2015_082846_FULL_EVENTS.csv)
- [`EQUIP_17SEP2015_082846_SIMPLIFIED.csv`](https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/EQUIP_17SEP2015_082846_SIMPLIFIED.csv)

### <a name="i_desc"></a>Description of EQUIP files
CRDFormatter is a little program written by me to allow data from QuarkNet cosmic ray detectors, output by EQUIP, to be used by humans and analyzed however you want, with whatever tools you want.

The [data output](#i_demo) from EQUIP looks like this:
   ```
Cols: 0        1  2  3  4  5  6  7  8  9        A          B      C D  E F
      C469D655 B4 00 37 00 00 00 37 00 C45DD4A4 123002.019 170915 A 11 0 +0069
      C469D655 00 00 00 00 39 00 00 00 C45DD4A4 123002.019 170915 A 11 0 +0069
      C469D656 00 00 00 00 00 00 00 2D C45DD4A4 123002.019 170915 A 11 0 +0069
      C469D656 00 00 00 33 00 31 00 00 C45DD4A4 123002.019 170915 A 11 0 +0069
      C469D656 00 38 00 00 00 00 00 00 C45DD4A4 123002.019 170915 A 11 0 +0069
      C469D6CC A1 00 24 00 25 00 23 00 C45DD4A4 123002.019 170915 A 11 0 +0069
   ```
and is largely written in hexadecimal (base 16), making it difficult for humans to work with.

The most important columns here are:
- Columns 1 through 8, which, when nonzero, represent that one of the detectors has had the beginning of a pulse (cols 1, 3, 5, 7) or the end of a pulse (cols 2, 4, 6, 8). In the snippet above, in the first row detectors 0, 1, and 3 have started a pulse, and in the second row detector 2 has started a pulse, and then over the next three rows each of them deactivate. The last row is the start of a new detection event.
- Column A, which is the time of day the event occurred, taken from GPS. In this example, all of the events occurred near 12:30pm (and 2.019 seconds).
- Column B, which gives the date (DDMMYY). In this case, it is 17 Sept 2015.
- Columns C, D, and E give error information (in this case everything is fine).

However, this is difficult to read, and almost impossible to analyze in its current form. That's where CRDFormatter comes in.

### <a name="i_using"></a>Using the program:
The program is very simple to use, and there is no user input required other than choosing the correct EQUIP file.

1. Download [CRDFormatter.jar](https://github.com/kyledebry/CRDFormatter/blob/master/CRDFormatter.jar?raw=true) and double-click to run the program.
2. A dialog opens asking you to choose an EQUIP text file to format. Navigate to the location of the desired file on your system.
3. Select the EQUIP file, and click `Format EQUIP File`.

![CRDFormatter file selection dialog][file-select]

That's it! Now, navigate through your file system to the location of the EQUIP file on your computer. In the same folder, you should now find two `.csv` files with the same name as the EQUIP file, but the file name of one will end with `_FULL_EVENTS.csv` and the other with `_SIMPLIFIED.csv`.

### <a name="i_full"></a>Full Events file:
The `_FULL_EVENTS.csv` file has the least amount of processing done on the data. It simply reports the most important information in human-readable format. Here is an example, from the same [sample file](#i_demo) as the snippet above:

![Full Events file][full-events]

The first three rows contain some information about CRDFormatter and when this file was generated.

On row 5, the data begin. The formatted version of the file contains:
- the date that the even was measured (for data collection runs that may last for days or weeks)
- the time the event occurred past midnight in seconds
- the time the event occurred, in milliseconds, after midnight UTC on January 1, 1970 (also known as [Unix time or epoch time](https://en.wikipedia.org/wiki/Unix_time))
- the number of the detector which went off first (the trigger number)
- a flag for GPS signal (1 = valid, 0 = invalid)
- a flag for the trigger rate (1 = valid, 0 = invalid, often due to saturation of the detector or board)

These data can then be analyzed in any program. `CSV ` files can be opened in any spreadsheet program, such as Microsoft Excel or Google Sheets.

### <a name="i_simplified"></a>Simplified file:
The `_SIMPLIFIED.csv` file does not contain each event separately, but rather generates two histograms from the data. The first has a row for each second of the run, and the number of events that occurred during that second, while the second has a row with how many events were recorded in each minute of the run.

These two data sets can easily be plotted to very quickly gain a sense of the nature of the run, and are also often used in further analyses. The generation of these two lists is a result of the difficulty of creating histograms in programs such as Excel, which has no native histogram functionality.

Below is the `_SIMPLIFIED.csv` file from the [sample dataset](#i_demo):

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

### <a name="i_errors"></a>Common errors:
#### <a name="i_e_file"></a>FileNotFoundException

![Error: FileNotFoundException][error-file]

This error results from the CRDFormatter program trying to write to a file that is currently opened. For example, if you have previously formatted a file called `EQUIP_X.txt` and then opened the resulting `EQUIP_X_Simplified.csv` file in Excel, trying to format another file of the same name (`EQUIP_X.txt`) will result in this error. To successfully format the file, first close the `EQUIP_X_Simplified.csv` window of the program it is opened in (or possibly `Save As` another filename first) and run CRDFormatter again.

#### <a name="i_e_other"></a>Other errors
If you experience any other errors (either an error upon running the program, or an incorrectly formatted file), [email me](mailto:debry.1@osu.edu) with:
- the file you were trying to format
- a screenshot of the error message or the resulting incorrect file
- a specific description of the error

I'll get back to you as soon as I can.

---

## <a name="contact"></a>Contact Me
Email: [debry.1@osu.edu](mailto:debry.1@osu.edu)

---

## <a name="copyright"></a>Copyright Stuff
You are free to distribute the CRDFormatter.jar program to anyone and use it for any reason. Giving credit would be nice. Specifically:

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/InteractiveResource" property="dct:title" rel="dct:type">CRDFormatter</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/kyledebry" property="cc:attributionName" rel="cc:attributionURL">Kyle DeBry</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

I'd prefer for you not to try to decompile the bytecode and muck around with it, not because I'll hunt you down, but because it's kind of a mess. If you have questions about the program or want to make modifications, please [email me](mailto:debry.1@osu.edu).

---

<center>This page &copy; 2017 Kyle DeBry.</center>

---

[file-select]: https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/Select%20File.PNG "CRDFormatter file selection dialog"
[full-events]: https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/Full%20Events.PNG "Full events file"
[simplified]: https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/Simplified%20Charts.PNG "Simplified file"
[error-file]: https://raw.githubusercontent.com/kyledebry/CRDFormatter/master/Error_FileNotFound.PNG "Error: FileNotFoundException"
