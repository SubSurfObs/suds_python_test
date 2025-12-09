# suds_python_test


## Notes

PC-SUDS is based on self-identifying structures (which may or may not have
data following them) stored as a linked list. These structures are typically stored
in files on disk or tape. Data of any type may be stored in PC-SUDS files and
there are no limits on the length of these data. PC-SUDS was designed to be
easily extensible by allowing users to add new structures to meet their specific
needs without compromising the ability of others to access the portions of the
data that they recognize. Additionally, new fields may be added to the end of
existing structures without interfering with ability of older software the read the
structure. This allows data to be shared among users with a minimum of
problems associated with the data format.



Phase arrival data is stored in the SUDS data file using SUDS_FEATURE
structures. Location programs can read data directly from and store solutions
directly (using SUDS_ORIGIN structures) to the SUDS data files or these data
can be extracted, used and then appended to the SUDS data files.
The program is designed to run without user interaction although a screen
display mode is available to help you tune the picking algorithm. The algorithm
used is loosely based on Rex Allen's paper; Automatic Earthquake Recognition
and Timing from Single Traces4. A detailed description of the actual algorithm
is presented later in this section. 

## Resources

https://banfill.net/suds/PC-SUDS.pdf
