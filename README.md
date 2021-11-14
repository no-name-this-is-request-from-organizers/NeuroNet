# NeuroNet

A. Настройка среды для обучения нейросети

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

```gcloud compute scp --project PROJECT_ID --zone VM_ZONE --recurse <local file or directory> fg-vm:~/```

B.1. Настройка среды выполнения

...

C* Также, возможно использование нейросети U-Net на базе датасета Fire Luminosity Airborne-based Machine learning Evaluation (от Air Force Research Laboratory, US) для обнаружения открытого огня с дронов

См. препринт https://arxiv.org/pdf/2012.14036.pdf
