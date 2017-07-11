# Jupyter notebook

Helpful tips for jupyter notebook

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
