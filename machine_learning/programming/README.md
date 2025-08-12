* 看gpu状态的gpustat：`gpustat -cpu`

* 训练模型的辅助：wandb tutorial
  * * wandb的report功能：可以很方便地筛选exp、整理表格
  *     * wandb log image：给train加上了visualize reconstruction的功能
* * 熟悉condor：    
  * * 在submit file里定义LD_LIBRARY_PATH到python env的lib目录可以解决GLIBCXX找不着的问题
  *     * 解决了condor一次提交多个job出现的bug：读h5py遇到Resource temporarily unavailable。解决方法：with h5py.File(cache_path,”r”) as f:
* 设置cfg.DEBUG：debug的时候只加载少量数据