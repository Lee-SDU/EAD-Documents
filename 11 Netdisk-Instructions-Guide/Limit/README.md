### 权限功能及限制

目前网盘资源权限分为正向权限和反权限

#### 正向权限

权限排序：管理>编辑>下载>只读；

- 只读：浏览资源(在线浏览图片、文本文件)

- 下载：只读+下载

- 编辑：下载+编辑

- 管理：编辑+管理(删除)

正向权限对应操作：

- 只读：在线浏览图片、文本文件

- 下载：下载

- 编辑：所有新建类操作、拷贝、移动、设置模板目录、自定义视图和内容的修改、重命名

- 管理：共享、删除

**注:** 如上所述的正向权限，按照从上至下的顺序，下方的权限和操作包含上方权限和操作。如，当用户拥有某个节点的编辑权限，那他同时拥有该节点的下载和只读权限，也就是说，他除了可以操作编辑权限范围的按钮，同时也拥有下载按钮及文件的在线浏览功能。以此类推。

#### 反权限

禁止：禁止访问(系统对授予禁止权限的资源置灰显示)

**注:** 禁止，当对某用户授予某资源的禁止权限后，指该用户对该资源以及该资源以下的所有子资源都无任何正向权限

#### 默认权限
当前网盘默认权限为"下载"，也就是说所有用户都在未被授予其他权限时，登录网盘后默认拥有文件的下载权限。