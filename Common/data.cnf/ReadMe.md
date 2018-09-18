# Data Configuration File

Data configuration files (data.cnf) are used by derivatives of the MGS engine to determine what files to load in a given data set of non-streamed data (``stage``, ``face``, ``slot``). Although always present when the files are left unpacked, data.cnf files are excluded when the .dir/.dat archives are built; the relevant information is included in the binary header of each stage/subdirectory.

## Table of Contents

- [Syntax & Examples](#syntax--examples)
  - [Slot References](#slot-references)
  - [@ Prefix](#-prefix-textures)
  - [? Prefix (MG Arcade)](#-prefix-mg-arcade)
- [Samples](#samples)
  - [Metal Gear Solid: Integral (Windows)](#metal-gear-solid-integral-windows)
  - [Metal Gear Solid 2: Substance](#metal-gear-solid-2-substance-windows)
  - [Metal Gear Solid: The Twin Snakes](#metal-gear-solid-the-twin-snakes)
  - [Metal Gear AC!D](#metal-gear-acd)
  - [Metal Gear AC!D 2](#metal-gear-acd-2)
  - [Metal Gear Solid: Portable Ops](#metal-gear-solid-portable-ops)
  - [Metal Gear Solid 4: Guns of the Patriots](#metal-gear-solid-4-guns-of-the-patriots)
  - [Metal Gear Solid: Peace Walker](#metal-gear-solid-peace-walker)
  - [Metal Gear Arcade](#metal-gear-arcade)
  - [Metal Gear Solid: Snake Eater 3D](#metal-gear-solid-snake-eater-3d)
  - [Anubis: Zone of the Enders HD Edition](#anubis-zone-of-the-enders-hd-edition)

## Syntax & Examples

One file is listed per line. In most cases, LF (Unix-style) linebreaks are used.

Lines beginning with a single period (".resident", for example) are used to set flags regarding into which portion of memory the following files are to be loaded.
```
.resident
resident_file1.txt
resident_file2.txt
.cache
cache_file1.txt
cache_file2.txt
```
The following table details known flags and their general usage.

| Flag | Description |
|-----|-----|
| .nocache | Typically used for [overlays](https://en.wikipedia.org/wiki/Overlay_(programming)) |
| .resident | Resident area, persists between stages |
| .cache | Work area, renewed upon loading a stage |
| .sound | MGS1-specific, sound data |
| .face | MGS2-specific, used for codec face data |
| .delayload | MGS4-specific, used for .dld/.dlz files |
| .delayload_w | MGS4-specific, used for .dld/.dlz files |
| .vram | PW-specific, used for .vrd/.vram pairs |

### Slot References

Beginning with the introduction of the slot system in *Metal Gear Solid 3*, groups of files within a slot archive are referenced using the following command.
```
.resident
resident_file.txt
.slot example_slot 1 12345
.endslot
.cache
cache_file.txt
.slot example_slot 3 56789
.endslot
```
This would instruct the program to load the first group of files (found at offset 12345) from "example_slot.slot" into the resident area, and the third group (found at offset 56789) to the work area.

### @ Prefix (Textures)

For a currently unknown reason, texture archive (.qar) entries are prefixed with the "@" character like so.
```
.cache
@cache.qar
cache.dar
```
It's possible this has something to do with texture units needing to be copied to VRAM via DMA many times during the frame rendering process.

### ? Prefix (MG Arcade)

Also for an unknown reason, the "?" character is used to prefix the following file in *Metal Gear Arcade*.
```
?afp_loadinfo_0000.afp
```

## Samples

### Metal Gear Solid: Integral (Windows)

Sample: [stage.mgz/stage/init/data.cnf](Sample/mgs1win_init.cnf)

Sample: [stage.mgz/stage/s00a/data.cnf](Sample/mgs1win_s00a.cnf)

Sample: [stage.mgz/stage/s01a/data.cnf](Sample/mgs1win_s01a.cnf)

Note: Most data.cnf files in this port have timestamps that date several months before the release of *Integral* for the PlayStation. They're likely unmodified from KCE Japan's originals as the PC port started development in December of 1999.

### Metal Gear Solid 2: Substance (Windows)

Sample: [cdrom.img/face/f00a/data.cnf](Sample/mgs2win_f00a.cnf)

Sample: [cdrom.img/stage/r_tnk0/data.cnf](Sample/mgs2win_r_tnk0.cnf)

Sample: [cdrom.img/stage/w00a/data.cnf](Sample/mgs2win_w00a.cnf)

### Metal Gear Solid: The Twin Snakes

Sample: [shared/stage/preview/data.cnf](Sample/mgstts_preview.cnf)

*The Twin Snakes* uses a stage.dat archive. This is the only file remaining in the **shared/stage** directory on the disc (North American and Japanese versions). This file is not present on either disc for debug build v099.1 or the European release. It's highly likely the rest followed the same rules as **Metal Gear Solid 2**.

### Metal Gear AC!D

Sample: [stage/init/_zar/data.cnf](Sample/mga1_init.cnf)

Sample: [stage/st00/_zar/data.cnf](Sample/mga1_st00.cnf)

Sample: [stage/st01/_zar/data.cnf](Sample/mga1_st01.cnf)

### Metal Gear AC!D<sup>2</sup>

Sample: [stage/init/_zar/data.cnf](Sample/mga2_init.cnf)

Sample: [stage/st0000/_zar/data.cnf](Sample/mga2_st0000.cnf)

Sample: [stage/st0100/_zar/data.cnf](Sample/mga2_st0100.cnf)

### Metal Gear Solid: Portable Ops

Sample: [stage/r_main01/_zar/data.cnf](Sample/mpo_r_main01.cnf)

Sample: [stage/st01/_zar/data.cnf](Sample/mpo_st01.cnf)

### Metal Gear Solid 4: Guns of the Patriots

Sample: [mgs/stage/init/data.cnf](Sample/mgs4_init.cnf)

Sample: [mgs/stage/mg_setup/data.cnf](Sample/mgs4_mg_setup.cnf)

Sample: [mgs/stage/mg_setup1/data.cnf](Sample/mgs4_mg_setup1.cnf)

Sample: [mgs/stage/title/data.cnf](Sample/mgs4_title.cnf)

Sample: [mgs/stage/r_sna01/data.cnf](Sample/mgs4_r_sna01.cnf) (MGS4 Demo Version)

Sample: [mgs/stage/s01a10l/data.cnf](Sample/mgs4_s01a10l.cnf) (MGS4 Demo Version)

Note: The same flags tend to appear multiple times.

### Metal Gear Solid: Peace Walker

Sample: [STAGEDAT.PDT/r_main01/data.cnf](Sample/mgspw_r_main01.cnf)

Sample: [STAGEDAT.PDT/w00s01a/data.cnf](Sample/mgspw_w00s01a.cnf)

### Metal Gear Arcade

Sample: [cdrom.img/stage/init_n/data.cnf](Sample/mgac_init_n.cnf)

Sample: [cdrom.img/stage/n001a/data.cnf](Sample/mgac_n001a.cnf)

Sample:  [cdrom.img/stage/r_sna01_n/data.cnf](Sample/mgac_r_sna01_n.cnf)

*Metal Gear Arcade*'s system is set up with a special case to detect .afp and .xml files and retrieve them from ``cdrom.img/afp`` instead of interpreting the entry as a file in the current directory.

### Metal Gear Solid: Snake Eater 3D

Sample: [romFS/stage/r_sna01/data.cnf](Sample/mgs3d_r_sna01.cnf)

Sample: [romFS/stage/s000a_0/data.cnf](Sample/mgs3d_s000a_0.cnf)

### Anubis: Zone of the Enders HD Edition

Sample: [ZoE2/stage/init/data.cnf](Sample/zoe2hd_init.cnf)

Sample: [ZoE2/stage/vr01/data.cnf](Sample/zoe2hd_vr01.cnf)

Sample: [ZoE2/stage/zakat/data.cnf](Sample/zoe2hd_zakat.cnf)

ZOE2 HD used a stage.dat archive equivalent to the original release. It's currently unknown what use these are to the game.
