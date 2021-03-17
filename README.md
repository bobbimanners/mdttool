# MDTTool

MDTTool is a simple multiplatform Python tool for manipulating CF card images
for the Reactive Micro MicroDrive/Turbo card for the Apple II family.

It is written in Python 3 and licenced under GPL v3.

Usage:
  mdttool [-haxlCHSG] [-o outputfile] inputfile [inputfile2...]

  -h  Show this help
  -a  Assemble CF Card image from list of .PO disk images
  -x  eXplode a CF Card image into constituent partitions
  -l  Display partition table
  -C  Set number of cylinders (when assembling new image)
  -H  Set number of heads (when assembling new image)
  -S  Set number of sectors (when assembling new image)
  -G  Set GS ROM version (when assembling new image)

## Display Partition Table

For example:
```
mdttool -l cf-card.img
```

## Explode a CF Card Image into .PO Disk Images

For example:
```
mdttool -x cf-card.img
```

## Assemble a CF Card Image from a list of .PO Disk Images

For example:
```
mdttool -a -o new-cf.img part1.po part2.po part3.po
```

