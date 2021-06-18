# Install

## Pre-requisites


__Anaconda python__: Install the [anaconda python distribution](https://docs.anaconda.com/anaconda/install/) (or [miniconda](https://docs.conda.io/en/latest/miniconda.html)). If conda is already installed on your system, make sure you have conda v4.8.4+. If not, update from an older version with `conda update conda`.


<!-- ```shell
curl https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o miniconda.sh
sh miniconda.sh -b -p $HOME/miniconda
export PATH="$HOME/miniconda/bin:$PATH"
``` -->

__CUDA libraries for using the GPU__: While _DAS_ works well for annotating song using CPUs, GPUs will greatly improve annotation speed and are highly recommended for training. _DAS_ uses Tensorflow as a  deep-learning backend. To ensure that Tensorflow can utilize the GPU, the required CUDA libraries need to be installed. See the [tensorflow docs](https://www.tensorflow.org/install/gpu) for details.

__Libsoundfile on linux__: If you are on linux and want to load audio from a wide range of audio formats (other than wav), then you need to install `libsndfile`. The GUI uses the [soundfile](http://pysoundfile.readthedocs.io/) python package, which relies on `libsndfile`. `libsndfile` will be automatically installed on Windows and macOS. On Linux, the library needs to be installed manually with: `sudo apt-get install libsndfile1`. Again, this is only required if you need to load data from more exotic audio files.

__Visual C++ runtime on windows__: This is typically installed so only required if das fails to load the native tensorflow runtime. Download the latest version from [here](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads).

## Install _DAS_ with or without the GUI
Create an anaconda environment called `das` that contains all the required packages, including the GUI:
```shell
conda env create -f https://raw.githubusercontent.com/janclemenslab/das/master/env/das_gui.yml -n das
```

If you do not need the graphical user interface (for instance, when training _DAS_ on a server), install the non-GUI version:
```shell
conda env create -f https://raw.githubusercontent.com/janclemenslab/das/master/env/das_plain.yml -n das
```

## Update
Don't. It only causes problems in our experience. Best to install into a fresh environment than to update an existing environment. The brave can update using pip:
```shell
conda activate das
pip install das --update  # DAS itself
pip install xarray_behave --update  # the GUI
```

## Test the installation (Optional)
To quickly test the installation, run these  commands in the terminal:
```shell
conda activate das  # activate the conda environment
das train --help  # test das training
das gui  # start the GUI
```
The second command will display the command line arguments for `das train`. The last command, `das gui`, will start the graphical user interface - this step will *not* work with the non-GUI install.

## Make a clickable desktop icon (Optional)
To start the _DAS_ GUI without having to use a terminal, create a clickable startup script on the desktop.

On macOS or linux, place a text file called `das.sh` (linux) or `das.command` (macOS) with the following content on the desktop:
```shell
# /bin/bash
source $CONDA_PREFIX/etc/profile.d/conda.sh
conda activate das
das gui
```
Make the files executable with `chmod +x FILENAME`, where FILENAME is `das.sh` (linux) or `das.command` (macOS).

For windows, place a text file called `das.bat` with the following content on the desktop:
```shell
TITLE DAS
CALL conda.bat activate das
das gui
```

## Next steps
If all is working, you can now use _DAS_ to annotate song. To get started, you will first need to train a network on your own data. For that you need annotated audio - either create new annotations [using the GUI](/tutorials_gui/tutorials_gui) or convert existing annotations [using python scripts](/tutorials/tutorials).