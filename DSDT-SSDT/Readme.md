Pre-patched DSDT/SSDT are avaliable but I'd recommend patching your own. If using Clover, then push ```F4``` to dump DSDT/SSDT into EFI/Clover/ACPI/Origin
Place all patched DSDT/SSDTs into the EFI/Clover/ACPI/patched folder

**Patches applied**

**DSDT**

* Rename _DSM methods to XDSM
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



**About the SSDTs**

```
SSDT-0 = This SSDT is for CPU (CPU Scopes) - Ignore. We will create our own
SSDT-1 = This SSDT is for CPU (CPU Scopes) - Ignore. We will create our own
SSDT-2 = This SSDT is for the Internal Graphics Card (\_SB.PCI0, B0D3, GFX0)
SSDT-3 = This SSDT is for the Nvidia Graphics Card (\_SB.PCI0)
SSDT-4 = This SSDT is unknown, but is needed (\_SB and IAOE)
```

* Apply ```Rename _DSM methods to XDSM``` to all SSDTs

**SSDT-2**

* Apply Rename GFX0 to IGPU patch
* Haswell HD4400 Patch
            ```However replace 0x06, 0x00, 0x26, 0x0a to 0x0C, 0x00, 0x16, 0x0A in the patch, which more compatible with HD4400```


**SSDT-3**

* Apply Rename GFX0 to IGPU

**SSDT-4**

* Apply Rename GFX0 to IGPU

**SSDT-0 and SSDT-1**

We will generate these using [https://github.com/Piker-Alpha/ssdtPRGen.sh]

```curl -o ssdtPRGen.sh https://raw.githubusercontent.com/Piker-Alpha/ssdtPRGen.sh/Beta/ssdtPRGen.sh
chmod +x ssdtPRGen.sh
./ssdtPRGen.sh -x 1 -p 'i5-4200U'
```
```
ASL Input:     /Users/USER/Library/ssdtPRGen/ssdt.dsl - 264 lines, 7903 bytes, 49 keywords
AML Output:    /Users/USER/Library/ssdtPRGen/ssdt.aml - 1619 bytes, 16 named objects, 33 executable opcodes
```



