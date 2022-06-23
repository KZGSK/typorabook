

# win11笔记本30系显卡pytorch1.11环境配置

##  安装conda创建虚拟环境

1. 从官网下载：[Anaconda | The World's Most Popular Data Science Platform](https://www.anaconda.com/)

2. 安装时注意勾选环境变量（但我这里选项是灰的点不了，只能后面再添加不知道为啥）<img src="../image\image-20220623221456927.png" alt="image-20220623221456927" style="zoom:50%;" />

3. 若没有加到环境变量则手动添加

   在电脑中搜索环境变量，打开下面的path选项，添加3个相应的安装路径<img src="../image\image-20220623222130296.png" alt="image-20220623222130296" style="zoom:50%;" />

4. 换pip源：在用户下的个人文件下创建pip.txt，添加如下文字后再改为ini文件

   ```txt
   [global]
   index-url = http://pypi.mirrors.ustc.edu.cn/simple
   [install]
   use-mirrors =true
   mirrors =http://pypi.mirrors.ustc.edu.cn/simple/
   trusted-host =pypi.mirrors.ustc.edu.cn
   ```

   换conda源，参考：[anaconda | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

5. 创建虚拟环境 

   ```pyt
   conda create -n torch1.11 python=3.8
   conda activate torch1.11
   ```

---

创建虚拟环境的bug：CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://repo.anaconda.com/pkgs/main/win-64/current_repodata.json> Elapsed: -。。。。。。

解决：在换conda源时创建了.condarc文件，找到打开将https改为http

## 安装pytorch1.11

PyTorch官网：https://pytorch.org/

1. 检查cuda和驱动版本

   在命令行输入`nvidia-smi`指令就能看到自己nvidia的驱动版本（前提是电脑上有装nvidia的驱动）<img src="../image\image-20220623224704860.png" alt="image-20220623224704860" style="zoom: 67%;" />

   然后在NVIDIA官网，看下CUDA版本以及GPU驱动的对应关系：https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html

   在我的理解中只要cuda版本大于pytorch中的版本就可以，但驱动版本要与cuda版本对应。我的满足，所以无需更新cuda和驱动。

   2. 安装pytorch

      <img src="../image\image-20220623224942797.png" alt="image-20220623224942797" style="zoom:67%;" />

      在对应虚拟环境中安装。注意的是网上说要把-c pytorch删掉才可以从清华源安装。但我这里提示`torchvision` 和`torchaudio`找不到。所以我直接按照要求安装没有删除，速度意外不慢。

   3. 验证安装成功与否

      ```py
      python
      import torch
      x = torch.rand(5, 3)
      print(x)
      #tensor([[0.3380, 0.3845, 0.3217],
      #        [0.8337, 0.9050, 0.2650],
      #        [0.2979, 0.7141, 0.9069],
      #        [0.1449, 0.1132, 0.1375],
      #        [0.4675, 0.3947, 0.1426]])
      import torch
      torch.cuda.is_available()
      ```

   
   # 安装pycharm
   
   参考：[Pycharm专业版2022.3.3安装激活 (qq.com)](https://mp.weixin.qq.com/s?__biz=MzIyMjYzMDMyMQ==&mid=2247486104&idx=1&sn=2c3ce4ac12c79c8b6f2ca9fb50031706&chksm=e82bd246df5c5b509dff0806ca8b9b8e80adb38a9e279dd57d35b42405d6ba37915c592bda93&mpshare=1&scene=23&srcid=0623vvLrJyR0kBYWz6loD0md&sharer_sharetime=1655996489703&sharer_shareid=a30ff933acc1ad487e9413d63254b95a#rd)