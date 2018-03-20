# `conda` package manager

`conda` package manager is used in the [Anaconda Python Distribution](https://anaconda.org) and [Miniconda](https://conda.io/miniconda.html). Here are some commands that I use a lot.

## Exporting an environment

By default, `conda env export` will show the full path prefix of your environment, which is not portable across machines, e.g.: 

```
prefix: /home/olga/anaconda3/envs/tabula-muris-env

```

### Without a prefix

To export without a prefix use [`head`](https://stackoverflow.com/questions/13380607/how-to-use-sed-to-remove-the-last-n-lines-of-a-file) to remove the last two lines of the file (prefix plus empty line)

```
conda env export -p | head -n -2 > environment.yml
```

An excerpt of the output is below:

```yaml
  - r-zoo=1.8_0=mro343h086d26f_0
  - readline=7.0=ha6073c6_4
  - rstudio=1.1.383=h2c9cdda_2
  - scipy=1.0.0=py35hcbbe4a2_0
  - seaborn=0.8.1=py35h04cba02_0
  - send2trash=1.5.0=py35_0
```

### Without a prefix and without versions

Sometimes you don't want the versions either. Then you need to slice the file on the equals sign `=`:

```
conda env export | head -n -2 | cut -f 1 -d '=' > environment.yml
```
