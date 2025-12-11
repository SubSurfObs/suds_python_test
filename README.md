# suds_python_test


PC-SUDS Tools (Work in Progress)

Utilities for reading, parsing, and converting PC-SUDS waveform files produced by SRC / Seismology Research Centre digitizers (EchoPro, Waves, etc).
PC-SUDS is a loosely specified, binary format containing:

* STATIONCOMP (ID 5) — station/channel metadata
* DESCRIPTRACE (ID 7) — waveform blocks
* COMMENT (ID 20) — recorder/sensor/system metadata
* FEATURE (ID 10) — analyst picks (rare and poorly documented)



⸻

Goals

* Extract waveforms and channel info and convert to Obspy Streams
* Extract reliable basic station metadata from PC-SUDS (network, station, location, lat/lon/elev, channels, units, polarity, etc).
* Extract recorder-level information (sensitivity, gain, sensor info) where available.
* Identify and decode manual picks (FEATURE blocks) for P, S, and other phase types.

⸻

What Works Today

✅ Metadata Extraction

* Full parsing of STATIONCOMP (5) and DESCRIPTRACE (7) blocks
* Extraction of basic channel info incl: channel code, units, sensor type, atod gain, station coordinates and network/station/location codes
* basic station information is available for multi-station files (each station gets its own metadata block)


✅ Recorder Metadata (First Pass)

* Station recorder and sensor info extracted via COMMENT structs
* Recognised issue: COMMENT blocks may be global, not per-station) (active area of investigation)

⸻

Known Issues / Current Limitations

* FEATURE parsing (picks) not fully generalised, Works for simple cases; multi-station/multi-pick files unresolved.

* Recorder metadata not yet correctly attributed per station, Multiple stations in one SUDS file may each have separate recorder info,
but COMMENT blocks are not yet mapped reliably.


⸻

Roadmap / To-Do

🔸 Waveform Conversion


🔸 Metadata Improvements
* Correctly bind COMMENT blocks to the station they belong to
* Parse sensitivity components separately (recorder × sensor × gain)

🔸 Picks /locations
* Full FEATURE decoder
* Multi-station FEATURE handling
* Output picks into stationXML or QuakeML-like structure



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


