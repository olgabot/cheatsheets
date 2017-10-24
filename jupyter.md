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
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
source activate other-env
python -m ipykernel install --user --name other-env --display-name "Python (other-env)"
```


## When you get "IOPub Data Rate Exceeded"

Need to start the notebook with this:

```
jupyter notebook --NotebookApp.iopub_data_rate_limit=1000000000
```
