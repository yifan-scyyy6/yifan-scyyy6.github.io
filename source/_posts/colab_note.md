---
title: CoLab Learning Note
date: 2023-12-20 10:00:00
---

## CoLab Learning Note

### Recommended Tutorials

Here are some recommended tutorials, you can easily get started with them.

In English:

- Google:

  https://colab.research.google.com/drive/16pBJQePbqkz3QFV54L4NIkOn1kwpuRrj

- Tutorials Point:

  https://www.tutorialspoint.com/google_colab/index.htm

In Chinese:

- zhihu

  https://zhuanlan.zhihu.com/p/527663163

### Common Usages

#### Connect to google drive

```python
from google.colab import drive
drive.mount('/content/drive')
```

#### Change direction

Sometimes will have problems with `cd`, use following commands to instead.

```python
import os
path = "/content/drive/MyDrive/target_folder_name"
os.chdir(path)
```

#### Check GPU state

Use following script to print info of GPU.

```python
gpu_info = !nvidia-smi
gpu_info = '\n'.join(gpu_info)

if gpu_info.find('failed') >= 0:
  print('Not connected to a GPU')
else:
  print(gpu_info)
```

Use following script to check whether GPU is available for your model.

And show the device type of GPU.

```python
import torch
print(torch.cuda.is_available()) #True if Torch is using GPU, otherwise False.
print(torch.cuda.get_device_name(0)) #Show the device name.
```

