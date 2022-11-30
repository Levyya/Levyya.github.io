---
title: conda命令
date: 2022-10-12 10:07:58
tags:
- python
---

conda 创建环境

```
conda create -n [name] python==3.8
# 通过现有环境创建
conda create --name [newName] --clone [oldName]
```

conda 移除环境

```
conda remove --name [name]  --all
```

conda 环境添加到jupyter

```
python -m ipykernel install --user --name py3.8  --display-name py3.8
```

jupyter 删除虚拟环境

```
jupyter kernelspec uninstall [name]
```

