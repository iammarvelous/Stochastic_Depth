Deep Networks with Stochastic Depth
====================
This repository hosts the Torch 7 code for the paper _Deep Networks with Stochastic Depth_
available at http://arxiv.org/abs/1603.09382. For now, the code reproduces the results in Figure 3 for CIFAR-10 and CIFAR-100, and Figure 4 left for SVHN. The code for the 1202-layer network on CIFAR-10 (which requires some memory optimization) will be available very soon.

### Prerequisites
- Torch 7 and CUDA with the basic packages (nn, optim, image, cutorch, cunn).
- cudnn (https://developer.nvidia.com/cudnn) and torch bindings (https://github.com/soumith/cudnn.torch).
- nninit torch package (https://github.com/Kaixhin/nninit); `luarocks install nninit` should do the trick.
- CIFAR-10 and CIFAR-100 datasets in Torch format; this script https://github.com/soumith/cifar.torch should very conveniently handle it for you.
- SVHN dataset in Torch format, available at this website http://torch7.s3-website-us-east-1.amazonaws.com/data/svhn.t7.tgz. Please note that running on SVHN requires roughly 28GB of RAM for dataset loading.

### Getting Started on CIFAR-10
```bash
git clone https://github.com/yueatsprograms/Stochastic_Depth
cd Stochastic_Depth
git clone https://github.com/soumith/cifar.torch
cd cifar.torch
th Cifar10BinToTensor.lua
cd ..
mkdir results
th main.lua -dataRoot cifar.torch/ -resultFolder results/ -deathRate 0.5
```

### Usage Details
`th main.lua -dataRoot path_to_data -resultFolder path_to_save -deathRate 0.5`<br/>
This command runs the 110-layer ResNet on CIFAR-10 with stochastic depth, using linear decaying survival probabilities ending in 0.5. The `-device` flag allows you to specify which GPU to run on. On our machine with a TITAN X, each epoch takes about 60 seconds, and the program ends with a test error (selected by best validation error) of __5.23%__.

The default deathRate is set to 0. This is equivalent to a constant depth network, so to run our baseline, enter: <br/>
`th main.lua -dataRoot path_to_data -resultFolder path_to_save` <br/>
On our machine with a TITAN X, each epoch takes about 75 seconds, and this baseline program ends with a test error (selected by best validation error) of 6.41% (see Figure 3 in the paper).

You can run on CIFAR-100 by adding the flag `-dataset cifar100`. Our program provides other options, for example, your network depth (`-N`), data augmentation (`-augmentation`), batch size (`-batchSize`) etc. You can change the optimization hyperparameters in the sgdState variable, and learning rate schedule in the the main function. The program saves a file every epoch to `resultFolder`/errors\_`N`\_`dataset`\_`deathMode`\_`deathRate`, which has a table of tuples containing your test and validation errors until that epoch.

The architecture and number of epochs for SVHN used in our paper is slightly different, please use the following command:<br/>
`th main.lua -dataRoot path_to_data -resultFolder path_to_save -dataset svhn -N 25 -maxEpochs 50 -deathRate 0.5`

### Contact
My email is ys646 at cornell.edu. I'm happy to answer any of your questions, and I'd very much appreciate your suggestions. 
