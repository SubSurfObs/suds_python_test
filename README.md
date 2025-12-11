# suds_python_test


PC-SUDS Tools (Work in Progress)

Utilities for reading, parsing, and converting PC-SUDS waveform files produced by SRC / Seismology Research Centre digitizers (Gecko, EchoPro, etc).
PC-SUDS is a loosely specified, binary format containing:
	•	STATIONCOMP (ID 5) — station/channel metadata
	•	DESCRIPTRACE (ID 7) — waveform blocks
	•	COMMENT (ID 20) — recorder/sensor/system metadata
	•	FEATURE (ID 10) — analyst picks (rare and poorly documented)
	•	Optional additional SRC-specific blocks

Real-world archives often contain mixed telemetry and locally stored files, duplicated minutes, partial channel sets, and inconsistent station naming. Our tools aim to make practical sense of these files at scale.

⸻

Goals
	1.	Extract reliable station metadata from PC-SUDS (network, station, location, lat/lon/elev, channels, units, polarity, etc).
	2.	Extract recorder-level information (sensitivity, gain, sensor info) where available.
	3.	Identify and decode manual picks (FEATURE blocks) for P, S, and other phase types.
	4.	Prepare for bulk conversion of large archives (20 TB+) into modern formats (MiniSEED + SDS).
	5.	Ensure repeatable, deterministic handling of mixed telemetry/local SUDS files.

⸻

What Works Today

✅ Metadata Extraction
	•	Full parsing of STATIONCOMP (5) and DESCRIPTRACE (7) blocks
	•	Extraction of channel info:
	•	channel code
	•	component & polarity
	•	units
	•	sensor type
	•	atod gain
	•	Extraction of station coordinates and network/station/location codes
	•	Handling of multi-station files (each station gets its own metadata block)
	•	Comment parsing (ID 20)

✅ Recorder Metadata (First Pass)
	•	Basic recorder info extracted via COMMENT structs
	•	Recognised issue: COMMENT blocks may be global, not per-station
(active area of investigation)

✅ Identification of Picks (FEATURE ID 10)
	•	Working method for locating FEATURE blocks
	•	Successfully extracted correct P-wave arrival times in single-station tests

⸻

Known Issues / Current Limitations
	1.	FEATURE parsing (picks) not fully generalised
Works for simple cases; multi-station/multi-pick files unresolved.
	2.	Recorder metadata not yet correctly attributed per station
Multiple stations in one SUDS file may each have separate recorder info,
but COMMENT blocks are not yet mapped reliably.
	3.	PC-SUDS format inconsistencies
Real-world files differ from published specs; some structures vary in size or content.
	4.	eqconvert directory mode is unsafe
When telemetry + local files coexist, it produces incomplete MSEED.
Solution: choose one file per minute before conversion.

⸻

Roadmap / To-Do

🔸 Waveform Conversion
	•	Implement “minute selector” choosing:
	1.	local files (with spaces)
	2.	fallback to telemetry (with underscores)
	•	Run eqconvert only on selected files
	•	Use scart to build proper SDS archives

🔸 Metadata Improvements
	•	Correctly bind COMMENT blocks to the station they belong to
	•	Parse sensitivity components separately (recorder × sensor × gain)

🔸 Picks
	•	Full FEATURE decoder
	•	Multi-station FEATURE handling
	•	Output picks into stationXML or QuakeML-like structure

🔸 Performance & Scale
	•	Directory walker for 20 TB archive
	•	Parallel processing
	•	Error logging and skipping corrupted files



## Notes from PC-SUDS documentation


(https://banfill.net/suds/PC-SUDS.pdf)

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


