# portal-mulit-cluster-script

#### Description
The Donau Portal enables the Donau Portal to connect to and manage multiple computing clusters of different types. For non-Donau scheduler clusters, third-party scripts need to be adapted based on the Donau rule output. portal-mulit-cluster-script provides the best script for connecting the Donau Portal to an LSF cluster. Currently, the following scripts are supported:

```
LSF Node Information Collection Script: node, nodeSample
LSF Job Information Collection Script : job, jobSample, job_date,jobSample_date
LSF Job Submission Script             : submit
LSF Job Operation Script              : stop, resume, rerun, suspend
LSF Queue Query Script                : query-active
```

**Note**:If the LSB_DISPLAY_YEAR parameter is not configured in the lsf.conf file of the LSF node, you need to modify the script job_date, jobSample_date file name job, jobSample, and overwrite the original script.

#### Software Architecture

Python2/Python3

#### Operation 

1. Download the compressed package from  https://gitee.com/openeuler/portal-mulit-cluster-script and decompress it to the {INSTALL_PATH}/huawei/portal/ac/scripts/scheduler/{SCHEDULER_TYPE}/  directory.

   Note: INSTALL_PATH is the client installation directory and SCHEDULER_TYPE is the scheduler type.

   <table> <tr> <td style='color:#fff;background:black'><pre>[root@client186 scheduler]# pwd</pre>
   <pre>/share/share_lsf/huawei/portal/ac/scripts/scheduler</pre>
   <pre>[root@client186 scheduler]# ll</pre>
   <pre>total 8</pre>
   <pre>drwx--x--x. 6 ccp_master ccs_master 60 Nov 28 17:21 LSF</pre>
   <pre>-r-xr-xr-x. 1 ccp_master ccs_master 592 Nov 28 00:00 post-exec.sh</pre>
   <pre>-r-xr-xr-x. 1 ccp_master ccs_master 673 Nov 28 00:00 pre-exec.sh</pre>
   [root@client186 scheduler]# </td> </tr> </table>

2. Upload the script in the attachment to the corresponding directory, the directory structure is as follows:

   <table> <tr> <td style='color:#fff;background:black'><pre>[root@host34 scheduler]# tree LSF/</pre>
   <pre>LSF/</pre>
   <pre>├── collection
   │   ├── job
   │   └── jobSample
   ├── job
   │   ├── rerun
   │   ├── resume
   │   ├── stop
   │   ├── submit
   │   └── suspend
   ├── node
   │   ├── node
   │   └── nodeSample
   └── queue
       └── query-active </td> </tr> </table>

3. Change the owner of the script to the client installation user with permission 644.

   <table> <tr> <td style='color:#fff;background:black'><pre>[root@host34 scheduler]# cd LSF/</pre>
   <pre>[root@host34 LSF]# ll</pre>
   <pre>total 0</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master  48 Dec  1 09:18 collection</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master 109 Dec  1 09:18 job</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master  50 Dec  1 09:18 node</pre>
   <pre>drwx--x--x. 2 ccp_master ccs_master  33 Dec  1 09:18 queue</pre>
   [root@client186 scheduler]# </td> </tr> </table>

4. Execute the following command to modify the environment variable parameters in the script file:

   ```sh
   sed -i "s#@SCHEDULER_PROFILE_PATH@#/opt/lsf/conf/profile.lsf#g" `grep @SCHEDULER_PROFILE_PATH@ -rl /opt/huawei/portal/ac/scripts/scheduler/LSF`
   ```

   Note: /opt/lsf/conf/profile.lsf is the environment variable path of the LSF scheduler; /opt/huawei/portal/ac/scripts/scheduler/LSF is the script directory of the current client node caller.

5. Modify the fileformat file format of the script file to unix (if dos2unix is installed, you can use dos2unix filename1 filename2 filename3 to convert multiple files, if dos2unix is not installed, you can follow the steps below to modify the file format.)

   a)     vim filename

   b)     Input:set ff=unix，then press Enter

   c)     :wq!Save the file.

#### Instructions

1. Use PuTTY to log in to the Donau Portal Client node as user root.

2. <span id="step2">Switch to the remote cluster script directory.</span>

   **cd** */opt/huawei*/**portal/ac/scripts/scheduler/**scheduler_type

   >**Description**
   >
   >* "/opt/huawei" is the installation path of Donau Portal.
   >* "scheduler_type" is the third-party scheduler type.
   >* Script permissions are all 644.
   >* The script owner and group are Donau Portal operation and maintenance administrators (such as ccp_master) and their user groups.

3. <span id="step3">Integrated job scripts.</span>

   For the description of the input and output parameters of the remote cluster script, please refer to the "Multi-cluster User Guide".

   > **Notice**
   >
   > * Multiple applications will be integrated in the user's business scenario, and the application integration is complex. Donau Portal cannot predict the command input in the script. Users need to ensure the security of the script to prevent the injection of malicious commands.
   > * The scripts under the node directory and collection directory provide host and job information sources in "host", "monitoring center" and "job management". If some fields cannot be collected, "host", "monitoring center", "job management" The information corresponding to this field will cannot be displayed.

4. Use the PuTTY tool to log in to the Donau Portal node as the root user (In an HA scenario, log in to the active and standby Donau Portal nodes). Refer to [Step 2](#step2) [Step 3](#step3) to modify the integration script.

#### Precautions

1.  The current script adaptation is for versions after HPC_23.0.0;
2.  Strictly follow the operation steps, otherwise the script may fail to execute.

#### Contribution

1.  Fork the repository
2.  Create Feat_xxx branch
3.  Commit your code
4.  Create Pull Request


#### Gitee Feature

1.  You can use Readme\_XXX.md to support different languages, such as Readme\_en.md, Readme\_zh.md
2.  Gitee blog [blog.gitee.com](https://blog.gitee.com)
3.  Explore open source project [https://gitee.com/explore](https://gitee.com/explore)
4.  The most valuable open source project [GVP](https://gitee.com/gvp)
5.  The manual of Gitee [https://gitee.com/help](https://gitee.com/help)
6.  The most popular members  [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
