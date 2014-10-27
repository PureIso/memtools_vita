-------
Memtools Vita 0.2.1 (iffy autoresolve)
-------
Allows to play with the Vita's webkit process' memory through by leveraging a webkit vuln. Autoresolve is a little iffy, supports no special cases and skips alot of modules because it crashes (reading invalid memory)

Known issues:
Does not dump the data section, only executable code. IDA does not like that, but its enough for ROP and some reversing. To dump the data section, manually add 4k increments (4k aligned) until crash. It probably will dump more than you need, but you will definately have the data section (it is at higher addresses than module_info)
Error handling does not account for ASLR. List of dumped modules needs to be serversided and SceWebKit (and the import tree) will have to be re-resolved every time it crashes


*Install Capstone for python (disassembly library)*
[Capstone] - Capstone Download link

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

- Implement : doughnut protocol

- Implement : special-case handling for unavailable (but imported) mods (I haven't been -able to dump SceLibKernel manually, may be a bug or it may actually be impossible)


[Capstone]:http://www.capstone-engine.org/download.html