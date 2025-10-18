---
id: 1ezdeiufbxfuf9ww7l73wsr
title: Case
desc: ''
updated: 1756693533549
created: 1755482553248
---

## a b 基于同一个提交分别开发，a开发后生成的提交A，和b开发后生成的提交B，其中A已经同步到了远程仓库上，B如何合并同步到远程仓库

```bash
 1998  git add .
 1999  git commit -m "[high][code] modify 4g usb interface, and add six aixs"
 2000  git pull --rebase origin develop_smart_1106
 2001  git status
 2002  git rebase --continue
 2003  git add 
 2004  git add src/global/gtype.h
 2005  git rebase --continue
```

## 拉取了仓库以后，基于提交3进行开发，做了修改，但是现在分支已经开发到10了，目前我需要一个 除了我修改的部分，其他都是最新提交状态的这样的代码，并且这是一个临时的，不会push到远程仓库的, 提交3的改动不能同步到远程仓库的develop分支

```bash

```

## clone仓库以后，在最新的develop分支，添加了想要测试的改动，还未进行暂存和提交，我的目的是不将改动同步到develop分支，而是创建一共临时分支


## 在develop分支提交10的时候拉取了临时分支dev2，然后进行改动，现在develop分支更新，提交到了13，dev2如何同步13的更新

```bash
2000  git status
 2001  git fetch origin
 2002  git rebase origin/develop
 2003  git stash     
 2004  git rebase origin/develop
 2005  git stash pop
 2006  git add .
 2007  git commit -m "[high][code] 同步develop分支最新改动，基于b9ee8d843f3859af91e提交，develop分支蓝牙部分有改动，暂时无法编译通过，待适配"
 2008  git reset --hard ae70177fc7e9d0113
```

但是rebase后发现改动大，无法编译通过，只能提交一个临时版本，然后要回到原先版本，通过reset hard回到原先版本以后，发现还是同样编译报错，查看分支图，旧版本也是基于develop最新分支了，导致问题
解决： git relog 查看历史编译通过的旧提交，直接基于旧提交chckout新分支用于临时使用

```bash
2010  git reflog
 2011  git reset --hard 0bfe9ce
 2012  git checkout -b dev2_compile 2e3a130
 2013  git status

 xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-ydn_rv1126_local/aplus_2$ git reflog
2e3a130 (HEAD -> dev2_compile) HEAD@{0}: checkout: moving from feature/develop-rv1126-sync-1106 to dev2_compile
0bfe9ce (feature/develop-rv1126-sync-1106) HEAD@{1}: reset: moving to 0bfe9ce
ae70177 HEAD@{2}: reset: moving to ae70177fc7e9d0113
0bfe9ce (feature/develop-rv1126-sync-1106) HEAD@{3}: commit: [high][code] 同步develop分支最新改动，基于b9ee8d843f3859af91e提交，develop分支蓝牙部分有改动，暂时无法编译通过，待适配
ae70177 HEAD@{4}: rebase (finish): returning to refs/heads/feature/develop-rv1126-sync-1106
ae70177 HEAD@{5}: rebase (pick): [high][code] sync the rv1106 smart wifi and bt diff
b9ee8d8 (origin/develop) HEAD@{6}: rebase (start): checkout origin/develop
2e3a130 (HEAD -> dev2_compile) HEAD@{7}: reset: moving to HEAD
2e3a130 (HEAD -> dev2_compile) HEAD@{8}: commit: [high][code] sync the rv1106 smart wifi and bt diff
fd7fe2a (tag: saas_2.6.6, develop) HEAD@{9}: checkout: moving from develop to feature/develop-rv1126-sync-1106
fd7fe2a (tag: saas_2.6.6, develop) HEAD@{10}: reset: moving to HEAD
fd7fe2a (tag: saas_2.6.6, develop) HEAD@{11}: checkout: moving from develop to develop
fd7fe2a (tag: saas_2.6.6, develop) HEAD@{12}: clone: from https://git.libawall.com/zhouhb/aplus_2.git
```


## rebase 冲突合并案例

```bash
<<<<<<< HEAD
=======
                qDebug() << "[EVENT] AutoStampling & Re-Calculate Operation Times";

            } else {
                if (m_sysStatus->operationStatus == OPER_UPGRADE_IMAGE && m_upgradeInfos->ugResult != 3) {
                    //更新镜像禁止自动休眠
                    m_noOperDTime = QDateTime::currentDateTime();
                    sleep_times = m_noOperDTime.secsTo(QDateTime::currentDateTime());
                    qDebug() << "[EVENT] Upgrading & Re-Calculate Operation Times";

                } else if (m_sysStatus->isLogin &&
                           (m_sysStatus->operationStatus == OPER_VERIFYFINGER ||
                                m_sysStatus->operationStatus == OPER_SEALSWTICH ||
                                    m_sysStatus->operationStatus == OPER_ADDINKING ||
                                        m_sysStatus->operationStatus == OPER_LOCKING ||
                                            (m_sysStatus->operationStatus >= OPER_STAMP_RDY && m_sysStatus->operationStatus <= OPER_STAMPCOMPELETE) ||
                                                (m_sysStatus->operationStatus >= OPER_STAMP_CONSECUTIVE && m_sysStatus->operationStatus <= OPER_STAMP_CONSECUTIVE_PAUSE))) {
                    //重要操作禁止自动休眠
                    m_noOperDTime = QDateTime::currentDateTime();
                    sleep_times = m_noOperDTime.secsTo(QDateTime::currentDateTime());
                    qDebug("[EVENT] Current Operations[%d] & Re-Calculate Operation Times !", m_sysStatus->operationStatus);

                } else {
                    MainDatas::instance()->getSystemStatus()->isLcdBrige = false;
#ifndef YDA_SMART
                    glGpioSetPin(LCD_ON_BANK, LCD_ON_PORT, LCD_ON_PIN, 0);
#else
                    glGpioSetPin(LCD_BK, 0);
#endif
                    qWarning("[EVENT] Device Lcd Screen Auto Off !!!!!!!!!!!!! [%dmin]", sleep_times);

                }
>>>>>>> 275c89a (smart引脚映射及基本功能对应；)
            }
```
冲突处理的过程：
1. 确认冲突位置
2. 生成冲突hunk文本
3. 通过diff格式显示双方基于第三方提交(BASE共同祖先)的冲突部分
4. 关键：如果是删除的，就不显示了
5. 所以，通常来说，要确认冲突文本中删除的内容，首先要确认一方相对于
6. vscode的插件功能的compare changes可以直接查看双方各自相对于冲突块的差异


## rebase 冲突合并：rebase过程，手动修改， 将自动合并的文件恢复到develop分支内容

```bash
git checkout --ours file

把文件恢复成 develop 的版本

等于你说的「我不要 smart 分支的改动」

git checkout --theirs file

把文件恢复成 smart 分支 commit 的版本

等于「我不要 develop 的内容」

git restore --source=HEAD file

和 --ours 在这个场景下结果是一样的

因为 HEAD 正是 rebase 的目标分支（develop）

所以效果就是「丢弃冲突，回到 develop 的版本」
```

## rebase冲突合并：rebase过程中，合并规则，有哪些基本规则，什么情况下改动直接应用，什么情况下判定为冲突，冲突处理规则，以及冲突显示逻辑

git 有哪些代码合并规则：


1. git代码合并，文本比对判断的规则
2. 在三方合并场景下，git的行改动判断规则？尤其是存在空行处理的情况下  【Git 判定“同一行修改”是 严格基于行内容，空行、缩进、注释的改动可能被认为“同一行未冲突”】

为什么代码合并采用三方合并这种方法？

差异部分内容是get_disk函数的实现，在develop分钟中这个函数被删除，在smart分支中存在并且被修改，为什么冲突内容会追加到handleNotifyRecordMinTimesToApp函数中
原因: rebase时，做的是三方比较
A = 共同祖先（base）
B = 你的分支（HEAD / ours）
C = 另一个分支（theirs）
所以处理过程：
计算B相对A的差异内容：由于B的处理是删除，所以显示为空
计算C相对于A的差异内容：由于C的内容是改动，所以显示的是修改部分，不会包含get_disk的函数，关键即 diff显示的冲突内容是相对于共同祖先A的差异，
而不是B和C的差异，如果
计算冲突内容所在位置，然后在B中进行冲突diff的显示
1. 问题：共同祖先是哪个？怎么确定？
2. 问题：待处理的冲突文件是哪一方的？
3. 问题：为什么双方冲突的内容不是和冲突文件的内容比较，而是和一个隐藏的文件内容比较，这样看容易有歧义
4. 问题：为什么这里冲突内容是放置结尾}的里面，而不是外面


```c++
//handleNotifyRecordMinTimesToApp在develop中函数行范围：304-321,在smart中getDiskSpaceSize_MB的函数行范围：314-337,在smart中getDiskSpaceSize_MB的改动行范围320-327，合并后冲突显示范围：417-437
void DevCheckController::handleNotifyRecordMinTimesToApp(int min_times)
{
    if (m_userInfos->identity == 4) {
        QString send_datas;
        if (m_userInfos->btCmdVersion == 2) {
            send_datas = QString("/>|23|RECORDTIMEV2|OK~0|%1|#").arg(min_times);
        }
        if (!m_userInfos->serialMode) {
            emit MainSignalManager::instance()->sendBTMessage(send_datas);
        } else {
            emit MainSignalManager::instance()->sendOneGpySerialDatas(send_datas);
        }
    } else {
        emit MainSignalManager::instance()->sendTopicTypeMessage(
                    TOPICID_RECORDED_TIME_POST,
                    glJsonCreateRecordMinTimesReply(0, m_systemConfig->sealId, m_userInfos->documentId, min_times));
    }
<<<<<<< HEAD
=======
#ifndef YDA_SMART
    QStringList arguments;
    arguments << "-c" << "df -lm | grep userdata | awk '{print $4}'";
    check_process.start("sh", arguments);
#else
    QString command = R"(df | grep userdata | awk '{print $2}')";
    check_process.start("/bin/sh", QStringList() << "-c" << command);
#endif
    check_process.waitForFinished(500);
    check_process.waitForReadyRead(500);
    QByteArray all = check_process.readAll();
    all.chop(1);//exp: 5600\n = 5600MB

    if (all.isEmpty()) { return -1; }

    int kb_size = all.toInt();
    return kb_size;
>>>>>>> 59fd51a (更替pgrep、pkill等工具指令；守护进程变更检查方式；磁盘溢出变更计算方式；)
}
```

## vscode 为git merge rebase显示的diff代码块，上方提示的compare changes内容说明

点击后左右两边显示的不是两个冲突提交原本的代码内容展示，而是两种冲突处理后的结果展示，所以不能通过compare changes来对比提交原本的内容差异
要查看冲突分支的内容差异，必须通过'resolve in merge editor' 按钮进行

## rebase之后，发现有文件rebase存在问题




## rebase后，develop分支又有更新，需进行同步

开发分支：develop_smart_1106_cur
被rebase分支: develop
备份分支：develop_smart_1106_bak

1. 备份分支内容

```bash
git branch -m develop_smart_1106_cur
```

2. 切换到待合并分支

```bash
git branch develop_smart_1106_bak
```

3. 拉取仓库内容，rebase处理

```bash
git fetch origin
git checkout develop_smart_1106_cur
git rebase origin/develop
# origin/develop 和 develop不同，前者是基于远程的devlop分支，所以基于这个，可以先进行fetch就行了，后者基于本地，需要本地同步完备
git rebase --continue
# git rebase --abort
# git log --oneline --graph --decorate --all
```

4. rebase origin/develop 的问题
第一次rebase花费了大量步骤，合并了一个分支，第二次rebase又是从rebase源头提交开始手动处理冲突，并没有自动进行
有可能是因为加了 origin/前缀，基于远程分支rebase，准备尝试先合并本地develop然后再基于本地rebase，无效

5. 尝试解决问题4，使用merge

6. 尝试解决问题5，使用rebase并指定rebase提交

rebase默认行为，确认共同祖先提交，那么需要被rebase的分支，共同提交之后的提交将会作为被rebase的起点
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git merge-base develop develop_smart_1106_cur
b9ee8d843f3859af91e3eb6e73fcefae848ec242

通过-onto参数，可以指定被rebase的分支具体的rebase起点

7. rebase --onto指定基地的时候错误，以外将被rebase的分支最新提交作为基地，导致没有任何东西合并，并且两个分支引用指向同一个提交
如何恢复develop_smart_1106_cur rebase前的状态

查看目标分支的提交引用情况

```bash
xjf1127@DESKTOP-E1M5RHH:~/codespace/git_down/pe-smart_rv1106_local/aplus_2$ git reflog develop_smart_1106_bak
2e438b6 (develop_smart_1106_bak) develop_smart_1106_bak@{0}: reset: moving to develop_smart_1106_bak@{1}
529d118 (HEAD -> develop_smart_1106_cur, origin/develop, develop) develop_smart_1106_bak@{1}: rebase (finish): refs/heads/develop_smart_1106_bak onto 529d118ae74565fd21670f9c6560b49c0264d409
2e438b6 (develop_smart_1106_bak) develop_smart_1106_bak@{2}: branch: Created from develop_smart_1106_cur
```

可以发现 第二条记录，表明，提交529d118被多个分支引用，其中包括develop_smart_1106_bak rebase的结果，refs/heads/develop_smart_1106_bak指同一个提交



8. gpt-question

question

```bash
现在需要讨论一下我之前代码合并的问题
develop分支是主开发分支
develop_smart_1106_cur是我的开发分支，二者编译器不同，我这边主要是同步develop的内容，然后修改有平台差异的地方，相当于develop是基线，每次我都是在他最新提交的基础上，把他那边的最新功能同步过来合并过来，通过宏定义使得develop_smart_1106_cur兼容两套代码
但现在有一个问题，在中期，由于我这边与develop分支差异较大，所以做了一次基于develop 的rebase，冲突量非常大，改动后，develop_smart_1106_cur相当于同步合并了当时develop最新的10提交，develop_smart_1106_cur提交5，但是随着开发进度推进，develop又更新了来到了提交15
此时我尝试进行rebase，但发现需要手动解决的冲突和上次rebase的一样，都需要重投开始，
后来我了解到onto参数，相当于可以指定共同祖先，但现在我有一个疑惑
我该指定什么为共同祖先呢
并且这样做和merge相比，直接进行合并有什么区别呢，在git自动处理冲突的层面
并且及时 rebase onto以后，下次进行同步，会发生什么，还是要和最早开始一样从很原始的提交开始rebase吗
```

answer

```bash
遇到的是 如何维护一个长期存在的功能分支（feature/platform branch）与基线分支（develop）同步
develop 是主开发分支
develop_smart_1106_cur 是平台适配分支
你一开始用 rebase 去同步 develop，结果：

第一次 rebase 时，冲突量巨大，你手工解决了。

但是 下次再 rebase，Git 又要求你重新解决一样的冲突，好像白做了。
这就是 rebase 的本质问题：
rebase 会把你的分支 从共同祖先开始重新应用，Git 不会记住你之前解决过的冲突。

--onto 的意义
把 branch 上 从 oldbase 之后的提交 迁移到 newbase 之上。

适合「我不要从最初的祖先，而是从某个中间点之后开始 rebase」。

⚠️ 但是这并不能解决“冲突重复解决”的问题，因为本质上 rebase 每次都会「重新打补丁」
3. rebase vs merge 的冲突处理差异
rebase

每次 rebase 都会从基线的共同祖先开始，逐个应用你分支的提交。

冲突不会记忆，即使你上次解决过，这次还是要重新解决。

好处：分支历史干净（像是一直在 develop 上开发）。

坏处：长期分支，rebase 成本极高。

merge

git merge develop 会产生一个 merge commit。

Git 会尝试使用 冲突解决的历史记录（称为 conflict resolution reuse），如果之前相同的冲突已经解决过，Git 会自动采用之前的解决结果。

好处：以后再 merge 相同的地方不会反复手动解决。

坏处：分支历史有 merge commit，不是线性的。
5. 你问的几个关键点

我该指定什么为共同祖先呢？

--onto 常见场景是「我不想 rebase 整个分支，只想 rebase 部分提交」。

但你这种情况（长期同步 develop），指定共同祖先意义不大，因为每次 develop 都在前进。

和 merge 相比，有什么区别？

merge 会记住冲突解决结果，rebase 每次都得重来。

merge 适合长期分支；rebase 适合短期分支。

即使 rebase onto 以后，下次同步会怎样？

下次 rebase，还是会从新的基线开始，之前的冲突依然要重来。

所以 问题不会消失。

✅ 总结建议

你的平台分支（develop_smart_1106_cur）应该用 merge develop 定期同步，而不是 rebase。

如果你坚持用 rebase，那就要接受每次都要重复解决同样的冲突。

--onto 适合裁剪提交范围，不解决冲突重复的问题。


```

merge会记住冲突处理，可以在下一次应用
但rebase本质上是为了裁剪范围，整理多分支在时间线上的提交结构，进行重排，并不会记住处理的冲突，每次rebase都需要从起点处理冲突，不适合当前两个分支的持续同步

## merge and rebase
