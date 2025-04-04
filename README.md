# CBT299
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 299 contains the source for the TAPEMAP program.  This    *   FILE 299
//*           version is a compilation of the original code from    *   FILE 299
//*           UCLA (that used to be in File 149) and the modified   *   FILE 299
//*           code that used to be in File 316 from the Air Force,  *   FILE 299
//*           and includes many additional changes from various     *   FILE 299
//*           places and various people.                            *   FILE 299
//*                                                                 *   FILE 299
//*           A load module library in XMIT format has now been     *   FILE 299
//*           included in this pds.  Please see member LOADLIB$     *   FILE 299
//*           for JCL to create the load library on your system.    *   FILE 299
//*                                                                 *   FILE 299
//*           Y2K fix to day of week routine, by Joel Ewing.        *   FILE 299
//*           This is in the TODAY CSECT.                           *   FILE 299
//*                                                                 *   FILE 299
//*   ---->>  *BEGIN* NEWS: ------                                  *   FILE 299
//*                                                                 *   FILE 299
//*           Another version of TAPEMAP, as member TAPEMAPM,       *   FILE 299
//*           contains changes from Steve Myers to accommodate      *   FILE 299
//*           File Sequence Numbers greater than 9999.  The         *   FILE 299
//*           reason that this is being presented as a separate     *   FILE 299
//*           member, is that it reformats the reports somewhat.    *   FILE 299
//*           That version of TAPEMAP (TAPEMAPM) has been marked    *   FILE 299
//*           as Version 2.5.1 to differentiate it from the other   *   FILE 299
//*           TAPEMAP (Version 2.5), and from the TAPEMAP version   *   FILE 299
//*           on File 804 (Version 2.5.3).                          *   FILE 299
//*                                                                 *   FILE 299
//*           Updated with a new list of DASD device types for      *   FILE 299
//*           the map of FDR tapes.  (J. Kalinich and Bruce Black)  *   FILE 299
//*                                                                 *   FILE 299
//*           There is another version of TAPEMAP, which resides    *   FILE 299
//*           in CBT File 804.  That version corresponds in level,  *   FILE 299
//*           to our Version 2.5, but it has replaced most of       *   FILE 299
//*           the branch instructions (e.g. BNE) with jump          *   FILE 299
//*           instructions (e.g. JNE), and 3 base registers are     *   FILE 299
//*           saved in the process.  The current version of TAPEMAP *   FILE 299
//*           here, has not been replaced, because of the MVS 3.8   *   FILE 299
//*           people who are assembling this program using IFOX00.  *   FILE 299
//*                                                                 *   FILE 299
//*           Added an XMIT of a load library which contains the    *   FILE 299
//*           following TAPEMAP versions:   (member LOADLIB)        *   FILE 299
//*                                                                 *   FILE 299
//*           member     CBT File    Version                        *   FILE 299
//*           ------     --------    -------                        *   FILE 299
//*           TAPEMAP      299        2.5                           *   FILE 299
//*           TAPEMAPM     299        2.5.1                         *   FILE 299
//*           TAPEMAPX     804        2.5.3                         *   FILE 299
//*                                                                 *   FILE 299
//*   ---->>  *END* NEWS: ------                                    *   FILE 299
//*                                                                 *   FILE 299
//*           THIS PROGRAM WILL PROVIDE SPECIAL INFORMATION FOR     *   FILE 299
//*           TAPE FILES CREATED BY IEBCOPY, IEHMOVE, IEBISAM,      *   FILE 299
//*           IEHDASDR, OR IN SMPPTFIN FORMAT.  IN ADDITION, IF     *   FILE 299
//*           A FILE CONTAINS AN IEBUPDTE INPUT STREAM THE          *   FILE 299
//*           MEMBERS IN THE STREAM WILL BE LISTED.                 *   FILE 299
//*                                                                 *   FILE 299
//*           This program will also provide special                *   FILE 299
//*           information for CBT MVS Utilities Tapes created       *   FILE 299
//*           with CBT973.  IEBUPDTE interpretation is done for     *   FILE 299
//*           CBT973-compressed files.                              *   FILE 299
//*                                                                 *   FILE 299
//*           Also, macros in members that are themselves macro     *   FILE 299
//*           libraries (in IEBUPDTE format with ./ changed to ><)  *   FILE 299
//*           will be listed.  Thus, with this TAPEMAP you can      *   FILE 299
//*           find almost any member name on the CBT Tape.          *   FILE 299
//*                                                                 *   FILE 299
//*           See also the load module for TAPEMAP on File 035.     *   FILE 299
//*                                                                 *   FILE 299
//*    For questions, I am leaving my name for reference:           *   FILE 299
//*                                                                 *   FILE 299
//*           Sam Golob    email:  sbgolob@cbttape.org              *   FILE 299
//*                                                                 *   FILE 299
//*    PARENTHETICAL NOTE:                                          *   FILE 299
//*                                                                 *   FILE 299
//*           The old version of TAPEMAP, called TAPEMAPO, is       *   FILE 299
//*           included both on this file, and on File 035.  The     *   FILE 299
//*           newer version was revised by Ron Tansky.  Due to      *   FILE 299
//*           the tediousness of testing any new version of         *   FILE 299
//*           TAPEMAP, its old version has not been deleted,        *   FILE 299
//*           just in case the new version affects some of the      *   FILE 299
//*           code that worked before.  You can simply say EXEC     *   FILE 299
//*           PGM=TAPEMAPO, instead.  I've tested the new           *   FILE 299
//*           version, but we're trying to be on the safe side.     *   FILE 299
//*           (TAPEMAP is read-only anyway.)                        *   FILE 299
//*                                                                 *   FILE 299
//*           Peter McFarland put in a kludge into the TAPEMAP      *   FILE 299
//*           code, to get around the case when the UCB extension   *   FILE 299
//*           is above the 16M line, and the sense information      *   FILE 299
//*           has not been gotten correctly.                        *   FILE 299
//*                                                                 *   FILE 299
//*           email:  Peter_Mcfarland@adp.com                       *   FILE 299
//*                                                                 *   FILE 299
//*           Sam Golob fixed (hopefully) some of the SMPPTFIN      *   FILE 299
//*           finding code, to allow for SYSMODs starting with      *   FILE 299
//*           a ++ASSIGN card.  TAPEMAP now treats such files as    *   FILE 299
//*           being in SMPPTFIN format, and correctly displays      *   FILE 299
//*           the SYSMOD numbers in SYSPRNT2.                       *   FILE 299
//*                                                                 *   FILE 299
```
