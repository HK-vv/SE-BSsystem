# BrainStorm 知识竞赛系统用户使用手册

## 1 版本历史

| 日期 | 版本 | 人员 | 更新内容 |
| ---- | ---- | ---- | -------- |
| 22.05.14 | v0.0 | 李昀泽 | 创建手册文档及目录;编写概述部分 |
| 22.05.24 | v0.1 | 李昀泽 | 编写管理员使用说明中个人修改、管理员信息管理、题库管理部分 |
| 22.05.24 | v0.2 | 刘硕 | 编写管理员使用说明中登录、登出、小程序用户信息管理、比赛管理部分 |

## 2 概述

### 2.1 文档概述

#### 2.1.1 编写目的

根据课程组的总体要求, 和小组的相关讨论, 我们编写本用户使用手册.

本手册为BrainStorm项目团队为团队开发的"BrainStorm知识竞赛系统"编写的用户使用手册.本手册面向的群体为软件工程课程组老师和助教及所有使用本竞赛系统的用户.

编者希望通过该使用手册能够使用户快速了解这个系统的基本情况及使用方法。

#### 2.1.2 项目背景

知识的重要性不言而喻. 人类几千年的历史虽创造出极其丰富的物质基础, 但各种科学/哲学提炼出的知识才是推动发展的核心因素. 无论是出于欣赏或功利, 知识总是被大众广泛喜爱的, 拥有知识的人更是如此. 人们总是喜欢把事物排个名次, 决个高低, 拥有知识的人也不例外.

古代有科举, 现代有考试. 受利于科技发展, 试卷和考场已经不必要了, 一个线上的知识竞赛系统就可以满足相当多的需求. 然而, 虽然当前存在很多公开的知识竞赛软件, 但其包含的题目通常已经被创建者固定, 而难以应用于诸如兴趣社团这样有一定规模但又不具有软件开发能力的中小群体. 因此, 我们计划开发BrainStorm知识竞赛系统, 除知识竞赛的基本功能外, 提供题库管理和比赛创建功能, 来满足上述群体的需要. 

#### 2.1.3 定义

BrainStorm：BrainStorm知识竞赛系统

#### 2.1.4 参考资料

用户使用说明书(示例), 北航软工课程组.

### 2.2 系统概述

#### 2.2.1 目标

BrainStorm的目标是搭建一个知识竞赛系统, 为中小用户群体的题目定制和比赛需求提供支持.

一个目标应用场景是: 学校内有很多社团, 每个社团都可以向本系统添加题目, 开放比赛. 学校里的同学可以自己练习, 以及参加比赛提升rating. rating可以作为学生的某种课外活动量化指标.

#### 2.2.2 功能

```sequence
participant 超级管理员 as spmg
participant 管理员 as mg
participant 题库及后台 as tk
participant 用户 as us
mg->tk: 管理题目
note right of mg: 题库管理功能
spmg->mg: 管理
mg->us: 管理
note right of mg: 用户管理功能
tk->us: 随机抽题练习
note left of us: 自主练习功能
mg->tk: 选题创建比赛并发布
tk->us: 参加比赛
note left of tk: 比赛创建及参与功能
```

## 3 运行环境要求

### 3.1 管理端

使用chrome72.0以上版本，Edge44以上版本，即可正常使用本系统

### 3.2 用户端

iOS 8.0.18及以上版本，Android 8.0.19及以上版本微信客户端

## 4 管理端使用说明

### 4.1 网站访问

管理端网站的地址为http://114.116.218.125/index.html#/

### 4.2 登入

#### 4.2.1 说明

管理员进入系统需同时验证用户名和密码，用户名由超级管理员创建，初始密码为123456。为安全考虑，**建议初次登录系统后立即修改个人密码**。用户名和密码匹配时，将提示登录成功；否则提示用户名或密码错误，且不能进入系统，可以联系超级管理员创建账号或重置密码。

#### 4.2.2 举例

<img src="./img/UM_4.2.2.png" alt="UM_4.2.2" style="zoom:70%;" />

如上图所示，正确填写用户名和密码后，点击登录即可进入系统。当系统提示如下信息时，表示用户名和密码不匹配，原因是没有当前账号或密码错误，可以联系超级管理员创建账号或重置密码。

<img src="./img/UM_4.2.2_2.png" alt="UM_4.2.2_2" style="zoom:70%;" />

### 4.3 个人信息修改

#### 4.3.1 说明

在该系统中登录后的任意界面，点击顶栏右侧的灰色个人中心按钮或按钮右侧用户名显示区域，即可进入个人信息修改界面。

***注：用户名显示区域右上角小红点代表该账号信息未完善，填写邮箱和联系电话信息后，小红点将消失**

个人信息修改界面中共包含5个输入框，可满足（超级）管理员修改用户名、邮箱、联系电话、密码中任一或多个信息的要求。填写后，系统会对其格式进行检查，如果格式有误会在输入框下进行提示。如果所填用户名与其他用户的用户名相同将提示“用户名已存在”。具体约束条件，将在4.3.2输入约束中详细介绍。

***注：（超级）管理员表示超级管理员和（或）管理员，下同**

#### 4.3.2 输入约束

用户名：3-20个字符

邮箱：需为正确邮箱格式

联系电话：需为正确的11位手机号码

密码：6-20个字符，必须包含大写字母、小写字母、数字及特殊字符

确认密码：需与密码栏填写内容保持一致

#### 4.3.3 举例

<img src=".\img\user_modify.jpg" alt="user_modify" style="zoom: 50%;" />

上图为个人信息修改界面，（超级）管理员填写个人信息且输入框下无格式错误提示后，点击确认按钮即可完成修改。当出现如下错误提示时只需按要求修改信息，并再次点击确认按钮就可。

<img src=".\img\user_modify_e.jpg" alt="user_modify_e" style="zoom:50%;" />

<img src=".\img\user_modify_e2.jpg" alt="user_modify_e2" style="zoom:50%;" />

<img src=".\img\UM_4.1.1.2_e1.jpg" alt="UM_4.1.1.2_e1" style="zoom:50%;" />

### 4.4 用户信息管理

该模块包含管理员信息管理和小程序用户信息管理两部分，点击左侧导航栏的“用户信息管理”按钮将默认进入管理员信息管理界面，可通过页面上方按钮实现页面切换。

#### 4.4.1 管理员信息管理

该部分将展示所有BrainStorm系统的超级管理员及管理员信息。

##### 4.4.1.1 管理员信息(主界面)

###### 4.4.1.1.1 说明

表格直接展示（超级）管理员的用户名、邮箱、联系电话、管理员类型。管理员类型显示“super”为超级管理员，显示“admin”为普通管理员，顶部可根据用户名查找（超级）管理员。底部显示当前（超级）管理员账号数量，用户还可调整每页显示信息数量。对于超级管理员将展示查找输入框右侧“创建管理员”按钮及表格最右侧两列操作按钮，分别用于重置管理员密码和删除管理员账号。

###### 4.4.1.1.2 举例

超级管理员视图：

<img src=".\img\UM_4.4.1.1.jpg" alt="UM_4.4.1.1" style="zoom:50%;" />

管理员视图：

<img src=".\img\UM_4.4.1.1_2.jpg" alt="UM_4.4.1.1_2" style="zoom:50%;" />

##### 4.4.1.2 创建管理员

###### 4.4.1.2.1 说明

此功能仅超级管理员有权限使用。

点击主界面上方”创建管理员“按钮即可进入该界面。

界面中包含用户名输入框，以及默认密码为123456的提示信息。超级管理员仅需在用户名输入框中填写填写后，系统会对其格式进行检查，如果格式有误会在输入框下进行提示。如果所填用户名与其他用户的用户名相同将提示“用户名已存在”。具体约束条件，将在4.4.1.2.2输入约束中详细介绍。

###### 4.4.1.2.2 输入约束

用户名：3-20个字符

###### 4.4.1.2.3 举例

<img src=".\img\UM_4.1.1.2.jpg" alt="UM_4.1.1.2" style="zoom:50%;" />

上图为创建管理员界面，超级管理员填写要创建的管理员账户用户名且输入框下无格式错误提示后，点击确认按钮即可创建。当出现如下错误提示时只需按要求修改信息，并再次点击确认按钮就可。

<img src=".\img\UM_4.1.1.2_e.jpg" alt="UM_4.1.1.2_e" style="zoom:50%;" />

<img src=".\img\UM_4.1.1.2_e1.jpg" alt="UM_4.1.1.2_e1" style="zoom:50%;" />

<img src=".\img\UM_4.1.1.2_e2.jpg" alt="UM_4.1.1.2_e2" style="zoom:50%;" />

##### 4.4.1.3 重置管理员密码

###### 4.4.1.3.1 说明

此功能仅超级管理员有权限使用。

点击主界面对应（超级）管理员信息右侧的黄色重置密码按钮，并在弹出的提示框中点击确认按钮，即可重置相应（超级）管理员的密码。

###### 4.4.1.3.2 举例

下图为提示框界面，如确定要重置该用户密码，则点击"确定"键，否则点击"取消"键或右上角"x"

<img src=".\img\UM_4.4.1.3.jpg" alt="UM_4.4.1.3" style="zoom:50%;" />

##### 4.4.1.4 删除管理员

###### 4.4.1.4.1 说明

此功能仅超级管理员有权限使用。

点击主界面对应（超级）管理员信息右侧的红色删除按钮，并在弹出的提示框中点击确认按钮，即可删除相应（超级）管理员的账号。

###### 4.4.1.4.2 举例

下图为提示框界面，如确定要删除该用户，则点击"确定"键，否则点击"取消"键或右上角"x"

<img src=".\img\UM_4.4.1.4.jpg" alt="UM_4.4.1.4" style="zoom:50%;" />

#### 4.4.2 小程序用户信息管理

该部分将展示所有使用BrainStorm系统小程序端的用户信息。

##### 4.4.2.1 用户信息(主界面)

###### 4.4.2.1.1 说明

表格直接展示小程序用户的用户名、rating和参赛次数，可以选择根据rating的升序或降序展示信息，顶部可根据用户名查找用户。

###### 4.4.2.1.2 举例

<img src="./img/UM_4.4.2.1.png" alt="UM_4.4.2.1" style="zoom:70%;" />

##### 4.4.2.2 用户比赛表现

###### 4.4.2.2.1 说明

点击主界面“比赛表现”列的“详情”按钮，可展示该用户参加过的所有比赛，也可查看比赛是否rating、用户得分、用时、排名、赛前rating和赛后rating的信息。点击用户答题情况按钮可以看到用户在该场比赛中具体的答题情况。

###### 4.4.2.2.2 举例

下图展示用户名为“涵语十级大师🏆”的用户的往期比赛表现。

<img src="./img/UM_4.4.2.2.jpg" alt="UM_4.4.2.2" style="zoom:67%;" />

下图为用户“Ariza”在比赛《员工试炼赛2》中的答题情况，左侧展示整体答题情况，绿色代表正确，红色代表错误，黄色为超时，灰色为未提交。点击题号可在右侧查看该题的详细信息，包括题号、题目id、题目类型、题目标签、题目作者、题目描述、题目选项、正确答案、用户作答状态、用户提交和是否正确，其中题目选项字段仅在题目类型为单选题或多选题时显示，用户提交和是否正确字段仅在用户作答状态为有效提交时显示。

<img src="./img/UM_4.4.2.2_2.png" alt="UM_4.4.2.2" style="zoom:67%;" />

### 4.5 题库管理

#### 4.5.1 查看所有题目(主界面)

##### 4.5.1.1 说明

该部分展示题库中所有题目信息，包括题目id、题目类型、题目标签、题目描述、选项、答案、公开性。（超级）管理员可以根据题目类型、题目标签、公开性进行筛选，也可以利用顶部搜索框通过题目描述或题目作者查找题目，还可以勾选右上角“仅显示自创题目”来筛选自己添加的题目。

题目id唯一标识每一道题目。题目类型共**4**种，分别为单选题、多选题、判断题、填空题。公开性共**2**种，分别为公开、未公开，仅公开的题目可在小程序端通过随机组卷进行练习。

表格最左侧为题目勾选框，用于勾选要公开/删除的题目，最右侧为编辑题目按钮(详见4.5.3)，界面左上角有绿、黄、红三个按钮，分别为添加题目按钮(详见4.5.2)、批量公开题目按钮(详见4.5.4)、批量删除题目按钮(详见4.5.5)。

##### 4.5.1.2 举例

#### <img src=".\img\UM_4.5.1.jpg" alt="UM_4.5.1" style="zoom: 25%;" />

#### 4.5.2 添加题目

点击题库管理主界面左上角的绿色添加题目按钮，即可进入该界面。该界面包含单个添加和批量添加两部分，可通过界面上方按钮实现界面切换。

##### 4.5.2.1 单个添加

###### 4.5.2.1.1 说明

单个添加题目界面根据所选题目类型会有相应的变化，其中所有类型题目均可见的为题目类型选项框、题目标签选项框、编辑标签库按钮（仅超级管理员可见）、题目描述输入框、题目答案输入框（或选项按钮）、控制题目是否公开的开关。对于单选/多选类型题目，会额外显示A、B、C、D四个选项的内容输入框。对于判断题，题目答案为正确/错误的二选一单选按钮；对于其他类型题目，题目答案为输入框。

填写后，系统会对其格式进行检查，如果格式有误会在输入框下进行提示。具体约束条件，将在4.5.2.1.2输入约束中详细介绍。

###### 4.5.2.1.2 输入约束

| 题目类型 | 单选题                                               | 多选题                                                       | 填空题                                                       | 判断题                    |
| -------- | ---------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------- |
| 题目标签 | 可从标签库中选择0~N个标签，N为当前标签库中标签数量） | 可从标签库中选择0~N个标签                                    | 可从标签库中选择0~N个标签                                    | 可从标签库中选择0~N个标签 |
| 题目描述 | 不超过150个字符且不为空                              | 不超过150个字符且不为空                                      | 不超过150个字符且不为空                                      | 不超过150个字符且不为空   |
| A选项    | 不超过25字符且不为空                                 | 不超过25字符且不为空                                         | -                                                            | -                         |
| B选项    | 不超过25字符且不为空                                 | 不超过25字符且不为空                                         | -                                                            | -                         |
| C选项    | 不超过25字符且不为空                                 | 不超过25字符且不为空                                         | -                                                            | -                         |
| D选项    | 不超过25字符且不为空                                 | 不超过25字符且不为空                                         | -                                                            | -                         |
| 题目答案 | 有且仅有一个字符，并且在`A`、`B`、`C`、`D`之中       | 有且不超过4个字符，这些字符分别在`A`、`B`、`C`、`D`之中，且不重复 | 不超过80字符，且不为空；若有多个答案，用逗号(`,`或`，`)分开，若一个空有多种答案，用`/`分开 | 有且仅为`正确`或`错误`    |
| 是否公开 | 有且仅为`公开`或`不公开`                             | 有且仅为`公开`或`不公开`                                     | 有且仅为`公开`或`不公开`                                     | 有且仅为`公开`或`不公开`  |

###### 4.5.2.1.3 举例

题目类型为单选：

<img src=".\img\UM_4.5.2.1_1.jpg" alt="UM_4.5.2.1_1" style="zoom: 33%;" />

题目类型为多选：

<img src=".\img\UM_4.5.2.1_2.jpg" alt="UM_4.5.2.1_2" style="zoom: 33%;" />

题目类型为判断：

<img src=".\img\UM_4.5.2.1_3.jpg" alt="UM_4.5.2.1_3" style="zoom:33%;" />

题目类型为填空：

<img src="E:\SE\SE-BSsystem\img\UM_4.5.2.1_4.jpg" alt="UM_4.5.2.1_4" style="zoom: 33%;" />

##### 4.5.2.2 批量添加

###### 4.5.2.2.1 说明

批量添加题目界面支持通过上传excel表格文件的方式批量添加题目。每个excel文件所包含的题目要添加标签需相同（或不添加），每次只支持上传一个文件。若需要添加不同的标签需分开放在不同的文件内多次上传。

批量添加题目界面左侧为标签选项框和编辑标签库按钮（按钮仅超级管理员可见），选项框下面为题目模板下载链接和文件格式约束说明下载链接。界面右侧为题目文件上传区域，每次仅可上传一个文件，文件格式需为.xls或.xlsx，文件大小限制为1MB以内。

填写后，系统会对其格式进行检查，如果格式有误会进行提示。

###### 4.5.2.2.2 输入约束

题目标签：可从标签库中选择0~N个标签作为本次要添加的所有题目的标签。

题目文件填写格式要求请通过该界面的题目模板下载链接和文件格式约束说明下载链接自行下载查看。

###### 4.5.2.2.3 举例

<img src="E:\SE\SE-BSsystem\img\UM_4.5.2.2.jpg" alt="UM_4.5.2.2" style="zoom:33%;" />

#### 4.5.3 编辑题目信息

##### 4.5.3.1 说明

点击主界面表格最右侧编辑题目按钮，即可修改相应题目的所有信息。

编辑题目信息界面与单个添加题目界面基本一致。

***注：管理员只能修改自己创建的题目，不能修改其他管理员创建的题目。超级管理员可以修改所有题目。**

##### 4.5.3.2 输入约束

同4.5.2.1.2

***注：对于已公开的题目不支持修改为未公开状态。**

##### 4.5.3.3 举例

同4.5.2.1.3

#### 4.5.4 批量公开题目

##### 4.5.4.1 说明

在题库管理主界面对希望公开的题目进行勾选，再点击界面左上方的黄色题目批量公开按钮，即可批量公开所有勾选的题目。

***注：管理员只能公开自己创建的题目，不能公开其他管理员创建的题目。超级管理员可以公开所有题目。**

#### 4.5.5 批量删除题目

##### 4.5.5.1 说明

在题库管理主界面对希望删除的题目进行勾选，再点击界面左上方的红色题目批量删除按钮，即可批量删除所有勾选的题目。

***注：管理员只能删除自己创建的题目，不能删除其他管理员创建的题目。超级管理员可以删除所有题目。**

#### 4.5.6 编辑标签库

##### 4.5.6.1 说明

此功能仅超级管理员有权限使用。

在单个添加/批量添加/编辑题目界面，点击”编辑标签库“按钮即可进入该界面。

界面中包含标签库中现有标签展示及“+ New Tag”按钮，可点击该按钮后输入要添加的标签名，即可添加标签。点击要删除的标签后面的"x"即可删除该标签。

##### 4.5.6.2 举例

<img src=".\img\UM_4.5.6.jpg" alt="UM_4.5.6" style="zoom:50%;" />

### 4.6 比赛管理

#### 4.6.1 查看所有比赛(主界面)

##### 4.6.1.1 说明

该部分展示所有已创建的比赛，包括比赛名称、比赛状态、比赛最早开始时间、比赛最晚开始时间、是否为公开赛等信息。管理员可以通过比赛状态筛选，也可以利用顶部搜索框通过比赛名称查找比赛，还可以勾选右上角“仅显示自创比赛”来筛选自己创建的比赛。

其中比赛最早开始时间和最晚开始时间为限制小程序用户**进入比赛**的时间，在该时间段内进入比赛均可。

比赛状态共**5**个，按比赛流程分别为未开始、比赛中、截止进入比赛、待公布成绩、已结束，需要说明的是，截止进入比赛表示用户无法进入比赛答题但此时未完成答题的用户仍可继续答题，直至完成所有题目，待公布成绩表示所有用户均已完成答题，管理员可以公布成绩。

##### 4.6.1.2 举例

<img src="./img/UM_4.6.1.2.png" alt="UM_4.6.1.2" style="zoom:67%;" />

#### 4.6.2 创建比赛

##### 4.6.2.1 说明

在主界面左上角点击创建比赛按钮进入创建比赛界面。

创建比赛分为两步：

第一步是比赛常用设置，包括比赛名称、最早开始时间、最晚开始时间、各类型题目时间限制、比赛是否rating、是否为公开赛和比赛密码。若选择rating，则参与该比赛的用户rating将在比赛公布结果后根据用户作答情况相应增加或减少。公开赛无需设置密码，所有用户都可以直接注册；非公开赛需要管理员设置比赛密码，用户输入正确密码方可完成注册。

第二步是选择赛题，该界面展示所有已在题库中的题目，包括题目类型、题目描述、题目选项等信息。可以通过题目类型、题目标签、题目公开性筛选题目，也可以利用顶部搜索框根据题目描述或题目作者查找题目，还可以勾选右上角“仅显示自创题目”筛选自己导入的题目。点击题目的勾选框可以将题目加入比赛中，已选择的题目数量和题目id会显示在底部。若表格上方“是否有序勾选”为“是”，则比赛时的题目顺序为勾选顺序，否则，将以随机顺序作为题目顺序。

##### 4.6.2.2 输入约束

所有比赛设置均为必填项。

比赛名称：字符长度不得超过30

最早开始时间：不得早于当前时间

最晚开始时间：不得早于最早开始时间

各题型时间限制：时间限制需在5~600秒之间。为使用户得分均匀分布，建议将时限设置为用户平均用时的2倍。

比赛密码：6-20个字符

赛题数量：1-30道题目

##### 4.6.2.3 举例

常用设置界面：

<img src="./img/UM_4.6.2.3_1.jpg" alt="UM_4.6.2.3_1" style="zoom:67%;" />

选择赛题界面：

<img src="./img/UM_4.6.2.3_2.jpg" alt="UM_4.6.2.3_2" style="zoom:67%;" />

异常情况：

比赛名称输入异常：未输入比赛名称/比赛名称超过最大限度

<img src="./img/UM_4.6.2.3_4.png" alt="UM_4.6.2.3_4" style="zoom:67%;" />

<img src="./img/UM_4.6.2.3_5.png" alt="UM_4.6.2.3_5" style="zoom:67%;" />

比赛时间设置异常：未设置比赛时间/最早开始时间早于当前时间/最晚开始时间早于最早开始时间

<img src="./img/UM_4.6.2.3_7.png" alt="UM_4.6.2.3_7" style="zoom:67%;" />

<img src="./img/UM_4.6.2.3_3.png" alt="UM_4.6.2.3_3" style="zoom:67%;" />

<img src="./img/UM_4.6.2.3_6.png" alt="UM_4.6.2.3_6" style="zoom:67%;" />

比赛密码异常：未设置比赛密码

<img src="./img/UM_4.6.2.3_8.png" alt="UM_4.6.2.3_8" style="zoom:67%;" />

选择题目异常：未选择题目/题目数量超过最大限度

<img src="./img/UM_4.6.2.3_9.png" alt="UM_4.6.2.3_9" style="zoom:67%;" />

<img src="./img/UM_4.6.2.3_10.png" alt="UM_4.6.2.3_10" style="zoom:67%;" />

#### 4.6.3 查看/修改比赛信息

##### 4.6.3.1 说明

点击主界面查看/修改列按钮，可以查看或修改比赛的所有信息。对于未开始的比赛，管理员可以修改比赛的信息。对于其他状态的比赛仅提供信息查看功能。

***注：管理员只能查看/修改自己创建的比赛，不能查看/修改其他管理员创建的比赛信息。超级管理员可以查看/修改所有比赛的所有信息。**

##### 4.6.3.2 输入约束

同4.6.2.2

##### 4.6.3.3 举例

查看比赛时，界面中所有字段均不可编辑。

<img src="./img/UM_4.6.3.3_1.png" alt="UM_4.6.3.3_1" style="zoom:67%;" />

修改比赛需要按约束。

<img src="./img/UM_4.6.3.3_2.png" alt="UM_4.6.3.3_2" style="zoom:67%;" />

#### 4.6.4 删除比赛

##### 4.6.4.1 说明

点击主界面删除列按钮，可以删除已创建的比赛。管理员只能删除未开始的比赛，对于其他状态的比赛不提供删除功能。

***注：管理员只能删除自己创建的比赛，不能删除其他管理员创建的比赛。超级管理员可以删除所有未开始的比赛。**

##### 4.6.4.2 举例

点击状态为未开始比赛的删除按钮

<img src="./img/UM_4.6.4.2_1.png" alt="UM_4.6.4.2_1" style="zoom:67%;" />

点击提示框中的确定即可将比赛删除。

<img src="./img/UM_4.6.4.2_2.JPG" alt="UM_4.6.4.2_2" style="zoom:67%;" />

#### 4.6.5 查看比赛分析

##### 4.6.5.1 说明

点击主界面分析列按钮将跳转至比赛分析界面，管理员可以在比赛的任何阶段实时查看比赛情况。该界面可以查看比赛排行榜和比赛统计分析。

排行榜展示所有已注册比赛的小程序用户的排名、用户名、用时、得分、正确题数、赛前rating、rating变化值等信息。用户每道题的答题情况用四种颜色表示：绿色代表正确、红色代表错误、黄色代表超时、灰色代表未作答，管理员可以提通过点击答题情况按钮查看用户答题情况的具体信息。该界面也提供比赛总人数、最高分、最低分、平均分的信息。

统计分析界面展示三张统计图：用户参与情况饼图、成绩分布柱状图和每题答题情况柱状图。

##### 4.6.5.2 举例

比赛排行榜：

<img src="./img/UM_4.6.5.2_1.png" alt="UM_4.6.5.2_1" style="zoom:67%;" />

比赛统计分析：

<img src="./img/UM_4.6.5.2_2.png" alt="UM_4.6.5.2_2" style="zoom:67%;" />

#### 4.6.6 公布比赛成绩

##### 4.6.6.1 说明

对于管理员自己创建的比赛，应及时公布处于“待公布成绩”状态的比赛，以便小程序用户查看自己在比赛中的答题情况和比赛排行榜前三名。点击公布后系统将以提示框确认，框内同时展示该次比赛在创建时的rating选项。对于不计入rating的比赛，公布成绩时不能修改“是否rating”选项；对于比赛设置为计入rating的比赛，管理员有权修改其为不计入rating。建议管理员在公布成绩时不修改此选项，仅在如比赛事故等特殊情况下酌情修改。

***注：管理员只能公布自己创建的比赛成绩，不能公布其他管理员创建的比赛成绩。超级管理员可以公布所有比赛成绩。**

##### 4.6.6.2 举例

若比赛在创建时设置为不计入rating，则公布比赛时不可勾选计入rating。

<img src="./img/UM_4.6.6.2_1.JPG" alt="UM_4.6.6.2_1" style="zoom:67%;" />

创建比赛时设置为计入rating，则公布比赛时是否计入rating是可选的：

<img src="./img/UM_4.6.6.2_2.png" alt="UM_4.6.6.2_2" style="zoom:67%;" />

### 4.7 登出

#### 4.7.1 说明

在该系统中的任意界面，点击顶栏右侧的红色退出按钮即可安全退出系统。

#### 4.7.2 举例

成功退出账号将显示下图提示，并跳转至登录界面。

<img src="./img/UM_4.7.2.JPG" alt="UM_4.6.4.2_2" style="zoom:67%;" />

## 5 用户端使用说明

### 5.1个人信息与登录

#### 5.1.1说明

在TabBar **“我”** 的页面点击**点我登录**图标并获取相应权限即可登录。点击昵称即可修改昵称。

#### 5.1.2举例

登录

<img src="./img/UM_5.1.2.1.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

修改昵称

<img src="./img/UM_5.1.2.2.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

### 5.2练习

#### 5.2.1说明

在TabBar **“排行榜”** 页面点击**练习一下** 图标即可进入练习界面，选择好数量与标签点击**开始练习**即可进入答题页面。答题结束后可点击题目序号查看答案与正确情况。

#### 5.2.2 举例

练习入口

<img src="./img/UM_5.2.2.1.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

标签与数量选择

<img src="./img/UM_5.2.2.2.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

答题页面

<img src="./img/UM_5.2.2.3.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

结束页面

<img src="./img/UM_5.2.2.4.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

回顾答题情况

<img src="./img/UM_5.2.2.5.1.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

<img src="./img/UM_5.2.2.5.2.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

### 5.3比赛

#### 5.3.1说明

在TabBar **“比赛”**界面或TabBar “**我**”界面的**已参加的比赛**，点击比赛即可注册未注册的比赛、开始进行中的比赛、查看已结束的比赛的结果。在答题结束前退出，且还在答题时间内，可点击**继续比赛**的按钮继续比赛。查看结果可点击题目序号查看答案与正确情况。

#### 5.3.2举例

注册比赛

<img src="./img/UM_5.3.2.1.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

开始比赛

<img src="./img/UM_5.3.2.2.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

开始答题

<img src="./img/UM_5.3.2.3.jpg" alt="UM_4.6.6.2_2" style="zoom:67%;" />

继续比赛

<img src="./img/UM_5.3.2.4.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

答题结束

<img src="./img/UM_5.3.2.5.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

查看结果

<img src="./img/UM_5.3.2.6.1.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

<img src="./img/UM_5.3.2.6.2.jpg" alt="UM_4.6.6.2_2" style="zoom:30%;" />

## 6 附录与其他