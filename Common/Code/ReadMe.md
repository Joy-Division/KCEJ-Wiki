# KCE Japan West Coding Conventions

KCE Japan West, like anyway software studio, had their own style, practices, and quirks when it came to writing a game's code. Some of this information has survived into various retail, demo, and debug builds of their games.

## Table of Contents

- [Style](#style)
  - [Character Encoding](#character-encoding)
  - [Identifier Naming](#identifier-naming)
- [Program Structure](#program-structure)
  - [Game](#game)
  - [System](#system)
  - [Kernel](#kernel)
  - [User](#user)
- [Source Tree](#source-tree)
  - [Library Archives](#library-archives)
  - [cdrom.img](#cdromimg)
  - [Known Source Files & Paths](#known-source-files--paths)
- [Source Control](#source-control)
  - [CVS](#cvs)
  - [StarTeam](#star-team)

## Style

KCE Japan West seemingly did not enforce a very strict coding standard. Certain styles prevail over others but they are not kept entirely consistent throughout a given codebase.

The following are examples of varying style:
- Indentation alternates between tabs and spaces. In addition, no intended tab display with seems to have been established, usually intended to appear either 4 or 8 characters in width.
- Line breaks alternate between Unix ``LF`` and Microsoft ``CRLF``.
- In some places within *Metal Gear Solid's* codebase, K&R style argument declaration was used -- notably within certain inline function definitions for Game, LibDG, and LibGCL.

### Character Encoding

EUC-JP is the standard Japanese character encoding used in KCE Japan West's C/C++ and GCL code. The use of this encoding is mandatory for the sake of generating the same GCL identifier hashes throught the codebase.

In places where a specific character encoding is not mandated, Shift-JIS is often used.

### Identifier Naming

Given that the coding standard was relatively lax, inconsistencies can be found in a given game's codebase. Still, certain styles tend to dominate.

The following points are examples of the most common styles:
- Local variable and structure names are often written in ``snake_case``.
- Global variable and structure names are often written in ``PascalCase``.
- Structure type names, defines and enum members are written in ``ALL_CAPS``, with ``PascaleCase`` serving as a secondary style.
- Function names are either written in either ``PascalCase`` or ``snake_case`` with most preferring the former.
  - Function names typically include a verb or imply some sort of action, helping to distinguish them from global variables.
  - Functions belonging to a certain library/actor/etc. are prefixed with its short name. (e.g. ``GCL_GetByte`` is a part of LibGCL.)
  - Functions responsible for creating a new object, actor, etc. often begin with the the word "New" (e.g. ``NewSoundTest``).
- Source file and path names are written in ``snake_case``, with ``PascaleCase`` serving as a secondary style.
- The extension ``.cc`` is used for C++ code, with ``.h`` being used for both C and C++ headers.
  - The more standard ``.cpp`` extension was adopted circa 2009 for the development of *Metal Gear Solid: Peace Walker* and the Fox Engine.

## Program Structure

KCE Japan West games are modeled on a common system structure established with the development of *Metal Gear Solid*. The system is built on top of a multitasking kernel/thread wrapper library known as the Multi-Task Scheduler (MTS). Most libraries, with the exception of those designed for providing generic functions such as mathematic algorithms, initialize a [daemon](https://en.wikipedia.org/wiki/Daemon_(computing)) process that servers as a part of the games' core system. For example, LibDG implements the DG daemon -- the process responsible for image rendering and display.

The *Document of Metal Gear Solid 2* provides a description of this system under the "Program > System Structure" section.

>The MGS2 system is structured as modules which are divided up as follows:
>```
>+------+-----------------------------------------+
>|      | Programs of the player, enemy           |
>| USER | soldiers, doors and bottles,            |
>|      | effects.                                |
>+------+-----------------------------------------+
>|      | A group of functions that can be        |
>|      | conveniently used for MGS2 and          |
>| GAME | the program managing the game           |
>|      | progress with system program            |
>|      | combinations.                           |
>+------+-----------------------------------------+
>|      | script language interpreter library     |
>|      |-----------------------------------------+
>|      | file/streaming system library           |
>|      |-----------------------------------------+
>|      | collision/zone management library       |
>|      |-----------------------------------------+
>|      | screen display library                  |
>|      |-----------------------------------------+
>|SYSTEM| motion processing library               |
>|      |-----------------------------------------+
>|      | miscellaneous multipurpose              |
>|      | calculation libraries                   |
>|      |-----------------------------------------+
>|      | memory management, execution            |
>|      | unit control library                    |
>+------+-----------------------------------------+
>|      | sound processing library                |
>|      |-----------------------------------------+
>|KERNEL| thread management wrapper library       |
>|      |-----------------------------------------+
>|      | CD/DVD control library                  |
>+------+-----------------------------------------+
>```
>Each USER program is written as a completed part (Actor).
Their activation and messages to them are controlled mainly by our original script language called GCL.
>For job efficiency, tasks are divided among the programmers creating each Actor and the Script Unit creating the games by placing each actor.
>One characteristic of this system is that when debugging, it constantly measures how long it takes for each Actor's processing. Knowing how long each processing takes allows us to locate which program is "heavy."
>This is written in C language. Inline Assembler is used in some areas.
>The source file is managed by CVS, and the compiling is done on Linux.
>There are 2600 source files (excluding self-generating files), totaling more than 1.15 million lines.

### Game

The game module implements functions for controlling the state of the overall game and managing various tasks, as well as implementing several libraries such as the camera daemon, sound control, etc.

### System

The system module is where the core libraries and most daemons reside. These implement each game's lower-level processes.

Common libraries include the following:

| Library | Description | Notes |
| ----- | ----- | ----- |
| LibDG | Image-drawing and display library. Given the highly specialized nature of its task, it has been heavily modified over th course of its history. | KCE Japan's series of *beatmania* ports to the PlayStation make use of their own image-drawing libraries LibOBJ and LibSRN. |
| LibFS | File system access and streaming library -- often specialized for reading from optical media with HDD functions removed for retail builds. |
| LibGEO | New collision library developed for *Metal Gear Solid 3* after LibHZD was found to be unsuitable for the game's jungle environment. |
| LibGV | Memory management and execution control library. This coordinates messages sent between actors, manages the games' various data heaps, and routes pad, mouse, and keyboard input. |
| LibGCL | GCL script language interpreter. | The equivalent library in the *Zone of the Enders* is named LibSCN. |
| LibHZD | Collision and zone management library. | Renamed to "LibHZX" after presumably being overhauled during the development of *Metal Gear Solid 2*, though it remained "LibHZD" for both PlayStation 2 *Zone of the Enders* games and was moved into the game module for *Anubis: Zone of the Enders*. LibHZD was replaced with LibGEO in *Metal Gear Solid 3* onwards. Its name is short for "hazard." |
| LibLA | 2D layout processing library, typically used for UI elements. | The equivalent library in *Anubis: Zone of the Enders* is LibSPR. |
| LibMA | Common, multi-purpose mathematics functions. | The equivalent library in both PlayStation 2 *Zone of the Enders* games is LibALG. LibGV in *Metal Gear Solid* seemed to fulfill this role. |
| LibMC | Library for reading and writing save game and other files stored on PlayStation memory cards. |
| LibMT | Motion processing library. LibMT also handles sound effect cues attached to animations. |
| LibNT | Network connection library.| Possibly introduced in *Metal Gear Online* (2005). |
| LibNAV | New navmesh processing library. | Possibly introduced in *Metal Gear Solid 3.* |
| LibPHYS | Physics processing library. | Possibly introduced in *Metal Gear Solid 4*. |
| LibUTL | Miscellaneous function library. |
| LibZON | New zone management library. | Possibly introduced in *Metal Gear Solid 3.* |

### Kernel

The kernel module includes libraries separate from the rest of the system libraries, as well as third-party libraries.

Common libraries include the following:

| Library | Description | Notes |
| ----- | ----- | ----- |
| MTS | Thread wrapper/management library. | Short for "Multi-Task Scheduler". Originally written for *Policenauts*. |
| SOUND | Sound processing/playback library. | On PlayStation 2, this is compiled and executed as an IOP module. |
| ZLIBDEC | A copy of [zlib](https://zlib.net/) with the deflate functionality removed. | Typically zlib version 1.1.3. |

### User

The user-level module primarily deals with implementing actors and other game-specific functions. Naturally, its contents vary a great deal between games.

## Source Tree

Aside from the structure of the actual program, each project's source tree is patterned after a common model KCE Japan West's programmers established at some point. *Metal Gear Solid* is the first game with enough information retained throughout its various builds for these practices to be known and documented.

The codebase for each project is split between two main directories.
- ``source``: contains the majority of the game's code
- ``module``: contains the MTS, sound, third-party, and other miscellaneous libraries

Each contains a pair of subdirectories named ``include`` and ``lib``, which are used to store common headers and library archives respectively.

The subdirectory ``source/main`` often contains a single source file that defines the program entry point (the ``main()`` function).

User-level code is sorted into subdirectories named after the programmer assigned to implement it. Such naming is a common practice for Japanese game developers across the industry and can be observed in the codebases of various non-Konami games such as Grasshopper Manufacture's *Killer7* and SCE Japan Studio's *ICO*.

There are two directory structures for ``source`` different games follow. The first can be observed in *Metal Gear Solid*, *beatmania APPEND 5thMix*, and *Zone of the Enders*.

- Sample representation:
```
mgs
├─module
│  ├─include
│  ├─lib
│  ├─mts
│  ├─sound
│  └─zlibdec
└─source
    ├─game
    ├─include
    ├─korekado
    ├─lib
    ├─libdg
    ├─libfs
    ├─libgcl
    ├─libgv
    ├─libhzd
    ├─libmt
    ├─main
    ├─okajima
    ├─sonoyama
    ├─takabe
    └─uehara
```

A new directory structure was standardized with *Metal Gear Solid 2* onwards. Libraries and user-level code are now further sorted unto the new sudirectories ``system`` and ``user``.

- Sample representation:
```
mgs
├─module
│  ├─include
│  ├─lib
│  ├─mts
│  ├─sound
│  └─zlibdec
└─source
    ├─game
    ├─include
    ├─lib
    ├─main
    ├─system
    │  ├─libdg
    │  ├─libfs
    │  ├─libgcl
    │  ├─libgv
    │  ├─libhzd
    │  └─libmt
    └─user
        ├─korekado
        ├─okajima
        ├─sonoyama
        ├─takabe
        └─uehara
```

### Library Archives

Object code is compiled into library archives before being linked when building the full program. Each library or subdirectory of ``source/user`` corresponds to an archive file.

For the above source tree samples, the following archives would be compiled:
```
module/lib/mts.a
module/lib/sound.a
module/lib/zlibdec.a
source/lib/game.a
source/lib/korekado.a
source/lib/libdg.a
source/lib/libfs.a
source/lib/libgcl.a
source/lib/libgv.a
source/lib/libhzd.a
source/lib/libmt.a
source/lib/okajima.a
source/lib/sonoyama.a
source/lib/takabe.a
source/lib/uehara.a
```

### cdrom.img

A directory named ``cdrom.img`` serves as the host path for running builds of a game from a host machine's hard disk drive or other media during development.

In the Windows port of *Metal Gear Solid 2: Substance* and *Metal Gear Arcade*, ``cdrom.img`` survived into the final builds of the games and serves as the root directory for game data.

The earliest appearance of ``cdrom.img`` can be found within the PlayStation version of *Policenauts* in the file ``kernel.sc``.

### Known Source Files & Paths

The following lists are compilations of known source file paths that have been collated from game data and/or executable code:
- [Metal Gear Solid / Metal Gear Solid: Integral](path_mgs1psx.txt)
- [Metal Gear Solid: Integral (Windows)](path_mgs1win.txt)
- [Metal Gear Solid 2: Substance (Xbox)](path_mgs2xbox.txt)
- [Metal Gear Solid 2: Substance (Windows)](path_mgs2win.txt)
- [Anubis: Zone of the Enders](path_zoe2.txt)
- Metal Gear Solid 4: Guns of the Patriots
- [Metal Gear Arcade](path_mgac.txt)
- [Metal Gear Solid 2 HD Edition](path_mgs2hd.txt)
- [Metal Gear Solid 3 HD Edition](path_mgs3hd.txt)
- [Metal Gear Solid: Peace Walker HD Edition](path_mgspwhd.txt)
- [Zone of the Enders HD Collection (Bootloader)](path_zoehd.txt)
- [Anubis: Zone of the Enders HD Edition](path_zoe2hd.txt)
- [Anubis: Zone of the Enders MARS](path_zoe2mars.txt)

**Note:** *Anubis: Zone of the Enders* has been edited somewhat from the information originally found within the game for the sake of clarity and completeness. For example paths have been expanded and original source files only listed in later ports of the game have been included.

## Source Control

### CVS

As mentioned in *The Document of Metal Gear Solid 2*, [CVS](https://en.wikipedia.org/wiki/Concurrent_Versions_System) was used as the revision management system for the development of *Metal Gear Solid 2*. A ``.cvsignore`` file was included with the Windows port of *Metal Gear Solid 2: Substance* within the directory ``cdrom.img/modules``.

Several CVS files were included in Anubis: Zone of the Enders HD Edition within the directory ``USRDIR/ZoE2/stage/CVS``.

### StarTeam

Silicon Knights is known to have used Borland [StarTeam
](https://en.wikipedia.org/wiki/StarTeam) to manage assets and possibly code during the development of *Metal Gear Solid: The Twin Snakes* as evidenced by the various StarTeam metadata files left on the discs of the retails builds of the game.
