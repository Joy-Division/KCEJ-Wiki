# DPK Archive

DPK archives are a generic file container used to pack various game data.

## Table of Contents

- [Basic Information](#basic-information)
- [File Structure](#file-structure)

## Basic Information

DPK archives are used by the game system as a sort of virtual directory. They are defined in ``KERNEL.SC`` with the command ``fsfile BIN.DPK``for instance. From there, opening a file within named "ITP.BIN" would be done by accessing ``BIN\ITP.BIN``.

DPK archives are found only in the PlayStation and Saturn versions.

## File Structure

The DPK header is made up of the following structures:

```c
typedef struct _DPK_HEADER {
	uint32 formatID;	// 'DIRF'
	uint32 attribute;
	uint32 blocksize;	// always 0x800
	uint32 filecount;
	uint32 length;
	uint32 unknown;		// always 0x18
	char padding[8];	// zero-filled
} DPK_HEADER;
```
```c
typedef struct _DPK_ENTRY {
	char name[12];
	uint32 offset;
	uint32 length;
	uint32 checksum;
} DPK_ENTRY;
```

The archive immediately begins with an instance of a header struct, followed by a file entry struct for each file contained within.

The endian signature is known to be either one of two values:
- 0xE0000000 for little-endian
- 0xA0000000 for big-endian

Note that, for an unkown reason, the PlayStation versions of the Tokimeki Memorial Drama Series use both big and little-endian DPKs, despite the PlayStation being a little-endian machine. This is not the case for the Saturn versions.

Files have been observed to always be stored in alphanumeric order.

Each file within the archive will always have its starting offset located at a multiple of 0x800 -- the size of a CD sector. Null bytes are used to pad the space between the header and each file. The space between the final file and the end of the archive is also padded so that the size of the archive itself will be a multiple of 0x800 as well.
