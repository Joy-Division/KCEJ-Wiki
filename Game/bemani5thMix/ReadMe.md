# beatmania APPEND 5thMIX -Time to Get Down-

*beatmania APPEND 5thMix -Time to Get Down-* is a port of the arcade game *beatmania 5thMix* and acts as an "Append Disc" for the original PlayStation *beatmania* release.

## Table of Contents

- [Development Information](#development-information)
  - [Development Team](#development-team)
- [Source Code](#source-code)
  - [DUMMY (LZH Archive)](#dummy-lzh-archive)

## Development

##### Sony PlayStation
- Developer: KCE Japan West: Production Department #2
- Development Period: 1999/09/28~2000/01/13
- Mastering Date (JP): 1999/11/25
- Release Date (JP): 2000/03/02
- Product Code: SLPM-86322

### Development Team

Note: The staff listing of the original arcade *beatmania 5thMix* can be found [here](http://www.mobygames.com/game/playstation/beatmania-append-5th-mix-time-to-get-down/credits).

- **Producer**
  - Motoyuki Yoshioka
  - Hideo Kojima
  - Noriaki Okamura
- **Director**
  - Hajime Yashiro
- **'Bonus Edit' Sound Organizer**
  - Hiroyuki Tougo
- **Programmer**
  - Tetsuya Funakubo
  - Tan Yudon
  - Taisei Nomura
  - Masao Tomosawa
- **Composer & Arranger**
  - Toshiyuki Kakuta
  - Hiroyuki Tougo
  - Takao Tsuchihama (DJ TAKAWO)
  - Kaori Nakagawa (K.O.O.STUDIO)
- **CG Designer**
  - Hajime Yashiro
  - Satoshi Higashida
  - Koichi Narita
  - Atsuko Ito
  - Yoshinori Natsume
- **Lyricist**
  - Hiroyuki Togo
- **Vocalist**
  - AKANE
  - Sanae 'Sana' Shintani
  - Hiroyuki Togo
- **DJ Performer**
  - Takao Tsuchihama (DJ TAKAWO)
- **Artist Coordinator**
  - Akira Uchibori (GUHROOVY)
- **Software Manual**
  - Takashi Kinbara
- **Package Design**
  - Takashi Kinbara
- **Thanks To**
  - Kazuki Muraoka
  - Kazunari Okido
  - KME 'BEMANI' Staff
  - Seiji Higurashi (GM)
  - GM arcade beatmania team
- **Debugging**
  - RMC
- **Development**
  - KCE Japan
  - BEMANI
- **Produced by KONAMI**

## Source Code

A majority of the game's source code can be found within the original issue of *beatmania BEST HITS* (SLPM-86596). The file ``cdrom0:\DUMMY`` is an LZH archive that contains the directory ``C:\5thMix\work.5th`` from programmer Tan Yudon's workstation hard disk. This file was made into garbage data for the "Konami The BEST" reissue (SLPM-86830).

While a working executable can be compiled using the Psy-Q PlayStation SDK, a number of the game's libraries are missing their original source and must be used as-is. These libraries are as follows:
```
LIB/LIBDM.LIB
LIB/LIBOBJ.LIB
LIB/MIYA.LIB
LIB/MTS.LIB
LIB/MTS_DBG.LIB
LIB/MTSCD.LIB
LIB/MTSMCD.LIB
LIB/NOLIBSIO.LIB
LIB/SIO.LIB
LIB/SOUND.LIB
```
Several of *APPEND 5thMix*'s libraries were originally written by Production Department #1 for *Metal Gear Solid* -- namely LibFS, the Multi Task Scheduler (MTS), Kazuki Muraoka's sound library, and possibly more.

### DUMMY (LZH Archive)

- [LZH Archive File List](DUMMY.txt)

A list of files contained within the source archive generated using 7-Zip's list command.

- [LIB Source File List](libsource.txt)

A list of unique source paths that were included in the library files. Note that it's not a complete listing of all source files. (It lists mostly only C files and a few headers.)
