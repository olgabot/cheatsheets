# Jupyter notebook

Helpful tips for jupyter notebook

## Create an environment that has jupyter inside

This is so you can install the environment kernel (the next step)

```
conda create --yes -n envname jupyter ipykernel
```

## Installing kernels from other environments

To install a kernel from another environment to your primary jupyter notebook, do this:

```
source activate myenv
python -m ipykernel install --user --name $CONDA_DEFAULT_ENV --display-name "Python 2.7 ($CONDA_DEFAULT_ENV)"
source activate other-env
python -m ipykernel install --user --name $CONDA_DEFAULT_ENV --display-name "Python 3.6 ($CONDA_DEFAULT_ENV)"
```


## When you get "IOPub Data Rate Exceeded"

Need to start the notebook with this:

```
jupyter notebook --NotebookApp.iopub_data_rate_limit=1000000000
```
