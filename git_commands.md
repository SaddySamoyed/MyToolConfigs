# Git

## `.git` 的组成

我们已经使用了很久 git 了,, 但是 barely 知道它的实现原理, 只模糊地知道它的用途

所以我认真地 (指使用 gpt) 学习了一下 git 的原理

Git 本质上是一个分布式的 Content-Addressable File System.

这里的 "Content-Addressable" 中的 Content, 注意并非是文件的 Content, 而是指 git 系统给一个 commit 赋的 HASH 值.

首先我们看到最大, 最外层的 `.git` 文件夹:

.git 由以下的 files 和 sub directory 组成:

| 文件/目录     | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| `HEAD`        | 记录当前检出的是哪个分支（通常指向 `refs/heads/main`）或某个 commit（在 detached HEAD 状态下） |
| `config`      | 仓库级别的配置文件，类似于 `git config --local` 设置的内容   |
| `description` | 给 GitWeb 用的仓库描述，平时用不到                           |
| `index`       | 暂存区（staging area）的快照信息，用于记录哪些文件准备提交   |
| `objects/`    | 所有 Git 对象（blob、tree、commit、tag）的实际存储地         |
| `refs/`       | 引用目录，保存指向各个 commit 的分支、tag 信息               |
| `hooks/`      | Git 钩子脚本目录，如 pre-commit、post-commit，支持自定义行为 |
| `logs/`       | 每次引用（HEAD、分支）变化的日志，可用于 `git reflog`        |
| `info/`       | 存储一些辅助信息，比如 `.git/info/exclude` 是局部忽略文件列表 |
| `packed-refs` | Git 会把部分 refs（分支/标签）打包存储在这里，提升查找效率   |

### `.git/objects/`

我们很容易发现, 最核心的部分就是里面的 `/objects/` 这个 sub directory. 这个 subdirectory 储存了各种核心 objects. 其结构是:

```shell
.git/objects/
├── 01/
│   └── abcd...    # 一个对象，其 SHA-1 哈希是 01abcd...
├── 3f/
│   └── 94ef...
└── info
```

这些对象本质上都是通过 zlib 压缩的二进制数据，可以用命令查看：

```shell
git log --pretty=raw
git cat-file -p <hash>
```



|    类型     |                          描述                          |
| :---------: | :----------------------------------------------------: |
|  **Blob**   |     存储文件内容 (binary large object), 不带文件名     |
|  **Trees**  | 存储目录结构（包括文件名、权限和 blob 或 tree 的引用） |
| **Commits** |     快照，记录 Tree 指针、父 Commit、作者等元信息      |
|  **Tags**   |              可选，给某个 commit 加上标签              |

每次我们修改文件并 `git add` 它, git 都会计算新的 SHA-1 hash (比如 `abc123...`), 并对应地创建新的 blob; 用暂存区 (index) 记录新的 blob, 

```shell
git hash-object myfile.txt           # 计算该文件的 blob 哈希
git cat-file -p <blob_hash>         # 查看该 blob 对象的真实内容
```

而 `git commit` 时, git 会创建一个新的 tree (目录结构, 把文件名映射到新的 SHA-1 hash)



比如说, 你只有一个文件 `hello.txt`, 内容是 `Hello Git!`, 那么当我们 add, commit 后, git 的对象构成如下：

```shell
复制编辑
[blob]         存 "Hello Git!"
   ↑
[tree]         记录 hello.txt → blob 的映射
   ↑
[commit]       指向 tree，表示这个版本的快照
```

其中

- **内容（blob）** 存的是 `"Hello Git!"`
- **结构（tree）** 存的是 `"hello.txt → blob_hash"`
- **快照（commit）** 存的是 `"tree_hash + parent_hash + message"`

git 查找文件的顺序, 就是 commit → tree → (子 tree)* → blob.



#### trees

Notice: tree 的目的就是对各个文件夹的相对位置做一个记录以方便从 commit 到 blob 的映射.

为什么不直接使用 commit 到 blob 的映射, 而要用 tree 在中间过渡呢? 因为文件结构可能会很复杂. tree 的存在就像是文件系统的多级 page table, 让查找更加快速并且节省了冗长的目录路径名.  tree 提供了：
- 结构清晰的分层管理
- 可重用性（相同目录结构只存一份）
- 增量更新（某个子目录有变，只需创建新的子 tree）
- 快速 diff（两个 tree 相同就意味着整个子目录一致）

比如项目中的 `src/models/resnet.py` 被改了, 那么只有`resnet.py` 对应的 blob 更新, `models/...` 对应的 tree, `src/...` 对应的 tree 以及 root tree 更新, Git 只需存这些更新部分, 而其他 tree 不变可复用.

我们可以查看 tree 的数量:

```shell
git cat-file --batch-check --batch-all-objects | grep tree | wc -l
```



#### commits

可以将一个 commit 看作:

```c
struct Commit {
    Tree snapshot;
    Commit[] parents;
    Metadata { author, committer, message, timestamp };
}
```

可以用 `git cat-file -p <commit_hash>` 来查看 commit 的具体内容, 比如:

```shell
git cat-file -p 17c2b8a
```

输出可能如下:

```shell
tree e68f7d18b1c442a3f5044ef8aa335d2bd7e9c3f4
parent 98d3a2bc8976e3fd60f4129c97a2c84d3b9a2451
author Qiulin Fan <rynnefan@umich.com> 1715671234 +0800
committer Qiulin Fan <rynnefan@umich.com> 1715671234 +0800

add initial model training script
```



Commit 和 Tree 的连接关系（结构图）

```shell
graph TD
  C1[Commit 1]
  C2[Commit 2]
  C3[Commit 3]
  T1[Tree 1]
  T2[Tree 2]
  T3[Tree 3]
  C1 --> T1
  C2 --> T2
  C3 --> T3
  C2 --> C1
  C3 --> C2
```

每个 commit 指向一个 tree (项目状态), 并且指向一个或多个 parent (历史).