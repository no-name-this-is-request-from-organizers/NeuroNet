# NeuroNet

A. Настройка среды для обучения нейросети

Pre-trained datasets: https://github.com/CMU-CREATE-Lab/deep-smoke-machine/tree/master/back-end/data/pretrained_models

**Open-source published under terms of BSD-3-Clause License**

---

A.1. How to request GPU quota increase in Google Cloud - 

1. Go to quotas page.
1. In Metrics column, deselect all and select the one with GPU.
1. Select the region for the GPU.
1. Click on EDIT QUOTAS and then fill the form that pops up on the right-hand side.
1. Click on Submit request.

> Compute Engine API; Quota: GPUs; Enter a new quota limit: 1

**2xCPU Intel Haswell + 1xGPU NVIDIA Tesla K80 / RAM 7.5 GB  / SSD 240 GB\
Debian GNU/Linux 10\
PyTorch 1.10 (CUDA 11.0)**

A.2* Google Colab trying

https://colab.research.google.com/drive/

A.3. New quota has been accepted by Google support,

https://console.cloud.google.com/iam-admin/quotas

Compute Engine API : 1 x GPU NVIDIA Tesla K80

B. Использование нейросети I3D, натренированной на базе датасетов UCF-101 и HMDB-51 для обнаружения дыма от пожаров

B.0. Копирование локальных файлов на VM

Downloading The Cloud SDK (gcloud), https://cloud.google.com/sdk/docs/install?hl=en_US

```$ gcloud compute scp --project PROJECT_ID --zone VM_ZONE --recurse <local file or directory> fg-vm:~/```

Заранее подготовили файлы к обработке:

```
$ find . -type f -name '*.jpg' | xargs -n1 -I{} rm -f {}
$ ls -1 *.{MTS,avi} | xargs -n1 -I{} ffmpeg -i {} ffmpeg -i {} {}_%011d.jpg
```

B.1. Настройка среды выполнения

1.1. Install the GCloud SDK

https://cloud.google.com/sdk/docs/install

1.2. Connect to Linux VM

By the `gcloud cloud-shell` utility, or by Cloud Shell activating in the browser.

![CPU1](https://user-images.githubusercontent.com/12969866/141667317-bcdb2fca-53f6-4072-b9a8-073b1a99796f.png)

1.3. Activate Cloud GPU

https://cloud.google.com/gpu<br/>
https://cloud.google.com/compute/docs/gpus/create-vm-with-gpus<br/>
https://cloud.google.com/compute/docs/gpus/install-drivers-gpu<br/>
https://cloud.google.com/ai-platform/training/docs/using-gpus

1.3. Configure NVIDIA drivers, CUDA and cuDNN for neuronet.



Disable `apt get` warning in GCP Console:
```mkdir ~/.cloudshell && touch ~/.cloudshell/no-apt-get-warning```

```
sudo apt-get update
...
W: Failed to fetch http://storage.googleapis.com/bazel-apt/dists/stable/InRelease  Temporary failure resolving 'storage.googleapis.com'
W: Some index files failed to download. They have been ignored, or old ones used instead.
```

1.3a Disable nouveau driver and install `CUDA`.

```
$ sudo dpkg -S cuda | wc -l
18
$ cd /etc/modprobe.d/
$ sudo nano blacklist.conf
```
```
# Add the following to this file
# Blacklist nouveau driver (for nvidia driver installation)
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
```sudo update-initramfs -u```


Deinstall old NVIDIA drivers
```
sudo apt-get remove --purge '^nvidia-.*'
sudo apt-get autoremove
```
Install NVIDIA drivers and CUDA
a) Download sh-installers from https://www.nvidia.com/ru-ru/

![NVIDIA-driver-TESLA-K80](https://user-images.githubusercontent.com/12969866/141670031-d8a99eb2-149b-4cac-bd47-a75fa68b5195.png)

```
$ wget https://developer.download.nvidia.com/compute/cuda/11.5.0/local_installers/cuda-repo-debian11-11-5-local_11.5.0-495.29.05-1_amd64.deb
$ sudo dpkg -i cuda-repo-debian11-11-5-local_11.5.0-495.29.05-1_amd64.deb
$ sudo apt-key add /var/cuda-repo-debian11-11-5-local/7fa2af80.pub
$ sudo add-apt-repository contrib
$ sudo apt-get update
$ sudo apt-get -y install cuda
```
1.3b Install PyTorch

```$ conda install pytorch torchvision -c pytorch```

1.4. Установка системы распознавания дыма на основе нейронной сети

Using Google Cloud Shell: https://cloud.google.com/shell/docs/using-cloud-shell

```

```


...

2.0. Загрузить тестовую выборку на Google Cloud vm-instance

```$ wget -c https://my-server/testing.tar -O smoke-yolo/testing.tar```

or ```Google Cloud Platform\Activate Cloud Shell\More...Upload File```

2.1. Проверка на тестовой выборке, 279326 JPEG-файлов, полученных из исходных MTS, AVI.

```
$ find testing/ -type f -name '*.jpg' | wc -l
279326
$ cd testing/
$ mkdir avi && find . -type f -iname '*.avi*' | xargs -n1 -I{} mv {} avi/
$ mkdir mts && find . -type f -iname '*.mts*' | xargs -n1 -I{} mv {} mts/
$ find avi/ -type f -name '*.jpg' | wc -l
270122
$ find mts/ -type f -name '*.jpg' | wc -l
9204
$ du -sh * 
6.2G	avi
649M	mts
```
![fin2_h264 avi_00000204451-fx](https://user-images.githubusercontent.com/12969866/141668872-543a2659-c8e5-4682-837b-1ceac9fbc8e2.jpg)

![fin2_h264 avi_00000197399-fx](https://user-images.githubusercontent.com/12969866/141668878-e21dd586-00aa-4ac5-be8d-3f50b99a3646.jpg)


...


C* Также, возможно использование нейросети U-Net на базе датасета Fire Luminosity Airborne-based Machine learning Evaluation (от Air Force Research Laboratory, US) для обнаружения открытого огня с дронов

См. препринт https://arxiv.org/pdf/2012.14036.pdf
