---
title: Validate a simplified TC-ResNet8 model
date: 2023-12-20 10:00:00
---

## Validate a simplified TC-ResNet8 model

This model is a simplified version of TC-ResNet8 by applying Quantization-Aware Training technique, which is tested on a specofic  dataset (Lege dataset).

### Resources

source code: https://github.com/KokyoK/Lege_KWS_customize

dataset: https://drive.google.com/file/d/1qaQ8wHKT9aHe09IcOZnvDSRE34swLuX7/view?usp=sharing

### Platform Selection

I choose to train this model by using CoLab, a public server offered by Google.

Here is a [brief guidance of CoLab](colab_note.md) which is written by myself.

### Runtime Environment Configuration

Clone the source code to your Google drive.

```shell
git clone https://github.com/KokyoK/Lege_KWS_customize
```

Change direction to target folder.

There are some dependency conflicts, to solve this problem, we should not restrict to the version of torchvision package.

Then run following command to install packages.

```shell
pip3 install -r requirements.txt
```

If you meet any problem, please refer to next section.

### Possible Problems

You may get an error message when you install packages: 

```python
ERROR: Failed building wheel for pyaudio
```

To solve it, you need to install pyaudio package manually.

```shell
sudo apt-get install portaudio19-dev
pip install pyaudio
```

### Run Program

1. Create `dataset` and `saved_model `directories.
2. unzip the dataset in `dataset` folder.
3. To save the training data for evaluate the training progress, add following codes in `utility.py`.

   ```python
   import csv
   
   #write training data to csv
   data_write = [epoch, train_accuracy, train_loss, valid_accuracy,  valid_loss]
   filename = "train_data.csv"
   
   with open(filename, 'a', newline='') as file:
   	writer = csv.writer(file)
       writer.writerow(data_write)
   print(f"Data successfully written to {filename}")
   ```
4. Run following command:

   ```shell
   python3 nn_main.py
   ```

### Current Problem

The training process on GPU is quite time-comsuming, it takes approximately 2.5 hours to run 280 epochs (500 in total).

Need to check whether it really uses GPU to train, and think how to improve training efficiency.

### Result Analysis

This part will add soon.

### Reference

https://stackoverflow.com/questions/49223985/errors-installing-pyaudio-failed-building-wheel-for-pyaudio

https://github.com/ardha27/AI-Waifu-Vtuber/issues/49