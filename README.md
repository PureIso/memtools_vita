-------
Memtools Vita 0.3.2
-------
Allows developers to play with the Vita's WebKit process memory by leveraging a WebKit vuln. Autoresolve is untested but should now handle kernel modules (patched syscalls) correctly.

Known issues:
Does not dump the data section, only executable code. IDA does not like that, but its enough for ROP and some reversing. To dump the data section, manually add 4k increments (4k aligned) until crash. It probably will dump more than you need, but you will definitely have the data section (it is at higher addresses than module_info)
Error handling does not account for ASLR. List of dumped modules needs to be serversided and SceWebKit (and the import tree) will have to be re-resolved every time it crashes


*Install Capstone for python (disassembly library)*
   - Install the core of Capstone: https://github.com/aquynh/capstone/blob/next/COMPILE.TXT
   - Install Python binding for Capstone: https://github.com/aquynh/capstone/blob/next/bindings/python/README

To use, first start the server:
```
    chmod a+x serv.py
    ./serv.py
```
Then with the Vita browse to `http://<ipaddr>:8888`.
If all goes well you will see some output from the `serv.py` script. 
When you see `%> ` it means that initialization is done.
The supported commands are:
- **autodump**  : use to begin recursively resolving the modules
- **savemods** : !unimplemented! save modules to disk (stored in "dump" folder and named as <modname>.bin)
- **x** `addr` `len` : to display `len` bytes from `addr` in a hex-editor-like fashion
- **dis** `addr` `len` `mode` : to disassemble `len` bytes from `addr` in `mode` (thumb or arm, latter is default)
- **dump** `addr` `len` `fname` : to dump `len` bytes from `addr` to `fname`
- **ss** `begaddr` `endaddr` `pattern`: to search for the string `pattern` in [`begaddr`, `endaddr`[
- **reload** : to reload/reset everything
- **exit** : to exit

-----
Manually Dumping
-----
Once you resolve a module and get module_info, you will need to look at the module_info to get stub_end. Dump from base of SceWebkit (sce_module.base) to stub end. The dump code appends to existing files with the same name, so delete the old ones if you are redumping for whatever reason. resolve.js implements the memory parsing for resolving a module, give it a look for help.

-----
TODO
-----

- Implement : special-case handling for offsize import list entries

- Implement : List of resolved modules serverside to prevent modules which are imported by more than one module from being dumped multiple times.

-----
Contributors
-----
- [CodeLion](https://twitter.com/bballing1): PoC using bug with netcat backend, and this new composite of code below:
- [hgoel0974](https://twitter.com/hgoel0974): contributions to above netcat streaming dump PoC
- [Josh_Axey](https://twitter.com/josh_axey): cleaner PoC using bug with python backend
- [Archaemic](https://twitter.com/Archaemic): even cleaner PoC using bug with even better python backend (and before everyone else)
- "a good friend": major refactor combining all three PoCs and ground up replacement of python backend with far superior python backend

-----
Github Contributers
-----
- [MrNetrix](https://github.com/MrNetrix) : implemented reverse search and hex search
- [Aquynh](https://github.com/aquynh) : Changed install instructions for capstone and linked to project
- [PureIso](https://github.com/PureIso): fixed directory existence check in dump code, added backup of old dumps and creation of new unique file if dump exists
- [Spectenoir06](https://github.com/spectrenoir06) : Added output of the server IP on load

-----
Testers
-----
- [BigBoss](https://twitter.com/psxdev)
- [Alin](https://twitter.com/Logomorph)
- [Kamil Maciejewski](https://twitter.com/Macia10)
- [SMOKE](https://twitter.com/SMOKE587)

-----
Hints & Help
-----
- [Yifan](https://twitter.com/yifanlu)
- [Davee](https://twitter.com/daveeftw)
