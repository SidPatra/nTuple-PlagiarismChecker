# nTuple-PlagiarismChecker
Command-line program indicating the percent of plagiarism detected when comparing two files.
Title: Plagiarism Checker
Submitter: Siddharth Patra

Project description:
Build a plagiarism checker using n-tuples to compare two different file's contents. Accounts for uses of synonyms.

Input:
1.	file of a list of synonyms
2.	input file 1
3.	input file 2
4.	(optional) the number N, the tuple size.  If not supplied, the default should be N=3.

Output:
some percentage (e.g. 100%)


Example:
Input (all text files):
syns.txt: run sprint jog
file1.txt: go for a run
file2.txt: go for a jog
Output:
100% <- in console/command-line?

Assumptions made in this program:
1. All inputs possible: syns.txt, file1.txt, file2.txt (all composed of space separated words of type string,
    required inputs), and 'n' (optional input given in command line, has to be a positive integer greater than 0)
2. All outputs possible:
	Best case: single percentage (rounded to nearest integer) indicating how similar file1.txt and file2.txt are (e.g: 100%)
	Worse case: TupleSize is reset to <min(len(file1),len(file2)), or 3> as default/inputted 'n' is too large for
	    one of the given files/can't be less than 0 percentage value
	Worse case: Program failed as at least one of the given files contains no information
	Worse case: syns.txt/file1.txt/file2.txt not found
		    Program failed as required files do not exist
	Worst case: syns.txt not found
		    file1.txt not found
		    file2.txt not found
		    Program failed as required files do not exist
	All outputs printed in command line/console
3. file1.txt and file2.txt both have the same number of words, but the program has special cases where it will check if
    they are of different lengths, and make appropriate adjustments.
4. All given files must have contain at least one word. If a file is found to create nothing, then program will fail.
5. The value of 'n' exists in the following range: 0 < n <= number of words in file1.txt == number of
    words in file2.txt. If 'n' does not meet these requirements, it will be reset to 3.
6. If the number of words in at least one of those files is less than the default 'n' of 3, then 'n' will
    be set to the number of words in the file with fewer words (if user inputs a number bigger than that). If user inputs a smaller integer number, that will be permitted.
7. Similarity check: all tuples that the input files are composed of are the exact same order - tuples1 and tuples2 will
    not get checked for rearrangements of letters or tuples unless each file is composed of a different number of n-tuples.
8. Percentage similarity finding: both file1.txt and file2.txt are assumed to contain the same number of space-separated
    words, and therefore the same number of 'n'd-strings, so percentage is found by dividing number of similarities
    by number of elements in tuples1 (the tuple of strings for file1.txt). If they do not have the same length, than it
    will be calculated using the length of whichever is smaller in length: tuples1 or tuples2.

How it works:
1. File inputs and optional input 'n' is taken (file inputs assumed to be in directory of .py file).
    'n' is taken in command-line or console.
2. If the files don't exist/are empty, program will fail (see Assumptions)
3. If the value 'n' that is inputted is less than 1, then it is reset to either 3 or the length of the smallest file
    (in terms of the number of words it contains). It'll take the smallest of these 3 possible values.
4. Phase 1: synonymReplacement function called for both files. This function will identify a pivot, or first mention
    of a synonym, and replace every mention of any synonym with that synonym.
5. Phase 2: tupleize function called for both files. Makes n-tuple strings of file1 and file2 and appends them to tuple1
    and tuple2 respectively.
6. Phase 3: Similarity checking. Checks how many n-tuple strings in both tuple1 and tuple2 are identical.
    Every identical n-tuple found is added to a counter.
7. Phase 4: Percentage finding and output. Percentage is found by dividing counter with length of tuple1. If lengths of
    tuple1 and tuple2 are different (due to different length of file1 and file2), then the smaller of the two's length
    is taken as denominator to give the maximum plagiarized percentage.

Other notes:
a) Instead of replacing all mentions of a synonym, it is possible to go through each string in tuple1 and tuple2 to check
    if any string contained a synonym, but I wished to show better understanding and not be dependent on in-built functions.

b) I made several alternative instructions in case the number of words in file1.txt and file2.txt were different. To
    speed up overall worst case run-time, these precautions can be removed, but will open the program up to encountering
    errors.

c) I also made use of try-except and other conditional statements to make sure that the inputs were as per my assumptions.
    Removing these would not effect the run-time significantly, but will also open the program up to encountering errors.

d) There are a few steps within this program that could have been replaced with numpy functions. I chose to not use these
    to keep the program simple and 'import-free'. For example, similarities(t1,t2) does not need to exist, as there is a
    numpy function that can perform its task.

e) Comparing more than two files: Because of how the code is set up, if all files to be checked are of same size and
    appropriate modifications are made (additional tuples (tuple3, tuple4, etc.), then time complexity would stay
    approximately the same (additional equality-checks needed for each additional tuple in similarities(t1,t2,...).
    If number of words in each file is different, additional for-loops will be required, multiplying O-time by the size
    of each new file being handled.

f) All the inputs being taken are .txt files. To check websites, for example, .html files need to be
    taken as inputs or at least parsed through and converted into .txt files to only account for text (without any
    website formatting). The same can be said about pdfs and other file types. Directly taking a non-.txt file into this
    program would not be wise, as it is not built to handle non-plain-text documents.

g) Code needs to be edited if files are not called syns.txt, file1.txt, or file2.txt

Overall runtime (for files of equal number of words, n = 3 != f1 != f2 (aka ideal case): O(f+f+f+fn) -> O(fn)
                (for files of unequal number of words...): O(f1 * f2) <- this case is not expected to happen, can still be handled

Space usage: taking s for syns.txt, f1 for file1.txt, f2 for file2.txt:
    total space usage is: s + f1 + f2 + (f1-(n-1)) + (f2-(n-1))
                                            ^ tuple1   ^ tuple2
