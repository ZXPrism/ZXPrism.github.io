---
date:
  created: 2024-09-06
---

# PAT！

不久之前，我新建了一个名为 PAT 的 repo，用于存放我的 PAT 题解。

今天写博客的时候，突然想起来这里也放了不少题解，既然如此，为什么还需要两个 repo 呢？遂决定把 PAT repo 里面的题解迁移过来。

问题在于，原仓库里面的题解都是以 *.cpp 的形式存放的，而本博客的文章必须使用 markdown 格式。

一个一个修改？不。这种机械且重复的事情可以交给计算机去做！

我写了一个 python 程序来进行题解的迁移：

```py linenums="1" title="transfer.py"
import os, re

cwd = os.getcwd()
pat = re.compile(r"[\w-]+\.cpp")

file_paths = [path for path in os.listdir(cwd) if pat.match(path)]

template = """---
date:
  created: 2024-09-06
---

# title

```cpp linenums="1"
"""

for path in file_paths:
    fp = open(path)
    code = fp.readlines()
    fp.close()
    
    os.remove(path)

    fp2 = open(path[: path.find(".")] + ".md", "w")
    fp2.write(template)
    fp2.writelines(code)
    fp2.close()
```
