# 手把手教你TensorFlow源代码阅读

---

`时间：2020-05-08` `作者：魏文应`

---



# 代码准备

复制一份TensorFlow源代码，方便一会我们进行对比，以及代码恢复。

## 创建分支

检出TensorFlow相应版本：

```bash
# git checkout -b 新起一个分支名称 检出的TensorFlow版本
git checkout -b code-reading v2.2.0
```

然后执行上面的编译、安装、测试等操作:

```bash
# 检查bazel版本
bazel version
# 配置，最小化编译即可，CUDA什么的，暂时不编译链接。
./configure
# 编译
bazel build //tensorflow/tools/pip_package:build_pip_package
```

编译成功，打印类似如下：

```bash
Target //tensorflow/tools/pip_package:build_pip_package up-to-date:
  bazel-bin/tensorflow/tools/pip_package/build_pip_package
INFO: Elapsed time: 8302.967s, Critical Path: 234.31s
INFO: 14283 processes: 14283 local.
INFO: Build completed successfully, 15315 total actions
```

生成pip安装包：

```bash
# 生成pip安装包
./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

# 尝试安装生成的.whl安装包
# pip install /tmp/tensorflow_pkg/相应的安装包
pip install /tmp/tensorflow_pkg/tensorflow-2.2.0-cp37-cp37m-linux_x86_64.whl
```

切换目录（不要在tensorflow项目的根目录下即可），并打开 `python` 解释器：

```bash
# 如果在在tensorflow项目的根目录下，执行import tensorflow as tf，会错误地认为
# 项目目录下的tensorflow目录，是我们要导入的包，导致错误。
>>> import tensorflow as tf
>>> t = tf.constant([1, 1, 1])
>>> print(t)
<tf.Tensor: shape=(3,), dtype=int32, numpy=array([1, 1, 1], dtype=int32)>
```

测试完，记得卸载：

```bash
pip uninstall tensorflow
```

## Fork项目

在Github上，Fork拷贝TensorFlow项目，并为项目添加上面创建的 `code-reading` 这个分支。

>   Tips:
>
>   github的Fork功能，是拷贝复制项目的意思，将项目复制到你的账号下，比如我fork后，我得到如下项目：
>
>   ```bash
>   https://github.com/weiwenying/tensorflow.git
>   ```
>
>   然后，将分支 `code-reading` 推送到上面项目中：
>
>   ```bash
>   # git remote add 名称 你的仓库地址
>   git remote add wwy https://github.com/weiwenying/tensorflow.git
>   # git push 仓库地址名称 
>   git push wwy code-reading
>   ```
>
>   ![02_fork_tensorflow项目.png](./img/02_fork_tensorflow项目.png)
>
>   还可以像我一样，添加笔记，并尝试提交：
>
>   ```bash
>   # git add 你的笔记
>   git add analyze_docs/01_手把手教你TensorFlow源代码阅读/01_手把手教你TensorFlow源代码阅读.md
>   # git commit -m "提交说明"
>   git commit -m "添加了代码分析笔记。"
>   # git push 你要上传的仓库地址 分支名称
>   git push wwy code-reading
>   ```

