# horus_frk
Extracting Horus internal RF firmware


```
Commands:
=========

Find prolog:
------------

$ binwalk -R "\x79\x09\xAA\x9A\xBE\x70\x25\xB3\x7C\xF9\x87\x5F\xAA\x7C\xC3\xD1" X10*.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
1793748       0x1B5ED4        \x79\x09\xAA\x9A\xBE\x70\x25\xB3\x7C\xF9\x87\x5F\xAA\x7C\xC3\xD1


Find end-of-firmware:
---------------------

$ binwalk -R "\x00\x00\x00\x00" -l 80000 -o 0x1B5ED4 X12S_mode1_NEU_1603_frtx.bin | head -n 5

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
1867476       0x1C7ED4        \x00\x00\x00\x00
1867486       0x1C7EDE        \x00\x00\x00\x00


Check what is before the prolog:
--------------------------------

$ binwalk -W -l 16 -o 0x1B5EC4 X12S_mode1_NEU_1603_frtx.bin

OFFSET      X12S_mode1_NEU_1603_frtx.bin
--------------------------------------------------------------------------------
0x001B5EC4  00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00 |...........<....|


Check epilog:
-------------

$ binwalk -W -l 16 -o 0x1C7ED4 X12S_mode1_NEU_1603_frtx.bin

OFFSET      X12S_mode1_NEU_1603_frtx.bin
--------------------------------------------------------------------------------
0x001C7ED4  00 00 00 00 01 20 01 00 4A 31 00 00 00 00 00 00 |........J1......|

-> firmware size: 0x012000 (72KB)

Cut firmware out:
-----------------

$ dd bs=1 skip=0x1B5ED4 if=X12S_mode1_NEU_1603_frtx.bin of=X12S_NEU_1603_iXJT.frk count=72k
73728+0 records in
73728+0 records out
73728 bytes transferred in 0.598368 secs (123215 bytes/sec)



Offset       Filename                           Before prolog                                     Epilog / Size
===========================================================================================================================================
0x110C5C     X10S_EU_1102_frtx.bin              00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000
0x110C0C     X10S_NEU_1102_frtx.bin             00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000

0x1220D0     X10_mode1_EU_1204_frtx.bin         00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000
0x1220D8     X10_mode2_EU_1204_frtx.bin         00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000

0x121DF0     X10_mode1_NEU_1204_frtx.bin        00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000
0x121DF8     X10_mode2_NEU_1204_frtx.bin        00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000

0x12C04C     X12S_NEU_1404_frtx.bin             00 00 00 00 00 00 10 3C 00 80 2A 44 01 18 01 00   size: 0x011800
0x12C0C4     X12S_EU_1404_frtx.bin              00 00 00 00 00 00 10 3C 00 80 2A 44 01 18 01 00   size: 0x011800

0x136D14     X12S_mode2_NEU_1505_frtx.bin       00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000
0x136D0C     X12S_mode1_NEU_1505_frtx.bin       00 00 00 00 00 00 10 3C 00 80 2A 44 01 20 01 00   size: 0x012000

0x13704C     X12S_mode2_EU_1505_frtx.bin        00 00 00 00 00 00 10 3C 00 80 2A 44 01 28 01 00   size: 0x012800
0x137044     X12S_mode1_EU_1505_frtx.bin        00 00 00 00 00 00 10 3C 00 80 2A 44 01 28 01 00   size: 0x012800

0x19FE1C     X10_EU_1304_Beta0228_frtx.bin      00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 20 01 00    size: 0x012000
0x19FAF4     X10_FLEX_1304_Beta0228_frtx.bin    00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 20 01 00    size: 0x012000
0x19FA34     X10_NEU_1304_Beta0228_frtx.bin     00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 20 01 00    size: 0x012000

0x1A259C     X12S_NEU_1225_frtx.bin             00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 18 01 00    size: 0x011800
0x1A25F4     X12S_EU_1225_frtx.bin              00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 18 01 00    size: 0x011800

0x1B5ED4     X12S_mode1_NEU_1603_frtx.bin       00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 20 01 00    size: 0x012000
0x1B5ED4     X12S_mode2_NEU_1603_frtx.bin       00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 20 01 00    size: 0x012000

0x1B5F8C     X12S_mode1_FLEX_1603_frtx.bin      00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 20 01 00    size: 0x012000
0x1B5F8C     X12S_mode2_FLEX_1603_frtx.bin      00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 20 01 00    size: 0x012000

0x1B62B4     X12S_mode1_EU_1603_frtx.bin        00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 28 01 00    size: 0x012800
0x1B62B4     X12S_mode2_EU_1603_frtx.bin        00 00 00 00 00 00 00 00 00 00 10 3C 2E 00 00 00   00 00 00 00 01 28 01 00    size: 0x012800
```
