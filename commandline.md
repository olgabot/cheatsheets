# Command line cheatsheet

This is a cheat sheet for "Bourne-again shell" (bash) and GNU/Linux commands.

## Compressing and decompressing files

### Create a gzipped tarball from a `*` glob command

This will create the gzipped tarball `voyages_different_direction.tar.gz` with everything in my current folder that starts with `voyages_different_direction_`.

```
tar -zcvf voyages_different_direction.tar.gz voyages_different_direction_*
```

### Decompress a gzipped tarball

```
tar xzvf voyages_different_direction.tar.gz
```

## File management

### Soft links

Make a pretend file in a new place that jumps to the old file when you look at it.

```
ln -s oldfile newplace
```
