# Weak deep priors for seismic imaging

Codes for generating results in Siahkoohi, A., Rizzuti, G., and Herrmann, F.J., Weak deep priors for seismic imaging. arXiv preprint [arXiv:2004.06835](https://arxiv.org/abs/2004.06835).

## Prerequisites

This code has been tested on Deep Learning AMI (Amazon Linux 2) Version 26.0 on Amazon Web Services (AWS), using `c5.4xlarge` and `g4dn.xlarge` instances. Using GPU is not essential since PDE solves dominate the computation. Also, we use GCC compiler version 7.3.1.

This software is based on [Devito-3.5](https://github.com/devitocodes/devito/releases/tag/v3.5) and [PyTorch-1.4.0](https://github.com/pytorch/pytorch/releases/tag/v1.4.0). Additionally, we borrow `JAcoustic_codegen.py`\, `PyModel.py`\, `PySource.py`\, `utils.py`\, and `checkpoint.py` from [JUDI](https://github.com/slimgroup/JUDI.jl), a framework for large-scale seismic modeling and inversion that abstracts forward/adjoint nonlinear and Born modeling Devito operators.

Follow the steps below to install the necessary libraries:

```bash
cd $HOME
git clone https://github.com/slimgroup/Software.SEG2020.git
git clone --branch v3.5 https://github.com/devitocodes/devito.git

cd $HOME/devito
conda env create -f environment.yml
source activate devito
pip install -e .
export DEVITO_ARCH=gnu
export OMP_NUM_THREADS=8 # or any other number of threads you prefer
export DEVITO_OPENMP=1

conda install pytorch torchvision cudatoolkit=10.1 -c pytorch
pip install matplotlib
pip install tqdm
pip install h5py
pip install tensorboardX
```

## Dataset


We use a 2D subset of the real Kirchoff time migrated [Parihaka-3D](https://wiki.seg.org/wiki/Parihaka-3D) dataset released by the New Zealand government, placed at `vel_dir/`. The observed data will be also downloaded into `vel_dir/data/` upon starting the inversion. See below for more details.

## Script descriptions

`run_weak_prior.sh`\: script for running inversion/training w/ weak deep prior method. It  creates `checkpoint/`, `log/`, and `sample/` directories in `$HOME/Software.SEG2020/siahkoohi2020SEGwdp/` for storing intermediate parameters, loss function log, and samples for monitoring, respectively. The drawn samples from the posterior will be stored at `training-logs.pt` located at `checkpoint/`.

`run_deep_prior.sh`\: script for running inversion/training w/ deep prior method. It  creates `checkpoint/`, `log/`, and `sample/` directories in `$HOME/Software.SEG2020/siahkoohi2020SEGwdp/` for storing intermediate parameters, loss function log, and samples for monitoring, respectively. The drawn samples from the posterior will be stored at `training-logs.pt` located at `checkpoint/`.

`run_MLE.sh`\: script for running MLE-based inversion/training. It  creates `checkpoint/`, `log/`, and `sample/` directories in `$HOME/Software.SEG2020/siahkoohi2020SEGwdp/` for storing intermediate parameters, loss function log, and samples for monitoring, respectively. The final and intermediate inversion results and training loss function logs will be stored at `samples.hdf5` and `training-logs.pt`, respectively, located at `checkpoint/`.

`src/main.py`\: constructs `LearnedImaging` class using given arguments and calls `train` function in the defined  `LearnedImaging` class.

`src/model.py`: includes `LearnedImaging` class definition, which involves `train` and `test` functions.

`src/sample.py`: script for loading the obtained estimations after (while) training and creating figures in the manuscript. Not that the script will throw an assertation error if there are no estimations.

### Running the code

To perform inversion/training, w/ weak deep prior, run:

```bash

bash run_weak_prior.sh

```

To perform inversion/training, w/ deep prior, run:

```bash

bash run_deep_prior.sh

```


To perform MLE-baed inversion/training, run:

```bash

bash run_MLE.sh

```

To generate and save figures shown in the manuscript, run `sample.py` with appropriate arguments. The figures will be saved in `sample/` directory.

## Citation

If you find this software useful in your research, please cite:


```bibtex
@conference {siahkoohi2020SEGwdp,
    title = {Weak deep priors for seismic imaging},
    booktitle = {SEG Technical Program Expanded Abstracts},
    year = {2020},
    month = {09},
    pages = {2998--3002},
    doi = {10.1190/segam2020-3417568.1},
    software = {https://github.com/slimgroup/Software.SEG2020},
    author = {Ali Siahkoohi and Gabrio Rizzuti and Felix J. Herrmann},
    url = {https://arxiv.org/pdf/2004.06835.pdf}
}
```


## Questions

Please contact alisk@gatech.edu for further questions.

## Acknowledgments

The authors thank Zezhou Cheng for his open-access [GitHub repository](https://github.com/ZezhouCheng/GP-DIP). We also thank Philipp Witte for his contributions in integrating Devito operators in PyTorch.


## Author

Ali Siahkoohi

