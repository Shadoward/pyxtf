pyxtf
=====
* created:       May 2016
* by:            p.kennedy@fugro.com
* description:   python module to read an XTF sonar file
* notes:         See main at end of script for example how to use this
* based on XTF version 34 21/2/2012
* version 3.00
* developed for Python version 3.4 

Done
====
* added close method, but it really is not required as it will auto close when the class is out of scope.
* added support for ping weight scale factor.  This is now applied to the sonar beam data on import and it works well to compensate for gain multipliers from edgetech 
* added support for padbytes
* now reads by packet rather than record type.  This means it will skip unsupported records 
* initial implementation
* reading an XTF file follows this path:
* Open the file for binary read
* call the XTFFILEHDR class, which reads the header record of 1024 bytes
* For each channel in the file header, the XTFFILEHDR then calls the XTFCHANINFO class to read the channel header
* from there, iterate through all the ping records by calling the XTFPINGHEADER class
* For each channel, XTFCHANINFO calls the XTFPINGCHANHEADER class to read the ping channel header and data
* this code was written as an exercise in coding for python as much as anything else. I was convinced a scripting language would be too slow for industrial data processing.  
* I was very wrong.

2Do
===
* add support for additional packet types.  We need sample XTF files before bothering to implement so for now skip unsupported packets
* add support for computing cmg instead of heading data
* 

Example Reader
-------
```
    # opening the file reads the header and then the channelinfo automatically
    r = XTFReader(<filename>)
        # we can then loop through all packets, which drills down into the soanr records through the xtfchaninfo, xtfchanheader to the actual sonar data 
        while r.moreData():
            pingHdr = r.readPing()
    # close the File 
    r.close()
    # see the test code in main() at the end of pystf for more details.  Have Fun
```
```
Data Type Conversions from XTF to Python::     
    * XTF types to python struct types
    * signed char = 1 byte = "b"
    * unsigned char = 1 byte = "B"
    * XTFWORD = signed int 2 bytes = h
    * XTFWORD = UNsigned int 2 bytes = H (for unipolar data)
    * DWORD = unsigned int 4 bytes = "L"
    * short = short integer 2 bytes = "h"
    * char = 1 byte = "c"
```