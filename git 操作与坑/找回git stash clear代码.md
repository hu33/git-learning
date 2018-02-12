## 找回git stash clear的代码



### 背景

某次脑抽，不小心将开发了一天的工作`stash`起来，又不小心`clear`掉了，然后发现时开发代码已经木有啦！！我也很崩溃，但是对生活充满希望的我沉着冷静地思考了一下，别人肯定也有过脑抽的情况，git肯定有办法找回`git stash clear`掉的代码！

然后我谷歌了一下，发现脑抽的人还不少，然后按照他们的解决步骤试了一下，确实是有用的，虽然过程稍微蠢了点~但是！谁叫我们自己chun呢~

### 解决思路

首先要知道的是，在`git`中把`commit`删除后，`git` 并没有将这些`commit`真正的删除，只是移除了对它的引用，这样它就变成了悬空对象（dangling commit）。所以我们只需要找到这些悬空对象，找到你想要恢复的那个`commit`，然后`merge`回去就行了~

###解决步骤

1. `git fsck —lost-found` ：你会看到一条一条的类似于dangling commit 1f1238422936这种记录，然后你要做的就是找到被你删掉的那个`commit`。

   ![找回git stash clear代码1](http://ow7p6xhhi.bkt.clouddn.com/%E6%89%BE%E5%9B%9Egit%20stash%20clear%E4%BB%A3%E7%A0%811.png)

2. `git show 1f1238422936`：然后就一个个 `show` 这些 `commit` 吧~

   ![找回git stash clear代码2](http://ow7p6xhhi.bkt.clouddn.com/%E6%89%BE%E5%9B%9Egit%20stash%20clear%E4%BB%A3%E7%A0%812.png)

3. `git merge a2537fe6fca9d`：找到你想要恢复的`commit`后，再`merge`一下，就恢复到当前分支啦~

步骤是不是很简单！！只是`show`的过程...稍微有点...恶心，哈哈哈。不过如果`git`执行了垃圾回收操作的话，靠这种方式就找不回来啦~