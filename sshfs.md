# SSHFS

`sshfs` is a way that you can view files on a server in your own finder as if they are on your computer. This is very helpful and convenient for viewing many images/plots from the output of a program, or viewing large files on the server, e.g. `bams` or `Robj`.

## Installation

On macOS, you'll need to install "OSXFuse" for low level filesystem stuff before you install SSHFS. I installed via [Homebrew](https://brew.sh/) (`brew` command):

```
brew cask install osxfuse
brew install sshfs
```

## Commands

Example command:

```
sshfs obotvinnik@tscc.sdsc.edu:/home/obotvinnik/projects/tasic2016/analysis/tasic_v1/majiq_v1 /home/Users/olga/majiq_v1
```

Broken down, the command is:

```
sshfs obotvinnik@tscc.sdsc.edu:[folder that you want from the server] [where you want to see it on your laptop]
```

## Follow symbolic links 

Normally, `sshfs` doesn't follow symbolic links To follow any symlinks (soft links) that you make. Here's the command to make it do that:

```
sshfs -o follow_symlinks obotvinnik@tscc.sdsc.edu:/oasis/tscc/scratch/obotvinnik tscc
```

## Glitches to be aware of

- Volumes may get unmounted
- Can't use it as `root`
- If you mount a volume, then switch users, the other user can still access the volume (unsafe)


Here's the command I've been using:

```
sshfs obotvinnik@tscc.sdsc.edu:/home/obotvinnik $HOME/Desktop/tscc
```

## Unmounting: 

```
umount ~/Desktop/tscc
```

or drag it to the trash
