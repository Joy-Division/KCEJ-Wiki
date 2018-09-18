# SELF Archive

The SELF archive (MSELF) format is used to store the game's overlays, which take the form of the standard PlayStation 3 [Signed ELF](http://www.psdevwiki.com/ps3/SELF_File_Format_and_Decryption).

## Table of Contents

- [Basic Information](#basic-information)
- [File Structure](#file-structure)

## Basic Information

MGS4 posesses a single MSELF archive located under ``mgs/mgs4.mself``.

The archive contains duplicate copies of the following overlays:
```
mgs/stage/init/init.self
mgs/stage/mg_setup/mg_setup.self
mgs/stage/mg_setup1/mg_setup1.self
```

## File Structure

All values are stored in big-endian byte order, given that [Cell/BE](https://en.wikipedia.org/wiki/Cell_(microprocessor)) is a big endian architecture.

The MSELF header is made up of the following structures:

```c
typedef struct _MSF_HEADER {
	char	magic[4];	// 'MSF\0'
	uint32	unknown1;
	uint32	unknown2;
	uint32	totalsize;
	uint32	entrycount;
	uint32	headersize;
	char	padding[40];
} MSF_HEADER;
```
```c
typedef struct _MSF_ENTRY {
	char	filepath[32];
	uint32	padding1;
	uint32	offset;
	uint32	padding2;
	uint32	size;
	char	padding3[16]
} MSF_ENTRY;
```

The archive immediately begins with an instance of a header struct, followed by a file entry struct for each file contained within. Immediately after the final entry, the first SELF declared in the header can be found.

There is no alignment padding outside of the header, SELF entries, and SELF files themselves as they all align to 16 bytes.
