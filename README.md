# MDTTool

MDTTool is a simple multiplatform Python tool for manipulating CF card images
for the Reactive Micro MicroDrive/Turbo card for the Apple II family.

It is written in Python 3 and licenced under GPL v3.

## Usage

There are really three use cases for the tool at the current time:

  1. Display MicroDrive/Turbo partition table in human-readable format.
  2. Take a CF card image (or raw CF card device) and extract all the
     partitions as disk images.
  3. Take a list of disk images and construct a MicroDrive/Turbo disk image
     (or write directly to the raw CF card device.)

```
  mdttool [-haxlCHSG] [-o outputfile] inputfile [inputfile2...]

  -h  --help       Show this help
  -a  --assemble   Assemble CF Card image from list of .PO disk images
  -x  --explode    eXplode a CF Card image into constituent partitions
  -l  --list       Display partition table
  -C  --cylinders  Set number of cylinders (when assembling new image)
  -H  --heads      Set number of heads (when assembling new image)
  -S  --sectors    Set number of sectors (when assembling new image)
  -G  --gs         Set GS ROM version (when assembling new image)
```

## Display Partition Table

The `-l` flag is used to display the partition table embedded in a
MicroDrive/Turbo CF card image.

For example:
```
$ mdttool -l cf-card.img 

MicroDrive/Turbo Partition Table

  Cylinders:   995
  Heads:       16
  Sectors:     63
  GS ROM Vers: 0

  #    Start      End   Length
  1      256    65790    65535
  2    65791   131325    65535
  3   131326   196860    65535
  4   196861   696892   500032

```

## Explode a CF Card Image into .PO Disk Images

Each partition will be written to a separate file in the current working
directory.

By default, these images are named `partition1.po`, `partition2.po` etc.

For example:
```
$ mdttool -x cf-card.img 
Writing 33553920 bytes to partition1.po
Writing 33553920 bytes to partition2.po
Writing 33553920 bytes to partition3.po
Writing 256016384 bytes to partition4.po
```

## Assemble a CF Card Image from a list of .PO Disk Images

This command builds a MicroDrive/Turbo partition table from scratch.

The partition table embeds the physical drive parameters - the number of
cylinders, heads and sectors.  It also encodes the setting for Apple IIgs
ROM01 and ROM03.

When creating a new CF card image in this way, MDTTool will use the
hard-coded defaults for cylinders, heads, sectors and GS ROM version.  These
defaults were copied from typical 512MB CF card.

You can override these with the `-C`, `-H`, `-S` and `-G` command line
options.  The heads always seems to be set to 16 and the number of sectors
to 63.  However the number of cylinders depends on the size of the CF card
in question.

For example:
```
$ mdttool -a -o new-cf.img partition1.po partition2.po partition3.po partition4.po 
  Reading partition1.po ...
  Reading partition2.po ...
  Reading partition3.po ...
  Reading partition4.po ...
New CF card image created in new-cf.img
```

Overriding the number of cylinders is done like this:
```
$ mdttool -a -C 32000 -o new-cf.img partition1.po partition2.po partition3.po partition4.po 
  Reading partition1.po ...
  Reading partition2.po ...
  Reading partition3.po ...
  Reading partition4.po ...
New CF card image created in new-cf.img
```


