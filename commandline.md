# Command line cheatsheet

This is a cheat sheet for "Bourne-again shell" (bash) and GNU/Linux commands.

## If you're prompted for a password with the ucsd-train account

Then the ssh key needs to be added again.

```
ssh-add ~/.ssh/biom_262.pri
```


## Text maninpulation

### Skip the first line of a file

```
tail -n +2 filename.txt
```

## Compressing and decompressing files

### Create a gzipped tarball from a `*` glob command

This will create the gzipped tarball `voyages_different_direction.tar.gz` with everything in my current folder that starts with `voyages_different_direction_`.

* `z` is for "g***z***ip"
* `c` is for "***c***ompress"
* `v` is for "***v***erbose." Show all the files that are getting compressed as it's happening. (Good for debugging)
* `f` is for "***f***ilename." Must be the last flag because the filename is inferred after it

```
tar -zcvf voyages_different_direction.tar.gz voyages_different_direction_*
```

### Decompress a gzipped tarball

The `-` is optional, can also say `tar -xzvf`

* `x` is for "e***x***tract"
* `z` is for "g***z***ip"
* `v` is for "***v***erbose." Show all the files that are getting extracted as it's happening. (Good for debugging)
* `f` is for "***f***ilename." Must be the last flag because the filename is inferred after it

```
tar xzvf voyages_different_direction.tar.gz
```

### Create a zip file

Create a zip file from multiple files

```
zip squash.zip file1 file2 file3
```


## File management

### Soft links

Make a pretend file in a new place that jumps to the old file when you look at it.

```
ln -s oldfile newplace
```

### Hard links

Make a new pointer (aka `inode`) to the same data in the filesystem. If there are multiple pointers to the data, the data is not truly "erased" until the last pointer is removed (i.e. the `inode` number is decremented to 0)

```
ln oldfile newplace
```

### Opening files containing and not containing certain text

Open all files that match this glob command, except if they have `stream` in them.

```
ls -1 exon_3p_exonbody_tier1_*/homerResults.html | grep -v stream | xargs open
```

## Installing programs 

### Python

To install a python program, usually you can use `pip`. I like the `-e` flag, which installs it in "developer mode" because any change in the files located there is also reflected in the imported code.

```
cd python_program/
pip install -e .
```

### From source

To install a program from source on a shared cluster, you will often need to specify the directory you want to install things. That can be done with `--prefix` at the `./configure` step. The directory is where you want all the `bin/`, `lib/` and so on files to be added to (default is `/usr/local`) Then, do `make` and `make install`. For example:

```
./configure --prefix=/projects/ps-yeolab/software
make && make install   # "make install" will run only if "make" is successful
```

## Connecting to other servers: `ssh`

Appending a file to something on a server, e.g. to send your ssh public key onto the list of authorized keys, i.e. "`cat`-ing something over `ssh`"

```
cat .ssh/id_dsa.pub | ssh obotvinnik@tscc.sdsc.edu 'cat >> ~/.ssh/authorized_keys'
```

## `xargs`

`xargs` is really powerful but I don't understand it and cringe when I have to use it. Here's an example where `xargs` worked:

```
ls -Str ~/googledrive/MACA/384W_QC/plate_reader/raw_plate_reader_output/3_Month/cherrypicked/*.csv | xargs dobby aggregate  --desired-concentration 0.3  --output-folder ~/googledrive/MACA/cDNA\ Pick\ Lists/3_month/
```

## Replace `^M` characters with newline for `less`

From https://stackoverflow.com/questions/19406418/remove-m-characters-from-file-using-sed

```
(immune-evolution)
 Thu 22 Apr - 07:56  /mnt/data_sm/olga/nextflow-kmermaid-botryllus-mhc-april2021/c9/f337129a87448c9685b18bb21e9416 
 olga@lrrr  tr '^M' '\n' <.command.err | less -S
```

Example output:

```
(immune-evolution)
 Thu 22 Apr - 07:57  /mnt/data_sm/olga/nextflow-kmermaid-botryllus-mhc-april2021/c9/f337129a87448c9685b18bb21e9416 
 olga@lrrr  tr '^M' '\n' <.command.err | tail

...loading from '.' / 280890 sigs total

...loading from '.' / 280900 sigs total

...loading from '.' / 280910 sigs total

...loading from '.' / 280920 sigs total

...loading from '.' / 280930 sigs total%
```

## GNU Parallel

A file of commands can be run with GNU parallel with:

```
parallel --progress --eta --jobs 96 < /home/olga/data_sm/immune-evolution/kmer-signatures/2--test-human/6--gather-adar/gather_adar_commands.txt
```

## Run `make` in parallel

E.g. if there are multiple targets, you can run make in parallel on all of them with `--jobs`

```bash
make --jobs 32
```

