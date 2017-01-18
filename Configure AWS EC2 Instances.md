# Configure AWS EC2 Instances

Copy and select relevant portions and paste to a file.sh, and then run it on your amazon EC2.

if hosts is not recognised, 

```
cd /etc
nano hosts
127.0.0.1 ip-xx-xx-xx-xxx
```

## Background requirements and Anaconda

```
#requirements 
sudo apt-get update 
sudo apt-get --assume-yes upgrade
sudo apt-get --assume-yes install tmux build-essential gcc g++ make binutils
sudo apt-get install unzip

#Installing Anaconda - Get updated link from the website if required
wget https://repo.continuum.io/archive/Anaconda2-4.2.0-Linux-x86_64.sh
bash Anaconda2-4.2.0-Linux-x86_64.sh -b
echo "export PATH="/home/ubuntu/anaconda2/bin:$PATH"" >> ~/.bashrc
source .bashrc

conda upgrade -y --all
conda update setuptools
conda install -y libgcc 

#install other packages if you need 
pip install hyperopt
``` 

## Configuring Juypter Notebook
Remember to choose your port number

```
jupyter notebook --generate-config

jupass=`python -c "from IPython.lib import passwd; print(passwd())"`

mkdir certs
cd certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.key -out mycert.pem

cd
cd ./jupyter

echo "c.NotebookApp.password = u'"$jupass"'" >> ./jupyter_notebook_config.py

echo "c.NotebookApp.ip = '*'
c.IPKernalApp.pylab = 'inline'
c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'
c.NotebookApp.keyfile = u'/home/ubuntu/certs/mycert.key'
c.NotebookApp.port = 9999
c.NotebookApp.open_browser = False" >> ./jupyter_notebook_config.py

cd

```

access your notebook at 

`https:/ip:Address:9999/`

To let your juypter notebook stay connected without ssh,

`nohup juypter notebook`


## Installing Cudnn

```
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
sudo apt-get update
sudo apt-get -y install cuda
sudo modprobe nvidia
nvidia-smi

wget http://platform.ai/files/cudnn.tgz
tar -zxf cudnn.tgz
cd cuda
sudo cp lib64/* /usr/local/cuda/lib64/
sudo cp include/* /usr/local/cuda/include/

```
## Installing Theano

```
pip install theano
touch .theanorc

echo "[global]
device = gpu
floatX = float32
[mode] = FAST_RUN
[nvcc]
fastmath = True
[lib]
cnmem = 0.95
[cuda]
root = /usr/local/cuda" > ~/.theanorc

```

to check whether Theano has been installed properly, 

```
from theano import function, config, shared, tensor
import numpy
import time

vlen = 10 * 30 * 768  # 10 x #cores x # threads per core
iters = 1000

rng = numpy.random.RandomState(22)
x = shared(numpy.asarray(rng.rand(vlen), config.floatX))
f = function([], tensor.exp(x))
print(f.maker.fgraph.toposort())
t0 = time.time()
for i in range(iters):
    r = f()
t1 = time.time()
print("Looping %d times took %f seconds" % (iters, t1 - t0))
print("Result is %s" % (r,))
if numpy.any([isinstance(x.op, tensor.Elemwise) and
              ('Gpu' not in type(x.op).__name__)
              for x in f.maker.fgraph.toposort()]):
    print('Used the cpu')
else:
    print('Used the gpu')
    
```

It should print `Used the gpu`

## Installing Keras

```
pip install keras
mkdir ~/.keras
echo '{
    "image_dim_ordering": "th",
    "epsilon": 1e-07,
    "floatx": "float32",
    "backend": "theano"
}' > ~/.keras/keras.json

```

## Installing tensorflow

```
echo "export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
export CUDA_HOME=/usr/local/cuda" >> .bashrc

pip install tensorflow-gpu

```

To check if its working,

```
>>> import tensorflow as tf
I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcublas.so locally
I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcudnn.so locally
I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcufft.so locally
I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcuda.so.1 locally
I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcurand.so locally
```

## Installing XGB (With/Without GPU)

give up as of now. not working.

```
wget https://github.com/NVlabs/cub/archive/1.6.4.zip
unzip 1.6.4.zip
git clone --recursive https://github.com/dmlc/xgboost
cd xgboost

sudo apt install cmake
mkdir build
cd build
cmake .. -DPLUGIN_UPDATER_GPU=ON -DCUB_DIRECTORY=/home/ubuntu/cub-1.6.4

cd ..

make -j4 #or bash build.sh

sudo apt-get install python-setuptools

sudo-s

python setup.py install




```




