# CBT897
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 897 is from John McKown and is a port of SQLITE 3.8       *   FILE 897
//*           to z/OS.  The current state of this package is        *   FILE 897
//*           described below.  It is hoped that any improved       *   FILE 897
//*           versions of this package will be posted here soon.    *   FILE 897
//*                                                                 *   FILE 897
//*           email:  john.archie.mckown@gmail.com                  *   FILE 897
//*                                                                 *   FILE 897
//*           Please look at the description below:                 *   FILE 897
//*                                                                 *   FILE 897
//*     SQLITE 3.8 for z/OS                                         *   FILE 897
//*     ===================                                         *   FILE 897
//*                                                                 *   FILE 897
//*     Sqlite is a self-contained, server-less,                    *   FILE 897
//*     zero-configuration, transactional SQL database engine.      *   FILE 897
//*     This is the standard sqlite library which is available      *   FILE 897
//*     on many UNIX and Linux systems. The code was compiled       *   FILE 897
//*     with almost no changes. The code is dependent on z/OS       *   FILE 897
//*     UNIX System Services.  The original code supports           *   FILE 897
//*     EBCDIC in addition to the normal ASCII. This                *   FILE 897
//*     distribution has been compiled to support EBCDIC            *   FILE 897
//*     characters and IEEE (BFP) floating point numbers. This      *   FILE 897
//*     latter is important because most other z/OS languages       *   FILE 897
//*     use the historical HFP floating point.                      *   FILE 897
//*                                                                 *   FILE 897
//*     At present, the code only has C language bindings on        *   FILE 897
//*     z/OS. Work is being done on writing bindings for            *   FILE 897
//*     Enterprise COBOL and Enterprise PL/I. The COBOL and PL/I    *   FILE 897
//*     code in this library is not even alpha code. It is just     *   FILE 897
//*     what I currently am working on.                             *   FILE 897
//*                                                                 *   FILE 897
//*     Sqlite 3.17.7 is documented at http://sqlite.org, and       *   FILE 897
//*     this code runs as described there. Therefore, no futher     *   FILE 897
//*     documentation is supplied at present.  When the COBOL       *   FILE 897
//*     and PL/I bindings are done, those will be documented        *   FILE 897
//*     here.                                                       *   FILE 897
//*                                                                 *   FILE 897
//*     What still needs to be done:                                *   FILE 897
//*     ----------------------------                                *   FILE 897
//*     1. Implement an easy to use COBOL callable API.             *   FILE 897
//*        Advanced COBOL programmers may be able to directly       *   FILE 897
//*        use the C language bindings, but these are very          *   FILE 897
//*        difficult in COBOL due to the extremely different        *   FILE 897
//*        calling sequences.                                       *   FILE 897
//*                                                                 *   FILE 897
//*     2. As above, implement a PL/I callable API.                 *   FILE 897
//*                                                                 *   FILE 897
//*     3. At present, the file which contains the sqlite           *   FILE 897
//*        database must reside in a UNIX subdirectory. This        *   FILE 897
//*        means that the user of sqlite must have an z/OS UNIX     *   FILE 897
//*        identity.  I would like to be able to use a VSAM         *   FILE 897
//*        Linear Dataset for storing the sqlite data at some       *   FILE 897
//*        time. Mainly due to the number of shops which have       *   FILE 897
//*        not really embraced z/OS UNIX.                           *   FILE 897
//*                                                                 *   FILE 897
//*     Members in this library:                                    *   FILE 897
//*     ------------------------                                    *   FILE 897
//*     - $README  - This member. The README in markdown format.    *   FILE 897
//*                                                                 *   FILE 897
//*     - LINK     - The JCL to link the SQLITE3 object code        *   FILE 897
//*                  into a LINKLIB.  This composite links in       *   FILE 897
//*                  the C and LE library subroutines.              *   FILE 897
//*                                                                 *   FILE 897
//*     - PAXFULL  - This is a compressed pax archive for the       *   FILE 897
//*                  entire SQLITE3 application, Including all      *   FILE 897
//*                  source code and make information.              *   FILE 897
//*                                                                 *   FILE 897
//*     - PAXRUN   - This is a compressed pax archive containing    *   FILE 897
//*                  only the files needed to use SQLITE. This      *   FILE 897
//*                  is really all you need if you want to          *   FILE 897
//*                  develop C language programs which use          *   FILE 897
//*                  SQLITE.  This is not needed for COBOL or       *   FILE 897
//*                  HLASM programs.                                *   FILE 897
//*                                                                 *   FILE 897
//*     - SQLITE3A - LE enabled HLASM subroutine which presents     *   FILE 897
//*                  an API to the SQLITE3 C subroutines which      *   FILE 897
//*                  is designed for use by COBOL or PL/I code.     *   FILE 897
//*                  It is composite (statically) linked with       *   FILE 897
//*                  the C object code.  This code has not been     *   FILE 897
//*                  fully tested yet and may contain errors.       *   FILE 897
//*                                                                 *   FILE 897
//*     - SQLITE3O - The object code for SQLITE to be linked        *   FILE 897
//*                  into the application.  This was compiled on    *   FILE 897
//*                  z/OS 1.13, but the C compiler options were     *   FILE 897
//*                  for compatibility with z/OS 1.11 or higher.    *   FILE 897
//*                                                                 *   FILE 897
//*     - TESTCOB1 - Example Enterprise COBOL program. It is        *   FILE 897
//*                  very basic.  It uses the SQLITE3A stub to      *   FILE 897
//*                  invoke SQLITE3 operations.  This is not        *   FILE 897
//*                  working at present.                            *   FILE 897
//*                                                                 *   FILE 897
//*     - TESTCOB2 - Example Enterprise COBOL program. It is        *   FILE 897
//*                  very basic.  It directly calls the C           *   FILE 897
//*                  language SQLITE3 subroutines.  This is not     *   FILE 897
//*                  working at present.                            *   FILE 897
//*                                                                 *   FILE 897
//*     - TSTPLI1  - Example Enterprise PL/I program. It is very    *   FILE 897
//*                  basic.  This is not working at present.        *   FILE 897
//*                                                                 *   FILE 897
//*     - UNPAX    - The JCL needed to unwind either the PAXFULL    *   FILE 897
//*                  or PAXRUN member into a z/OS UNIX              *   FILE 897
//*                  subdirectory.                                  *   FILE 897
//*                                                                 *   FILE 897
//*     Note: This member is in "markdown" compatible format.       *   FILE 897
//*     For more information on markdown, go to                     *   FILE 897
//*     http://daringfireball.net/projects/markdown/                *   FILE 897
//*                                                                 *   FILE 897
```
