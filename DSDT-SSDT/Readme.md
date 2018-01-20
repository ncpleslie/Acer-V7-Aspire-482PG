Pre-patched DSDT/SSDT are avaliable but I'd recommend patching your own. If using Clover, then push ```F4``` to dump DSDT/SSDT into EFI/Clover/ACPI/Origin
Place all patched DSDT/SSDTs into the EFI/Clover/ACPI/patched folder

**Patches applied**

**DSDT**

* Fix _WAK Arg0 v2
* HPET Fix
* SMBUS Fix
* IRQ Fix
* RTC Fix
* OS Check Fix (Windows 8)
* Fix Mutex with non-zero SyncLevel
* Rename GFX0 to IGPU
* 7-Series/8-Series USB
* USB3 _PRW 0x6D
* Audio Layout 03
* Shutdown fix
```
#Shutdown Fix
into method label _PTS code_regex (If\s*\(LEqual\s*\(Arg0,\s*0x05\)\)\s*\n\s*\{\s*\n)(?:[^\n\}]+\n)+(\s*\}) replace_matched begin
%1
            Store (Zero, SLPE)\n
            Sleep (0x10)\n
%2
end;
into definitionblock code_regex . code_regex_not OperationRegion\s*\(PMRS insert begin
OperationRegion (PMRS, SystemIO, 0x1830, One)\n
Field (PMRS, ByteAcc, NoLock, Preserve)\n
{\n
        ,   4, \n
    SLPE,   1\n
}
end;
```
* Brightness Keys
```
# Make EC-based brightness up/down work with RehabMan VoodooPS2 ACPI keyboard mechanism
into method label _Q8F replace_content
begin
// Brightness Down\n
    Notify(\_SB.PCI0.LPCB.PS2K, 0x0405)\n
end;
into method label _Q8E replace_content
begin
// Brightness Up\n
    Notify(\_SB.PCI0.LPCB.PS2K, 0x0406)\n
end;
```

Disable Nividia

Add the following lines to the top of the DST
```
External (_SB_.PCI0.RP05.PEGP._OFF, MethodObj) // Warning: Unresolved Method, guessing 0 arguments (may be incorrect, see warning above)
External (_SB_.PCI0.RP05.PEGP._ON, MethodObj) // Warning: Unresolved Method, guessing 0 arguments (may be incorrect, see warning above)
```

* Add ```\_SB.PCI0.RP05.PEGP._ON()``` at the beginning of _PTS method
* Add ```\_SB.PCI0.RP05.PEGP._OFF()``` at the end of _WAK method, but before Return statement
* Add ```\_SB.PCI0.RP05.PEGP._OFF()``` at the end of SB.PCI0._INI method



**SSDT**

