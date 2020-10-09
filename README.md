# U-Sleep

This repository stores code for training and evaluating the U-Sleep sleep staging model. The U-Sleep repository builds upon and significantly extends our [U-Time](https://github.com/perslev/U-Time) repository, published in [[1]](#u-time). In the following, we will use the term *U-Sleep* to denote the recilient high frequency sleep staging model, and *u-time* to denote this repository of code used to train and evaluate U-Sleep.

## Contents

- [Overview](#overview)
- [System Requirements](#system-requirements)
- [Installation Guide](#installation-guide)
- [Demo](#demo)
- [Full Reproduction of U-Sleep](#full-reproduction-of-u-sleep)
- [U-Time and U-Sleep Citations](#citations)


## Overview
## System Requirements
## Installation Guide
On a Linux machine with at least 1 CUDA enabled GPU available and `Anaconda` installed, run the following command to create your `u-sleep` environment:

```
conda env create --file environment.yaml
```

This instllation process may take up to 10 minutes to complete.

## Demo
In this following we will demonstrate how to launch a short training session of U-Sleep on a significantly limited subset of the datasets used in [[2]](#U-Sleep).

## Full Reproduction of U-Sleep


## TLDR: An end-to-end example
<pre>
<b># Clone repo and install</b>
git clone https://github.com/perslev/U-Time
pip3 install -e U-Time

<b># Obtain a public sleep staging dataset</b>
ut fetch --dataset sleep-EDF-153 --out_dir datasets/sleep-EDF-153

<b># Prepare a fixed-split experiment</b>
ut cv_split --data_dir 'datasets/sleep-EDF-153' \
            --subject_dir_pattern 'SC*' \
            --CV 1 \
            --validation_fraction 0.20 \
            --test_fraction 0.20 \
            --common_prefix_length 5 \
            --file_list

<b># Initialize a U-Time project</b>
ut init --name my_utime_project \
        --model utime \
        --data_dir datasets/sleep-EDF-153/views/fixed_split

<b># Start training</b>
cd my_utime_project
ut train --num_GPUs=1 --channels 'EEG Fpz-Cz'

<b># Predict and evaluate</b>
ut evaluate --out_dir eval --one_shot

<b># Print a confusion matrix</b>
ut cm --true 'eval/test_data/dataset_1/files/*/true.npz' \
      --pred 'eval/test_data/dataset_1/files/*/pred.npz'
      
<b># Print per-subject summary statistics</b>
ut summary --csv_pattern 'eval/test_data/*/evaluation_dice.csv' \
           --print_all

<b># Output sleep stages for every 3 seconds of 128 Hz signal </b>
<b># Here, the 'folder_regex' matches 2 files in the dataset </b>
ut predict --folder_regex '../datasets/sleep-EDF-153/SC400[1-2]E0' \
           --out_dir high_res_pred \
           --data_per_prediction 384 \
           --one_shot
</pre>


## Citations

If you found U-Time and/or U-Sleep useful in your scientific study, please consider citing the paper(s):

#### [1] U-Time

```
@incollection{NIPS2019_8692,
	title = {U-Time: A Fully Convolutional Network for Time Series Segmentation Applied to Sleep Staging},
	author = {Perslev, Mathias and Jensen, Michael and Darkner, Sune and Jennum, Poul J\o rgen and Igel, Christian},
	booktitle = {Advances in Neural Information Processing Systems 32},
	editor = {H. Wallach and H. Larochelle and A. Beygelzimer and F. d\textquotesingle Alch\'{e}-Buc and E. Fox and R. Garnett},
	pages = {4415--4426},
	year = {2019},
	publisher = {Curran Associates, Inc.},
	url = {http://papers.nips.cc/paper/8692-u-time-a-fully-convolutional-network-for-time-series-segmentation-applied-to-sleep-staging.pdf}
}
```

#### [2] U-Sleep

```
[IN REVIEW]
```
