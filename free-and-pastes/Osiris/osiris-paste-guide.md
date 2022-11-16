# Osiris

Osiris是CS:GO的一个开源合法作弊器。它通常被用来作为粘贴作弊器的基础。在这个由BaconatorPizza编写的指南中，我们将教大家如何从源代码中编译Osiris。

***

## Step 1: Downloading Needed Items

Download the [Osiris source](https://github.com/danielkrupinski/Osiris). Osiris is one of the most stable open source cheats with the most features.

Download [SigBench](https://www.unknowncheats.me/forum/anti-cheat-bypass/167449-sigbench-test-binary-signature-scans.html), we will use it to compare signatures.

Download any source code obfuscator that works decently well for C++.

Lastly, download [the VMProtect demo](https://vmpsoft.com/purchase/buy-online/).

***

## Step 2: Setting Up

Extract the .ZIP file you downloaded with the Osiris source inside of it. Double click the .sln file inside of the folder you extract. This will open Visual Studio.

![Failed to load image](https://preview.redd.it/1ec9jyqmthi51.png?width=658&format=png&auto=webp&s=6f354cdbff8ddc923a9bea990ad43305fa0feeb4)

Right click the text that says 'Osiris' on the sidebar, and click Properties. Click on the arrow next to C/C++, click on Optimization, and set Optimization to 'Disabled (/Od)'.

Click OK.

![Failed to load image](https://preview.redd.it/7uc4dzumr8y31.png?width=846&format=png&auto=webp&s=a98d79a9cdd273c142cfe4c6101f10ab63119835)

Then, in the top bar, change Debug to Release, and change the thing next to it to x86 (it should be x86 by default though).

![Failed to load image](https://preview.redd.it/pyn1lr6yuhi51.png?width=320&format=png&auto=webp&s=627d3c4b81f3390e18a3457039f18872eec24e93)

Press Local Windows Debugger (or whatever it says for you), and wait for it to build. Then go into your project folder again, and then into Release, you should see a file called Osiris.dll. Copy that file and paste it in another directory for later, like your Desktop.

***

## Step 3: 移除 VMT Hook

On the sidebar, you will see a folder called Hooks, open that up and delete VmtSwap.cpp, VmtSwap.h, VmtHook.cpp and VmtHook.h.

Next go into Header Files, and open Hooks.h. You will see 2 lines towards the top:

```cpp
#include "Hooks/VmtHook.h"

#include "Hooks.VmtSwap.h"
```

Delete these 2 lines.

You will also see a line saying using `using HookType = VmtSwap;`, delete that line too.

You are done removing VMT hooking! Onto the next step.

***

## Step 4: 移除多余功能

进入名为hacks的文件夹，删除所有你不会使用的功能的文件。删除.cpp和.h文件。如果你不是太滑头，你可以进入Visuals.cpp，删除你不会使用的视觉效果的功能。

接下来进入Source Files，并打开Config.cpp。你应该有一大堆错误。简单地删除导致错误的那几行。(例如，如果你在第15行有一个错误，删除第15行）。

进入Hooks.cpp，做同样的事情。最后，对GUI.cpp进行同样的操作。

再按一下本地Windows调试器，因为有时（至少对我来说），某些错误在你编译前不会显示出来。另外，如果它说错误是在GUI.obj、Hooks.obj或其他地方，打开file-with-error.cpp。如果它没有显示错误在那里，就打开file-with-error.h。

你已经完成了删除功能的工作 进入下一个步骤。

***

## Step 5: 重命名功能

当然，每个文件/功能都有一个名字。然而，VAC可以扫描你的作弊代码并检查这些名字（以及代码的其他方面）。如果它与已知的作弊代码相匹配（会的，Osiris和其他开源作弊器肯定会被检测到），它将禁止你。

因此，我们必须做的是重新命名，以对抗这种情况。这不会完全解决我们的问题，但它仍然有一点帮助。最重要的是，它可以改变一下签名，这很好!

例如，将Backtrack.cpp和Backtrack.h重命名为AimRewind（或类似的东西）。请记住，你也必须在所有对该文件的引用中改变这一点，比如当另一个文件在做`#include <Hacks/Backtrack.cpp>`时。

在像GUI.cpp这样的地方，包含它将实际显示的名字的字符串不需要修改它的值。然而，对变量本身进行重命名是一个明智的想法。

一旦你对作弊器中的大多数东西都做了这个处理，就可以进入下一个步骤了

***

## Step 6: 修改代码

你知道我说过VAC会根据已知的作弊代码扫描你的作弊吗？那么仅靠重命名是不够的。我们需要做得更多!

即使是做一些小事，比如把`currentTargetXPos = 52.234`替换成`currentTargetXPos = 50 + 2.234`（或类似的东西，笑，只是我想出来的一个例子），不仅可以改变签名，而且可以对抗VAC的检测！所以到处做这些事。

所以到处去做这个。这可能需要一些时间。然而，如果你做得够多，它将对你的粘贴不被发现有很大帮助。

一旦你对作弊器中的大多数东西都这样做了，就可以进入下一个步骤了!

***

## Step 7: 编译

这最后一步将帮助你编译你的作弊器。

First of all, if you did decide to use a source code obfuscator, make sure you use that first before you compile because it probably wont work on your finished build of the cheat. Once you have ran it, press Local Windows Debugger to compile.

Next, go into your project folder, Release, and rename the DLL inside to whatever you want your cheat to be called (ex. RainbowWare).

If you decided to get VMProtect, run that on your DLL. Take the outputted one, and now we will test the signatures!

Open up SigBench. In the first slot, input the original DLL from earlier. In the second slot, input your new one. Press benchmark.

签名应该有一个相当大的差别，它应该看起来像这样。

![https://imgur.com/6kOpTCv](https://i.imgur.com/6kOpTCv.png)

You are now done!

***

## Step 8: Injecting

Find any decent injector (like [Extreme Injector](http://www.extreme-injector.com/#:~:text=Extreme%20Injector%20is%20a%20small,the%20hacking%20of%20computer%20games.), [Xenos](https://www.unknowncheats.me/forum/general-programming-and-reversing/124013-xenos-injector-v2-3-2-a.html) or the [GH Injector](https://guidedhacking.com/resources/guided-hacking-dll-injector.4/), really anything with manual mapping). Select your finished DLL, and inject!

I also reccomend you use the [VAC Bypass Loader](https://matt12945.gitbook.io/csgo-subreddit/free-and-pastes/vacbypass) as it will also greatly reduce your chances of being banned with any cheat.

Don't share the cheat with very many people or VAC may start detecting it. Also, do these steps again every now and then to change the signature and get any new updates your source may have.

Enjoy!
