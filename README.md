# ALPS2023-MT-LAB
Python notebook and models for the [MT Lab @ ALPS 2023](http://lig-alps.imag.fr)

This repository is a modified version of [another tutorial on NMT](https://github.com/nyu-dl/AMMI-2019-NLP-Part2/blob/master/02-day-RLM%26NMT/02.c.NMT/NMT.ipynb) by Kyunghyun Cho et al.

### Running on Google Colab

1. Go to https://colab.research.google.com
2. Under the "GitHub" tab, type the URL of this repo (https://github.com/naverlabseurope/ALPS2023-MT-LAB), then click on "NMT.ipynb"

![image](https://user-images.githubusercontent.com/1795321/213119740-7197f65a-25f7-4c4a-864f-46266a1b3fbd.png)
![image](https://user-images.githubusercontent.com/1795321/213120125-383bf9f7-6537-4d0f-9b07-db149edc7fb5.png)

3. In the Colab menu, go to "Runtime / Change runtime type", then select "GPU" in the "Hardware accelerator" drop-down list

![image](https://user-images.githubusercontent.com/1795321/213120345-d2a89b85-afd6-4d02-8eee-b9fdc189cfc7.png)
![image](https://user-images.githubusercontent.com/1795321/213120508-ec8f6b37-4b34-46cf-a262-a92d162e7dca.png)

4. Open [this link](https://drive.google.com/drive/folders/1E07YaKths98YpoBCH2PjdtTPqOXgfdZB?usp=sharing) and connect to your Google Drive account
5. Then go to "Shared with me" in your Google Drive, right-click the "ALPS2023-NMT" folder and select "Add shortcut to Drive"

![drive](https://user-images.githubusercontent.com/1795321/149558193-c7d008e7-09c8-418d-8fcf-2cfb517a52dc.png)

6. Start playing with the notebook. Note that the models you train in the notebook won't be saved (they will be lost when you close the notebook). However, you can manually download them to your computer or copy them to your Google Drive if you wish.

Note: if you don't have a Google account, you can still run the notebook in Colab. You just need to set `colab = False`, and the `download-data.sh` script will be used to download the data and pre-trained models.

### Running the notebook on your own computer

```
git clone https://github.com/naverlabseurope/ALPS2023-MT-LAB.git
cd ALPS2023-MT-LAB
scripts/setup.sh            # creates a Python environment, installs the dependencies and downloads the data and models
scripts/run-notebook.sh     # starts a jupyter notebook where the lab will take place
```
You also need to set `colab = False` in the notebook.
If you don't have a GPU, you need to set `cpu = True`. Models will be very slow to train, but you can still do inference in a reasonable time.

### Running the notebook remotely

In the following, replace `HOSTNAME` by the name of your server.

1. SSH to the server, install the repo and run the notebook
```
ssh HOSTNAME
git clone https://github.com/naverlabseurope/ALPS2023-MT-LAB.git
cd ALPS2023-MT-LAB
scripts/setup.sh
scripts/run-notebook.sh    # modify this script to change the port if 8888 is already used
```
2. Create an SSH tunnel from your machine
```
ssh -L 8888:localhost:8888 HOSTNAME
```
3. Open the URL printed by the `scripts/run-notebook.sh` command (which looks like http://127.0.0.1:8888/?token=XXX) in your favorite browser
4. Enjoy!

### Training models via the command line

The `train.py` script can be used to train models directly from the command line (locally or via SSH on a remote machine), without using the notebook. It is convenient for training multiple models.

How to use it:

```
nvidia-smi
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA Tesla T4     Off  | 00000000:03:00.0 Off |                    0 |
| N/A   47C    P0    26W /  70W |  10734MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  NVIDIA Tesla T4     Off  | 00000000:41:00.0 Off |                    0 |
| N/A   30C    P8     9W /  70W |      3MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
# GPU 1 is free
CUDA_VISIBLE_DEVICES=1 ./train.py models/en-fr/transformer.pt -s en -t fr \
--model-type transformer --encoder-layers 2 --decoder-layers 1 --heads 4 --embed-dim 512 --ffn-dim 512 \
--epochs 10 --lr 0.0005 --batch-size 512 --dropout 0.1 -v
```

This will reproduce the training of the EN-FR Transformer model we shared.
Run `./train.py -h` to see more options.
