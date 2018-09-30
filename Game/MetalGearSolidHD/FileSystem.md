# MGS HD File System

Bluepoint Game's standard operating procedure when starting a remaster project is to extract a game's assets from a retail copy of the original, rather than to rely solely on what the client provides. This is done to sidestep the issue of source assets going missing, which is unfortunately quite common in the industry.

## Table of Contents

- [Bluepoint Archives](#bluepoint-archives)
- [Directory Structure](#directory-structure)
- [Stream Data](#stream-data)
- [Archive Data](#archive-data)
  - [File Names & Extions](#file-names--extensions)
- [Sorting](#sorting)

## Bluepoint Archives

Data is stored using different archives formats depending on the platform.

| Platform | Extension | Format |
| ----- | ----- | ----- |
| PlayStation 3 | .psarc | PSARC |
| Xbox 360 | .xbarc | unknown |
| PlayStation Vita | .psp2arc | PSARC |
| Nvidia Shield | .obb | PKZIP |

The following archives can be found in the various editions of the games excluding the Nvidia Shield port.

**Note:** Some of these may be incorrect as not all builds of the game have been checked.

**Note:** For European builds ``_eu`` is appended to the archive names, and ``_jp`` for Japanese builds.

| Archive Name | Contents |
| ----- | ----- |
| bootloader | Game select menu data |
| mgs2 | Main game data |
| mgs2_1 | Xbox 360-only archive |
| mgs2_common | Xbox 360-only archive |
| mgs2_misc | Additional data |
| mgs2d | Update data |
| mgs3 | Main game data |
| mgs3_1 | Xbox 360-only archive |
| mgs3_common | Xbox 360-only archive |
| mgs3_misc | Additional data |
| mgs3d | Update data |

The Nvidia Shield ports handle installation by downloading in two phases. The main archive is retrieved during the initial install and a patch file is downloaded afterwards. This is presumably a workaround for some kind of download limit.

The file names for MGS2 HD's archives are as follows:
```
main.14.jp.konami.mgs2hd.shield.obb
patch.14.jp.konami.mgs2hd.shield.obb
```

## Directory Structure

The basic root directory structure is as follows:

| Directory | Contents |
| ----- | ----- |
| assets | Assets sorted by extension |
| EngineSupport | Fonts and shaders, Vita & Shield-only |
| eu | European data |
| face | Textures from face.dat |
| fr | French/English data |
| gr | German data |
| it | Italian data |
| jp | Japanese data |
| misc | Supplementary data, etc. |
| slot | Textures from slot.dat |
| sp | Spanish data |
| stage | Textures from stage.dat |
| textures | All .ctxr files under "flatlist" subdirectory |
| us | North American data |

Instead of packing files into .dat archives like the original PS2 games, MGS HD keeps the data unpacked. The files are sorted into a directory named after their origin. For example, streams from the US version of MGS3's bgm.dat are found under ``us/bgm``.

**Exception:** The contents vox and vox2 are merged in MGS2 HD.

Each region/language has its own set of these subdirectories, though since the European language versions use the English dub, they merely contain text files pointing to the US version's files. In most cases, the US data prevails.

**Exception:** The European data for codec streams and GCL scripts is preferred even in the North American versions of the HD Editions.

[data.cnf](../../Common/data.cnf/ReadMe.md) files have not been recreated but instead Bluepoint has devised a new system for referring to assets using text files.

| File | Contents |
| ----- | ----- |
| bp_streams.txt | Refers to stream data |
| bp_assets.txt | Refers to CMDL/CTXR files |
| manifest.txt | Refers to PlayStation 2 assets |

## Stream Data

In the PS2 originals, stream .dat archives in are simply multiple stream files concatinated together. The system referred to streams by the starting offset of the required stream (often divided by 0x800). This system has been preserved via a look-up-table that composes bp_streams.txt.

- Excerpt of ``eu/demo/_bp/_ps3/bp_streams.txt`` from MGS2 HD
```
0000000000 us/demo/_bp/t01a1D.sdt
0006410240 us/demo/_bp/t02a1D.sdt
0025880576 us/demo/_bp/t02a2D.sdt
```
The original file names have been restored -- likely from information provided in Konami's source package. These names would have been impossible to recover without guesswork otherwise.

The ``codec.dat`` stream file names in both games are rather strange and inconsistent. As evidenced from a fragment of a map of MGS1's ``radio.dat``, these file names were originally written mostly in Japanese. Bluepoint -- being made up of English-speaking staff -- likely had them machine-translated and then inserted the translations into the game.

Another point of note is that stream data is always given a ``.sdt`` extension in MGS HD. Whether or not this is original extension remains unknown, albeit unlikely, as other games with unpacked streams give no hint of this practice.

## Archive Data

Files from ``stage.dat``, ``face.dat``, and ``slot.dat`` have all been unpacked and sorted into their respective subdirectories.

Files are further sorted into subdirectories based on the flags originally assigned to them by [data.cnf](../../Common/data.cnf/ReadMe.md), primarily cache and resident. Since overlays aren't used by MGS HD, nocache does not appear and the sound files (.sdx) are placed in the stage's root.

**Note:** Both MGS2 HD and MGS3 HD's sound files are given the extension .sdx, however, the original PS2 version of MGS3's sound format extension is in fact ``.psq``. This is the result of a mistake made on the part of Bluepoint Games that went uncorrected.

- Excerpt of ``eu/stage/a00a/bp_assets.txt`` from MGS2 HD;
```
textures/flatlist/000385a3.ctxr,stage/a00a/cache/000385a3.ctxr,eu/stage/a00a/cache/00fc06e8/000385a3.ctxr
textures/flatlist/0003c3de.ctxr,stage/a00a/cache/0003c3de.ctxr,eu/stage/a00a/cache/00000f32/0003c3de.ctxr
textures/flatlist/00085d1e.ctxr,stage/a00a/cache/00085d1e.ctxr,eu/stage/a00a/cache/00fc06e8/00085d1e.ctxr
```
- Complementing ``eu/stage/a00a/manifest.txt``
```
assets/tri/us/2d_tex.tri,us/stage/a00a/cache/00715d80.tri,cache/00715d80.tri
assets/tri/us/cameratex.tri,us/stage/a00a/cache/00b80129.tri,cache/00b80129.tri
assets/tri/us/comdlcache.tri,us/stage/a00a/cache/00349b50.tri,cache/00349b50.tri
```

Each file lists three different paths to two different copies of a given file. ``manifest.txt``'s latter two paths are (near) absolute and relative paths to the same given file.

### File Names & Extensions

When building the ``.dat`` archives in the PlayStation 2 originals, all file names were replaced with a 24-bit hash. Bluepoint made some attempt to restore the original names in most places. The results, however, aren't always correct. Many names remain unrestored, case wasn't strictly respected, some names are simply incorrect, and in one instance a set of data files for MG1 and 2 were purposefully renamed for seemingly no reason.

MGS2 refers to file name extensions using a single-byte hash of *only the first character.* For instance, there are at least 12 unique extensions beginning with the letter "r" in a given version of MGS2. Bluepoint handled this by choosing one extension of its respective letter and assigning it to all files. Hence all 12 "r" extensions are now ".row" in MGS2 HD.

MGS3 handles extensions by assigning a single-byte ID to each extension, and thus the extensions in MGS3 HD are (most likely) correct.

## Sorting

The first paths of ``bp_assets.txt`` and ``manifest.txt`` point to a common pool of files.

For the ``assets`` directory, each extension is given a subdirectory and then another subdirectory within that to separate files by region. For example, the resulting file path for a file named "scenerio.gcx" belonging to a Japanese build would be ``assets/gcx/jp/scenerio.gcx``.

Naturally, there are many files named "scenerio.gcx". In this case, the system will first determine if any are duplicates, exclude them, and then append the original path of the file to the file name. The resulting file list would look something like this:
```
assets/gcx/jp/scenerio.gcx
assets/gcx/jp/scenerio_stage_s001a.gcx
assets/gcx/jp/scenerio_stage_s002a.gcx
...
```

Files modified by Bluepoint are further sorted into another subdirectory, and even further depending on if a file was marked as platform-specific.

| Example Directory | Description |
| ----- | ----- |
| demo | Files unmodified from the PS2 data |
| demo/_bp | Files modified by Bluepoint |
| demo/_bp/_360 | Files modified for Xbox 360 |
| demo/_bp/_ps3 | Files modified for PlayStation 3 |
| demo/_bp/_vta | Files modified for PlayStation Vita |
| demo/_bp/_win | Files modified for Nvidia Shield |

**Note:** Although ``bp_streams.txt`` properly points to the data, ``bp_assets.txt`` does not. For example the real location of ``textures/flatlist/000385a3.ctxr`` is ``textures/flatlist/_vta/000385a3.ctxr`` in the Vita port.
