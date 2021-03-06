# gan_fingerprint_removal

## Introduction

This repo provides the implementation of a GAN fingerprint removal approach, as described in [Real or Fake? Spoofing State-Of-The-Art Face Synthesis Detection Systems](https://arxiv.org/abs/1911.05351)

Use this bibtex to cite this repository:
```
@misc{1911.05351,
    title={{GANprintR: Improved Fakes and Evaluation of the State-of-the-Art in Face Manipulation Detection}},
    author={João C. Neves and Ruben Tolosana and Ruben Vera-Rodriguez and Vasco Lopes and Hugo Proença and Julian Fierrez},
    year={2019},
    eprint = {arXiv:1911.05351},
}
```

## Remove GAN fingerprint from a folder of images

````
python gan_fingerprint_removal.py IMAGE_PATH TRANSFORMED_IMAGE_PATH AE_MODEL_PATH
python gan_fingerprint_removal.py F:\\FACE_DATASETS\\NVIDIA_FakeFace\\byimg_alignedlib_0.3 fingerprint_removal\\ae_models\\ae32\\ae32_epoch100.pytorch
````

## Datasets

The TPDNE dataset comprises 150K synthetic face images generated by the StyleGAN.

TO BE UPDATED

A subset of the this dataset comprising only frontal aligned faces is also available.

[http:/socia-lab.di.ubi.pt/~jcneves/TPDNE/byimg_alignedlib_0.3.zip](http:/socia-lab.di.ubi.pt/~jcneves/TPDNE/byimg_alignedlib_0.3.zip)

The 100K-Faces dataset comprises 100K synthetic face images generated by the StyleGAN.

[https://generated.photos/](https://generated.photos/)

A subset of this dataset ccomprising only frontal aligned faces is also available.

[http:/socia-lab.di.ubi.pt/~jcneves/100KFaces/byimg_alignedlib_0.3.zip](http:/socia-lab.di.ubi.pt/~jcneves/100KFaces/byimg_alignedlib_0.3.zip)


## Paper experiments

This section details how to replicate the experiments carried out in the paper

### Controlled Scenario

- Generate a dataset with two classes: real faces, synthetic faces
````
python create_real_to_fake_dataset.py REAL_FACES_DATASET_PATH SYNTHETIC_FACES_DATASET_PATH NEW_DATASET_PATH 
python create_real_to_fake_dataset.py  E:\FACE_DATASETS\VGG_FACE_2\byid_alignedlib_0.3_train\ E:\FACE_DATASETS\NVIDIA_FakeFace\byimg_alignedlib_0.3\ E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE
````

- Train a CNN on the dataset created
````
python ../../train_pytorch_gpu.py NEW_DATASET_PATH
python ../../train_pytorch_gpu.py E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE
````

- Test the model trained on the test set of dataset created

````
python ../../test_pytorch_gpu.py MODEL_PATH NEW_DATASET_PATH
python ../../test_pytorch_gpu.py E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE
````

- Run all paper experiments
````
python run_all_experiments.py
run_all_experiments.bat
````

### In The Wild (Unseen Dataset)

- Transform a dataset with two classes: real faces, synthetic faces
````
python create_real_to_fake_dataset_mixed.py REAL_FACES_DATASET(1)_PATH SYNTHETIC_FACES_DATASET(1)_PATH REAL_FACES_DATASET(2)_PATH SYNTHETIC_FACES_DATASET(2)_PATH NEW_DATASET_PATH 
python create_real_to_fake_dataset_mixed.py E:\FACE_DATASETS\VGG_FACE_2\byid_alignedlib_0.3_train\ E:\FACE_DATASETS\NVIDIA_FakeFace\byimg_alignedlib_0.3\ E:\FACE_DATASETS\VGG_FACE_2\byid_alignedlib_0.3_train\ E:\FACE_DATASETS\100K_FAKE\byimg_alignedlib_0.3\ E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE_train_VF2_100F_test
````

- Test the model trained on the original dataset (refer to controlled scenario experiment)

````
python ../../test_pytorch_gpu.py MODEL_PATH TRANSFORMED_DATASET_PATH
python ../../test_pytorch_gpu.py E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE_train_VF2_100F_test_R32 E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE
````

- Run all paper experiments

````
python run_all_experiments_downsize.py
run_all_experiments_downsize.bat
````

### In The Wild (Resolution)

- Transform a dataset with two classes: real faces, synthetic faces
````
python transform_dataset_downsize.py IMG_RESOLUTION DATASET_PATH TRANSFORMED_DATASET_PATH 
python transform_dataset_downsize.py 32 E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE_train_VF2_100F_test E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE_train_VF2_100F_test_R32
````

- Test the model trained on the original dataset (refer to controlled scenario experiment)

````
python ../../test_pytorch_gpu.py MODEL_PATH TRANSFORMED_DATASET_PATH 
python ../../test_pytorch_gpu.py E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE_train_VF2_100F_test_R32 E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE
````

- Run all paper experiments
````
python run_all_experiments_downsize.py
run_all_experiments_downsize.bat
````

### GAN Fingerprint Removal

- Transform a dataset with two classes: real faces, synthetic faces

````
python transform_dataset_cae.py AE_SNAPSHOT_EPOCH AE_SNAPSHOT_LATENT_CODE_SIZE DATASET_PATH TRANSFORMED_DATASET_PATH 
python transform_dataset_cae.py 100 32 E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE_train_VF2_100F_test real2fake_cae32_100_real2fake_VF2_TPDNE_train_VF2_100F_test
````

- Test the model trained on the original dataset (refer to controlled scenario experiment)

````
python ..\test_pytorch_gpu.py MODEL_PATH TRANSFORMED_DATASET_PATH
python ..\test_pytorch_gpu.py real2fake_cae32_100_real2fake_VF2_TPDNE_train_VF2_100F_test E:\gan_fingerprint_removal\data\real2fake_VF2_TPDNE
````

- Run all paper experiments

````
python run_all_experiments_ae.py
run_all_experiments_downsize.bat
````

- Remove GAN fingerprint from 
