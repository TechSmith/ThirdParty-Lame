How to build the binary

Note we still use Visual Studio 2008 to compile the binaries as VC9 is what got used in the source we downloaded from Lame website. This is to follow the LGPL license as we really don't want to make changes in the code to have it being able to compile in Visual Studio 2010 (VC10).

1. vc9_lame solution:
1) Open vc9_lame solution
2) Switch to "Release" (we don't use NASM and SSE2, those are for faster encoding, but we haven't tested enough on how compartible they are with different OSes, so for safety, we just use release config for now);
3) Build solution
Note app mp3x needs gtk, it fails if gtk is not installed, that's ok, because the rest of libs (which we need) compile ok. So we just removed this project from the solution.

2. vc9_lame_clients solution:
2.1 We made a few "changes" to the code:
1) Change lame_acm.xml to TSClame_acm.xml in AEncodePropeties.cpp
2) Change a few LAME to TechSmith LAME in ACM.cpp
3) If we link the lib dynamically, eg: use "$(OutDir)\libmp3lame.lib" in ACM project, the files we need to CS to work:
TSClame_acm.xml
TSClame.acm
lame_enc.dll
libmp3lame.dll
If we link statically, eg: use "$(OutDir)\libmp3lame-static.lib" and "$(OutDir)\libmpghip-static.lib" in ACM project, the files we need for CS to work:
TSClame_acm.xml
TSClame.acm
4) DShow project also has the option of linking dynamically or statically:
Dynamically use:
$(OutDir)\libmp3lame.lib
Statically use:
$(OutDir)\libmp3lame-static.lib
$(OutDir)\libmpghip-static.lib 

5) lame_test project missed file lame_test.cpp (the orignal zip from Lame website missed this file, we can't do nothing about it, so we just removed this project from the solution.

6) Removed libmp3lame project from this solution as we got it already in vc9_lame solution

7) Make sure we use the same GUID in uids.h as in CS solution's TSCGUIDs.h for CLSID_LAMEDShowFilter. eg we use this one in both Camtasia.sln and vc9_lame_clients.sln:
// {2477BBD1-8D11-4c61-9FA3-8A9E371AF926}
DEFINE_GUID(CLSID_LAMEDShowFilter, 
            0x2477bbd1, 0x8d11, 0x4c61, 0x9f, 0xa3, 0x8a, 0x9e, 0x37, 0x1a, 0xf9, 0x26);


2.2 Compile:
1) Open vc9_lame solution_client solution
2) Switch to "Release" (we don't use NASM and SSE2, those are for faster encoding, but we haven't tested enough on how compartible they are with different OSes, so for safety, we just use release config for now);
3) Build the solution (we actually only need to compile the ACM project if link libs statically).

3. Copy final files to redist folder
Copy files below from $(OutDir) folder (eg: \output\debug or \output\release) to redist folder, so CS installer script can grab them there.

Files:
lame_dshow.ax
TSClame.ac
TSClame_acm.xml