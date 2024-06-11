---
title: Setup
---


> ## Data
> Create a folder called data within your personal working respository
>
> Specifically, we use the following data files:

> - [US_births_1994-2003_CDC_NCHS.csv](US_births_1994-2003_CDC_NCHS.csv)
> - [US_births_2000-2014_SSA.csv](US_births_2000-2014_SSA.csv)
> - [Source](https://github.com/fivethirtyeight/data/tree/master/births)

> Please download them (by clicking on the corresponding links) and move them to the data folder
> {: .source}
{: .prereq}



## Required Python Packages

The following are packages needed for this workshop:

- [Pandas](https://pandas.pydata.org/)
- Jupyter notebook
- [Numpy](https://numpy.org/)
- [Matplotlib](https://matplotlib.org/)
- [Plotnine](https://plotnine.readthedocs.io/en/stable/)

All packages apart from `plotnine` will have automatically been installed with Anaconda
and we can use Anaconda as a package manager to install the missing `plotnine` package:
You need to open up a *Terminal*, if you are using Mac OSX, or Linux (see instructions above),
or launch an *anaconda-prompt*, if you are using Windows. In your terminal window type the following:

```bash
conda install -y -c conda-forge plotnine
```

This will then install the latest version of plotnine into your conda environment.

## Required packages: Miniconda

Miniconda is a lightweight version of Anaconda. If you install Miniconda instead of Anaconda,
you need to install required packages manually in the following way:

```bash
conda install -y numpy pandas matplotlib jupyter
conda install -c conda-forge plotnine
```

### *(Alternative)* Installing required packages with environment file

Download the
[environment.yml](../episodes/files/environment.yml)
file by right-clicking the link and selecting save as.
In the directory where you downloaded the environment.yml file run:

```bash
conda env create -f environment.yml
```

Activate the new environment with:

```bash
conda activate python-ecology-lesson
```

You can deactivate the environment with:

```bash
conda deactivate
```

## Launch a Jupyter notebook

After installing either Anaconda or Miniconda and the workshop packages,
launch a Jupyter notebook by typing this command into the *terminal* or *anaconda-prompt*:

```bash
jupyter notebook
```

The notebook should open automatically in your browser. If it does not or you
wish to use a different browser, open this link: [http://localhost:8888](https://localhost:8888).

> ## Leave terminal used to launch Jupyter open

> Jupyter depends on a server running in the background associated with the window used to launch it. Closing that window will results in web interface errors in the web interface. When done, you can either close the terminal or shut down the server using <kbd>CTRL</kbd>+<kbd>C</kbd> and submitting <kbd>y</kbd> within 5 seconds if the terminal is needed for other tasks.
> {: .source}
{: .prereq}


For a brief introduction to Jupyter Notebooks, please consult our
[Introduction to Jupyter Notebooks](jupyter_notebooks.md) page.

[python] (https://www.python.org/)
[anaconda] (https://www.anaconda.com/)
[jupyter] (https://jupyter.org/)