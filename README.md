# Install_CUDA_and_CUDA-toolkit
◎因為本流程全部安裝成功後不建議再做任何更新，若有任何其它套件需使用，最好在進行本流程前先安裝，也建議Python2.7與3的套件皆安裝。  
◎本流程Python會安裝2.7與3兩種版本，因此pip的使用也依版本分為pip2與pip3，可以選擇只安裝2.7、只安裝3，或2.7與3的套件皆安裝。  
◎常見的python3版本還分為3.5與3.6，請固定使用其中一種。  


更新清單   
$ sudo apt-get update   
$ sudo apt-get upgrade  

  
到https://developer.nvidia.com/cuda-toolkit-archive  
下載cuda工具包丟到家目錄  
   
選擇CUDA Toolkit 8.0 GA2 (Feb 2017)→  
Linux→x86_64→Ubuntu→16.04→deb(local)→Download(1.9GB)  
   
安裝deb檔案  
$ sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb  
     
下載補充包  
選擇Patch2        Download (121.4MB)  
安裝Patch2  
$ sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-cublas-performance-update_8.0.61-1_amd64.deb  
   
將CUDA路徑添加至環境變數   
$ echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc  
$ echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc  
$ echo 'export CUDA_HOME=/usr/local/cuda-8.0'>> ~/.bashrc  
$ echo 'export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}'>> ~/.bashrc  
$ echo 'export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda-8.0/lib64:/usr/local/cuda-8.0/extras/CUPTI/lib64:~/cuDNN_installpath" '>> ~/.bashrc  
    
啟動環境變數   
$ source ~/.bashrc  
   
成功後更新清單  
$ sudo apt-get update   
$ sudo apt upgrade  
   
重新開機  
$ reboot  
  
安裝cuda+cuda_tools  
$ sudo apt-get install cuda  
$ sudo apt-get install nvidia-cuda-toolkit  
  
重開機  
  
若遇到以下錯誤碼: (此為解決Nvidia驅動的bug)  
/sbin/ldconfig.real: /usr/lib/nvidia-384/libEGL.so.1 is not a symbolic link  
/sbin/ldconfig.real: /usr/lib32/nvidia-384/libEGL.so.1 is not a symbolic link  
以下為375.39版本的顯卡驅動的修復方式：  
$ sudo mv /usr/lib/nvidia-375/libEGL.so.1 /usr/lib/nvidia-375/libEGL.so.1.org  
$ sudo mv /usr/lib32/nvidia-375/libEGL.so.1 /usr/lib32/nvidia-375/libEGL.so.1.org  
$ sudo ln -s /usr/lib/nvidia-375/libEGL.so.375.39 /usr/lib/nvidia-375/libEGL.so.1  
$ sudo ln -s /usr/lib32/nvidia-375/libEGL.so.375.39 /usr/lib32/nvidia-375/libEGL.so.1  
以下為384.111版本的顯卡驅動的修復方式：  
$ sudo mv /usr/lib/nvidia-384/libEGL.so.1 /usr/lib/nvidia-384/libEGL.so.1.org  
$ sudo mv /usr/lib32/nvidia-384/libEGL.so.1 /usr/lib32/nvidia-384/libEGL.so.1.org  
$ sudo ln -s /usr/lib/nvidia-384/libEGL.so.384.111 /usr/lib/nvidia-384/libEGL.so.1  
$ sudo ln -s /usr/lib32/nvidia-384/libEGL.so.384.111 /usr/lib32/nvidia-384/libEGL.so.1  
(其他版本請自行更改)  
  
  
輸入,測試cuda安裝  
$ nvcc --version  
成功顯示如下:  
nvcc: NVIDIA (R) Cuda compiler driver  
Copyright (c) 2005-2016 NVIDIA Corporation  
Built on Tue_Jan_10_13:22:03_CST_2017  
Cuda compilation tools, release 8.0, V8.0.61  
  
查詢nvidia 顯卡驅動版本並記下來，例如375.39 or 384.90  
$ nvidia-smi  
   
安裝cuDNN library  
到https://developer.nvidia.com/rdp/cudnn-download（需登入）  
  
要下載的有：  
Download cuDNN v6.0 (April 27, 2017), for CUDA 8.0中的cuDNN v6.0 Library for Linux  
（此為壓縮檔,依以下順序解壓縮再放置到所需位置）  
$ tar xvf cudnn-8.0-linux-x64-v6.0.tgz  
$ cd cuda  
  
複製libray檔案到目標位置  
$ sudo cp */*.h /usr/local/cuda/include/  
$ cd lib64  
$ sudo cp  libcudnn.so.6.0.21  /usr/local/cuda/lib64/  
  
修改權限  
$ sudo chmod a+r /usr/local/cuda/lib64/libcudnn*  
$ cd /usr/local/cuda-8.0/targets/x86_64-linux/lib  
$ sudo ln -s libcudnn.so.6.0.21  libcudnn.so.6  
$ sudo ln -s libcudnn.so.6  libcudnn.so  
  
如果出現以下error：  
Processing triggers for libc-bin (2.23-0ubuntu10) ...  
/sbin/ldconfig.real: /usr/local/cuda-8.0/targets/x86_64-linux/lib/libcudnn.so.6 is not a symbolic link  
請依以下流程處理：  
$ sudo rm -rf  /usr/local/cuda/lib64/libcudnn.so.6.0.21 libcudnn.so.6  
$ cd  /usr/local/cuda/lib64  
$ sudo ln -s libcudnn.so.6.0.21 libcudnn.so.6   
$ sudo ln -s libcudnn.so.6 libcudnn.so      
$ cd ~/cuda/include
$ sudo cp cudnn.h /usr/local/cuda/include/  #複製標頭檔  
修改權限   
$ sudo chmod a+r /usr/local/cuda/lib64/libcudnn*  
本流程使用的cudnn版本為6.0.21，若為其它版本請自行更換檔名。  
   
確認更新  
$ sudo apt update  
 
