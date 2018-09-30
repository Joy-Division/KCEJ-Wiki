# Game Command Language

Game Command Language (GCL) is a scripting language created by KCE Japan West during the development of *Metal Gear Solid* in 1996. It was initially designed and implemented primarily by *Metal Gear Solid's* lead programmer, Kazunobu Uehara, after taking inspiration from [Tcl](https://en.wikipedia.org/wiki/Tcl). GCL is compiled to bytecode and relies on an interpreter library (LibGCL) in the system program to be executed.

## Table of Contents

- [Details & Syntax](#details--syntax)
- [Examples](#examples)
  - [Metal Gear Solid](#metal-gear-solid)
  - [Metal Gear Solid 2](#metal-gear-solid-2)
  - [Metal Gear Solid 4](#metal-gear-solid-4)
    - [From "Hideo Kojima's Gene"](#from-hideo-kojimas-gene)
    - [From CEDEC 2008 Presentations](#from-cedec-2008-presentations)
- [Compilation](#compilation)
- [GCX Format Description](#gcx-format-description)
  - [Timestamp](#timestamp)

## Details & Syntax

The language was designed with the use of Japanese identifiers in mind, as the members of the script unit found it easier to work in their native language.

GCL is statically typed, as dynamic typing systems were found prone to creating bugs in the hands of the script unit, which mainly consisted of non-programmers (e.g. game designers and planners).

GCL supports C-like preprocessor directives.

Comments are handled the same as in C/C++. The ``#`` character was also used in MGS1 to denote single-line comments, though it's currently unknown if this conflicted with the preprocessor and was later removed or if its commenting functionality remained a part of the language.

KCEJ developers are know to have used EUC-JP encoding for Japanese characters in source files. This must be kept standard across files, as identifiers are hashed. If different encodings are used, the hash of what may appear to have been the same string will not match.

## Examples

Various excerpts of GCL are collected here for the purpose of serving as syntax examples.

### Metal Gear Solid

Fragments of the game's GCL source files (along with other code and data) were embedded into the game's overlays for an unknown reason -- possibly memory leaks. Unfortunately, in many cases these fragments are not only incompletely but partially corrupted. The files presented here have been edited in an attempt to restore lost information.

Be aware that these scripts do not correspond with the stage the overlay belongs to. For instance, s02a.bin contains part of the script for s10a.

| Fragment | Version | Overlay |
| ----- | ----- | ----- |
| [mgsi_vr_sud01.gcl](Examples/mgsi_vr_sud01.gcl) | MGS Integral (VR Disc) | vr_sud01.bin |
| [mgs_jp_s02a.gcl](Examples/mgs_jp_s02a.gcl) | Japan (Disc 1/2) | s02a.bin |
| [mgs_jp_s02c.gcl](Examples/mgs_jp_s02c.gcl) | Japan (Disc 1/2) | s02c.bin |
| [mgs_jp_s02e.gcl](Examples/mgs_jp_s02e.gcl) | Japan (Disc 1/2) | s02e.bin |
| [mgs_us_s10ar.gcl](Examples/mgs_us_s10ar.gcl) | North America v1.1 (Disc 1/2) | s10ar.bin |
| [mgsvr_eu_vr_fms03.gcl](Examples/mgsvr_eu_vr_fms03.gcl) | MGS Special Missions | vr_fms03.bin |
| [mgsvr_eu_vr_fms04.gcl](Examples/mgsvr_eu_vr_fms04.gcl) | MGS Special Missions | vr_fms04.bin |
| [mgsvr_eu_vr_grn01.gcl](Examples/mgsvr_eu_vr_grn01.gcl) | MGS Special Missions | vr_grn01.bin |

Note: For display purposes, text encoding has been changed from EUC-JP to UTF-8. Null bytes in places that could not be restored have been replaced with ``[NUL]``. This is NOT a part of the script.

### Metal Gear Solid 2

The *Document of Metal Gear Solid 2* provides a description of how the language operates and an example of actual GCL code.

>The GCL (Game Command Language) is the script language designed for the development of MGS. It is a very versatile language. The contents of the script can be divided up into those for "program startup" and "event setting."
>In MGS, programs other than the system are designed as "parts" that can be started up by the Script Unit when putting together a stage. For example, the "player", "enemy soldier", "surveillance camera", and "door" are all "parts" that can be assigned positions and parameters freely when starting up the stage.
>Individual effects are also treated as parts whose parameters can be adjusted freely.
>"Event setting" pertains to orders causing events to happen when the player or enemy steps into or out of a predetermined area. For example, "the door will open when the player steps in front of the door" is such an order. In other words, a message is sent so that the "parts" programs that have already been started up perform their functions. These messages are predetermined.
Here is an actual example.
>```
>chara ドア DoorA1 \
>	-kms std_door \
>	-position 9325,0,-5050 \
>	-rotate 0,2048,0
>
>trap DoorA1_Area スネーク \
>	-mask 入る \
>	-exec {
>		mesg ドア DoorA1 open
>	}
>```
>The portion beginning with "chara" is what calls for a "parts" program. Such portions are give specific names ("DoorA1" in this case). The parameters assigned are the name of the door model, position, and rotation angle. All of this makes a door show up in a certain position in the game.
>The portion beginning with "trap" is what "sets an event". When "スネーク" (Snake) enters the area designated as "DoorA1_Area" by another tool, the execution block beginning with "exec" sends an "入る" (enter) order to the "ドア" (door) program called "DoorA1".
>Other situations involve multiple "condition forks" that change what happens depending on the circumstance. The entire game is pretty much composed of combinations of these parts and event sets. It might be fun to play the game trying to guess what kind of script governs each item position, camera behavior, and individual event you experience.

**Note:** Liberties have been taken with the transription so that the original, correct identifiers could be restored from the Japanese version of this excerpt.

### Metal Gear Solid 4

#### From "[Hideo Kojima's Gene](https://youtu.be/FWykspO8Gyc?t=67)"

Footage of GCL from the game was briefly shown on-screen.

See: [mgs4_making.gcl](Examples/mgs4_making.gcl)

**Note:** This example may contain errors as it was copied by hand from a somewhat blurry screen. ([Image 1](Examples/mgs4_making_a.jpg)) ([Image 2](Examples/mgs4_making_b.jpg))

#### From CEDEC 2008 Presentations

Collected from PowerPoint slides presented at CEDEC 2008. Note that these are pieces of example code, not excerpts from the game's actual scripts.

See: [mgs4_cedec2008_1.gcl](Examples/mgs4_cedec2008_1.gcl) ([Image](Examples/mgs4_cedec2008_1.jpg))

See: [mgs4_cedec2008_2.gcl](Examples/mgs4_cedec2008_2.gcl) ([Image](Examples/mgs4_cedec2008_1.jpg))

## Compilation

An in-house tool was developed for compiling GCL scripts. The tool for *Metal Gear Solid* was named "GCLCONV" and was written by lead programmer Kazunobu Uehara.

The compiled files will be named ``scenerio.gcx`` and sorted into the proper stage. There can also be additional GCX files in a given stage. In *Metal Gear Solid*, the demo theater sequence executes ``demo.gcx``, and *Metal Gear Solid 2*'s boss rush mode executes ``boss.gcx``.

``codec.dat`` in *Metal Gear Solid 2* and the following games contain GCX scripts concatenated into a streaming-type DAT archive along with various other data, usually an additional font for Japanese characters.

The "scenerio" misspelling has persisted from 1996 into the present day.

## GCX Format Description

### Timestamp

The first four bytes of each GCX script (excluding MGS1) are a Unix hexadecimal timestamp in little-endian byte order. All scripts for a given build of a game in both ``stage`` and ``codec`` share the same timestamp.

Convert the values to big-endian and input it them into a [conversion tool](https://www.epochconverter.com/hex) to see when the scripts were compiled.

See the [Date Table](DateTable.md) page for a collection of known timestamps.
