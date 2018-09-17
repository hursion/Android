## cts环境配置遇到aapt失败问题：
cts 初始化阶段，aapt安装apk失败，host端吐出log如下：
错误关键“apt: error while loading shared libraries: libc++.so”

```
09-17 13:45:17 D/RunUtil: [aapt, dump, badging, /home/local/SPREADTRUM/hursion.zhang/workspace/google_test/CTS/android-cts-8.1_r8-linux_x86-x86/android-cts/tools/../../android-cts/testcases/CtsSystemUiTestCases.apk] command failed. return code 127
09-17 13:45:17 E/AaptParser: aapt dump badging stderr: aapt: error while loading shared libraries: libc++.so: cannot open shared object file: No such file or directory

09-17 13:45:17 E/AaptParser: Failed to run aapt on /home/local/SPREADTRUM/hursion.zhang/workspace/google_test/CTS/android-cts-8.1_r8-linux_x86-x86/android-cts/tools/../../android-cts/testcases/CtsSystemUiTestCases.apk. stdout: 
09-17 13:45:17 E/ModuleDef: TargetSetupError in preparer: com.android.tradefed.targetprep.suite.SuiteApkInstaller
09-17 13:45:17 E/TestInvocation: Unexpected exception when running invocation: java.lang.RuntimeException: com.android.tradefed.targetprep.TargetSetupError: apk installed but AaptParser failed [QXP2I7091D600080 sp9853i_1h10:U6B OPM2.171019.012]
09-17 13:45:17 E/TestInvocation: com.android.tradefed.targetprep.TargetSetupError: apk installed but AaptParser failed [QXP2I7091D600080 sp9853i_1h10:U6B OPM2.171019.012]
java.lang.RuntimeException: com.android.tradefed.targetprep.TargetSetupError: apk installed but AaptParser failed [QXP2I7091D600080 sp9853i_1h10:U6B OPM2.171019.012]
	at com.android.compatibility.common.tradefed.testtype.ModuleDef.runPreparerSetup(ModuleDef.java:354)
	at com.android.compatibility.common.tradefed.testtype.ModuleDef.runPreparerSetups(ModuleDef.java:288)
	at com.android.compatibility.common.tradefed.testtype.ModuleDef.run(ModuleDef.java:250)
	at com.android.compatibility.common.tradefed.testtype.CompatibilityTest.run(CompatibilityTest.java:477)
	at com.android.tradefed.invoker.TestInvocation.runTests(TestInvocation.java:796)
	at com.android.tradefed.invoker.TestInvocation.prepareAndRun(TestInvocation.java:471)
	at com.android.tradefed.invoker.TestInvocation.performInvocation(TestInvocation.java:322)
	at com.android.tradefed.invoker.TestInvocation.invoke(TestInvocation.java:984)
	at com.android.tradefed.command.CommandScheduler$InvocationThread.run(CommandScheduler.java:558)
```


按照官方文档介绍，cts测试需要配置adb 和aapt环境，原文如下

ADB and AAPT

Before running the CTS, make sure you have recent versions of both Android Debug Bridge (adb) and Android Asset Packaging Tool (AAPT) installed and those tools' location added to the system path of your machine.

To install ADB, download the Android SDK Tools package for your operating system, open it, and follow the instructions in the included README file. For troubleshooting information, see Installing the Stand-alone SDK Tools.

Ensure adb and aapt are in your system path. The following command assumes you've opened the package archive in your home directory:

`export PATH=$PATH:$HOME/android-sdk-linux/build-tools/<version>`
  
  Note: Please ensure your starting path and directory name are correct.
  
  以上主要是需要把aapt加入环境变量中，unisoc之前的方法是直接将aapt复制到/usr/bin中执行，没有配置环境变量，但是在build-tools/28.0.2中遇到上述问题，地位aapt执行共享库出问题，cd 到build-tools/28.0.2单独执行./aapt成功。
  至此，可以断定aapt需要 libc++.so加入环境变量。
  于是执行解决.可加入.bashrc中：
  
  `export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/home/local/SPREADTRUM/hursion.zhang/.local/share/Trash/files/28.0.2/lib64`
