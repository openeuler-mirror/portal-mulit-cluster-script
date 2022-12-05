# portal-mulit-cluster-script

#### 介绍
多瑙管理平台（Donau Portal）实现了Donau Portal对接和管理多个不同类型计算集群的能力，针对非Donau类型的调度器集群，需要根据Donau的规则输出进行对应的第三方脚本脚本适配。portal-mulit-cluster-script提供了Donau Portal对接LSF类型集群的最佳实际脚本，方便用户参考集成。目前支持的脚本有：

````
LSF节点信息采集脚本: node,nodeSample
LSF作业信息采集脚本: job,jobSample,job_date,jobSample_date  
LSF作业提交脚本: submit
LSF作业操作脚本: stop, resume,rerun,suspend
LSF队列查询脚本: query-active
````

**注意**：如果LSF节点lsf.conf文件中未配置LSB_DISPLAY_YEAR参数，则需要修改脚本job_date,jobSample_date文件名为job,jobSample，覆盖原脚本。

#### 软件架构
Python2/Python3


#### 操作教程

1. 从网址 https://gitee.com/openeuler/portal-mulit-cluster-script 下载压缩包, 解压至{INSTALL_PATH}/huawei/portal/ac/scripts/scheduler/{SCHEDULER_TYPE}/目录下；

   注：INSTALL_PATH为client安装目录，SCHEDULER_TYPE为调度器类型

   <table> <tr> <td style='color:#fff;background:black'><pre>[root@client186 scheduler]# pwd</pre>
   <pre>/share/share_lsf/huawei/portal/ac/scripts/scheduler</pre>
   <pre>[root@client186 scheduler]# ll</pre>
   <pre>total 8</pre>
   <pre>drwx--x--x. 6 ccp_master ccs_master 60 Nov 28 17:21 LSF</pre>
   <pre>-r-xr-xr-x. 1 ccp_master ccs_master 592 Nov 28 00:00 post-exec.sh</pre>
   <pre>-r-xr-xr-x. 1 ccp_master ccs_master 673 Nov 28 00:00 pre-exec.sh</pre>
   [root@client186 scheduler]# </td> </tr> </table>

2. 将附件中的脚本上传到对应的目录中，目录结构如下:

   <table> <tr> <td style='color:#fff;background:black'><pre>[root@host34 scheduler]# tree LSF/</pre>
   <pre>LSF/</pre>
   <pre>├── collection
   │   ├── job.sample
   │   └── jobSample.sample
   ├── job
   │   ├── rerun.sample
   │   ├── resume.sample
   │   ├── stop.sample
   │   ├── submit.sample
   │   └── suspend.sample
   ├── node
   │   ├── node.sample
   │   └── nodeSample.sample
   └── queue
       └── query-active.sample </td> </tr> </table>

3. 更改脚本的属主为client安装用户，权限为644

   <table> <tr> <td style='color:#fff;background:black'><pre>[root@host34 scheduler]# cd LSF/</pre>
   <pre>[root@host34 LSF]# ll</pre>
   <pre>total 0</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master  48 Dec  1 09:18 collection</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master 109 Dec  1 09:18 job</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master  50 Dec  1 09:18 node</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master  33 Dec  1 09:18 queue</pre>
   [root@client186 scheduler]# </td> </tr> </table>

4. 执行以下命令，修改脚本文件中的环境变量参数

   ```sh
   sed -i "s#@SCHEDULER_PROFILE_PATH@#/opt/lsf/conf/profile.lsf#g" `grep @SCHEDULER_PROFILE_PATH@ -rl /opt/huawei/portal/ac/scripts/scheduler/LSF`
   ```

   注：/opt/lsf/conf/profile.lsf为LSF调度器环境变量路径；/opt/huawei/portal/ac/scripts/scheduler/LSF为当前client节点调取器脚本目录

5. 修改脚本文件的fileformat文件格式为unix（如果安装了dos2unix，可以使用dos2unix filename1 filename2 filename3转换多个文件，若未安装dos2unix，可按照下面步骤修改文件格式）

   a)     vim filename

   b)     输入:set ff=unix，然后回车

   c)     :wq!保存文件

#### 使用说明

1. 使用PuTTY工具，以root用户登录Donau Portal Client节点。

2. <span id="step2">切换至远程集群脚本目录。</span>

   **cd** */opt/huawei*/**portal/ac/scripts/scheduler/**scheduler_type

   >**说明**
   >
   >*  “/opt/huawei”为Donau Portal安装路径。
   >*  “scheduler_type”为第三方调度器类型。
   >*  脚本权限均为644。
   >*  脚本属主和属组均为Donau Portal运维管理员（如ccp_master）及其用户组。

3. <span id="step3">集成作业脚本。</span>

   远程集群脚本输入输出参数说明请参见《多集群使用指导说明书》。

   >**须知**
   >
   >* 用户业务场景中会集成多种应用，且应用集成较复杂，Donau Portal无法预测脚本中的命令输入，用户需确保脚本的安全性，防止恶意命令的注入。
   >* node目录和collection目录下脚本提供“主机”、“监控中心”、“作业管理”中主机和作业信息来源，若某些字段采集不到，则“主机”、“监控中心”、“作业管理”中该字段对应信息将无法显示。

4. 使用PuTTY工具，以root用户登录Donau Portal节点（若为HA场景，需登录主备Donau Portal节点）。参考[步骤2](#step2)[步骤3](#step3)修改集成脚本。

#### 注意事项

1. 当前脚本适配是针对HPC_22.0.0之后的版本；
2. 严格按照操作步骤执行，否则可能会导致脚本执行失败

#### 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码
4.  新建 Pull Request


#### 特技

1.  使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md
2.  Gitee 官方博客 [blog.gitee.com](https://blog.gitee.com)
3.  你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解 Gitee 上的优秀开源项目
4.  [GVP](https://gitee.com/gvp) 全称是 Gitee 最有价值开源项目，是综合评定出的优秀开源项目
5.  Gitee 官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6.  Gitee 封面人物是一档用来展示 Gitee 会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
