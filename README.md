> 本文同步發表於部落格（好讀版 →）：https://igouist.github.io/post/2021/04/newbie-1-hello-git/

![](https://i.imgur.com/N54Pg7s.png)

這是俺整理公司新訓內容的第一篇文章，目標是整理 Git 相關的筆記。

- [前言、推薦資源](#前言推薦資源)
- [什麼是 Git？](#什麼是-git)
- [分散式版本控制](#分散式版本控制)
- [確認 Git 已經安裝](#確認-git-已經安裝)
- [建立一個新的儲存庫（Init）](#建立一個新的儲存庫init)
- [加入變更（Add）](#加入變更add)
- [提交變更（Commit）](#提交變更commit)
  - [Commit 的訊息](#commit-的訊息)
  - [Commit 的時機](#commit-的時機)
  - [Commit 和 Add 的 Combo 技](#commit-和-add-的-combo-技)
  - [加入 .gitignore 忽略檔案](#加入-gitignore-忽略檔案)
- [提交紀錄（Log）](#提交紀錄log)
- [版本差異（Diff）](#版本差異diff)
- [關於 HEAD](#關於-head)
- [Revert](#revert)
- [Reset](#reset)
- [分支（Branch）](#分支branch)
  - [新建分支（branch）與切換分支（checkout）](#新建分支branch與切換分支checkout)
  - [合併（Merge）](#合併merge)
  - [衝突（Conflict）](#衝突conflict)
  - [Rebase](#rebase)
  - [查看分支列表、刪除分支](#查看分支列表刪除分支)
  - [關於斷頭（detached HEAD）](#關於斷頭detached-head)
  - [關於分支策略](#關於分支策略)
- [遠端儲存庫（Remote）、推送（Push）](#遠端儲存庫remote推送push)
- [擷取（Fatch）、提取（Pull）](#擷取fatch提取pull)
- [Clone](#clone)
- [關於提取要求（pull request）](#關於提取要求pull-request)
- [小結](#小結)
- [本系列文章](#本系列文章)
- [參考資料](#參考資料)

<!--more-->

### 前言、推薦資源

說來慚愧，前陣子 PTT 和臉書社團都有討論到相關科系畢業卻不會 Git 會不會太誇張，我正是畢業之後才開始用 Git 的那類人囧，相信像我一樣的人並不少，因此這個系列就決定從「**新訓時學到的 Git 的基本操作**」開始記錄。

開始之前先感謝公司前輩和完善的新手教學，還有第一天就先學 Git 的優良傳統。另外，也感謝相當多優秀的 Git 學習資源，說明得也更為詳細深入，想好好了解 Git 的朋友也可以逛逛，這邊就先推薦一波：

- [為你自己學 Git](https://gitbook.tw/)
    - 對新手非常友善。網站點進去後往下拉，可以看到大部分章節都能免費看，佛！
    - 最有價值的是裡面的各種狀況題。畢竟當你用 Git 不只需要基本操作的時候，呃，祝你好運
- [連猴子都能懂的 Git 入門指南](https://backlog.com/git-tutorial/tw/)
    - 從入門到進階篇跟過一次的話，基本操作就沒有問題了
    - 圖解讓人很好理解，而且在教學的實作部分會提供儲存庫讓你下載實作
- [黑暗執行緒的 Git 分類文章](https://blog.darkthread.net/blog/category/Git)
  - 同場加映：[黑暗執行緒的 Git 指令筆記](https://blog.darkthread.net/blog/my-git-cheatsheet/)
  - 黑大出品，品質保證
- [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_TW)
    - 用遊戲通關的方式認識 Git，對於一些分支的概念會很有幫助
- [30 天精通 Git 版本控管](https://ithelp.ithome.com.tw/users/20004901/ironman/525)
- [Pro Git](https://git-scm.com/book/zh-tw/v2)

接下來我們就從認識 Git 開始吧！

### 什麼是 Git？

你發生過以下狀況嗎？

- 從沒做過版本控制，結果突然要改回前一版，不知所措
- 使用資料夾／壓縮檔板控
    - 20201201.rar, 20201215_v2.rar, 20201215_首頁.rar......
    - 空間越吃越兇，東西越來越雜，事情越想越不對勁，但是不敢刪除
    - 其實不知道每一份實際上改了哪裡，要復原某一段的時候要找半天，不如直接重寫一段
- 團隊合作／分組報告，各自負責一個區域，結果複製來複製去組不起來，不只需要看眼科，修 BUG 還比寫的時間還多
- 看到一段程式碼
    - 完全不知道為什麼要這樣寫
    - 或是氣到要死，抓不到戰犯

那麼，你很有可能需要 Git！

<!--more-->

**Git 是一套分散式的版本控制，就像是打電動時的存檔**。讓我們可以在面臨重要選擇的時候存檔、打王之前存檔、打贏的時候也存個檔。當然，像是那種有多劇情多結局的遊戲，也可以針對不同路線各自存檔。

同時它也支援雲端存檔，你可以在電腦上存個檔，然後有網路的時候就丟上去雲端備份一下。而這個雲端備份是共用的，所以你可以跟朋友一起玩同一款遊戲，各自攻略不同的 BOSS，再把存檔和朋友互相交流交流，合成一個有兩份戰利品的存檔。

這些功能在 Git 有著聽起來比較厲害的名字，例如認可（Commit）、分支（Branch）、分散式、合併（Merge）等等。我們後續再慢慢了解它們。

### 分散式版本控制

現在我們已經有個大致上的印象了，但 Git 還有更多特色，例如：

- 免費！開源！
- 讀檔存檔的速度很快！
- 分散式！

前面兩項比較好理解，我們這邊說明一下什麼是「分散式」的版本控制：

以前的版本控制，例如 SVM，是採用「集中式」的版本控制：每次變更完要存個檔等等，都要連線到伺服器上進行處理，就像是好幾個人一起連線存取同一張資料表一樣。但是這樣遇到沒有網路的狀況就沒辦法讀檔存檔，或是變更的檔案很大就會等到天荒地老，這種對伺服器強依賴的狀況實在是有點兒不方便。

分散式版本控制呢，則是**每個人都有各自的完整一份資料**，你要存檔讀檔啥的都你自己電腦上弄就好了，只有必須和其他人交流的情況（例如想丟上去雲端存檔了，寫完了要合併了）才需要透過網路來處理，這樣子日常做事起來就快上不少。

也因為每個人都有一份，某台機器掛掉就導致整份程式碼不見的狀況少了很多，變更歷史也不容易竄改。同時更發展出強大的分支操作，和一些工作流程等等，是相當靈活的版本控制方式。

關於版本控制方式的比較也可以參考這兩則文章：

- [集中式vs分布式 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600/896202780297248)
- [關於版本控制 - 開始 - Pro Git 繁體中文版 (iissnan.com)](https://iissnan.com/progit/html/zh-tw/ch1_1.html)

現在我們知道了 Git 是一個分散式的版本控制軟體，幫助我們做一些存檔讀檔同步的動作。接著就讓我們開始來操作看看吧！

### 確認 Git 已經安裝

> 小提示：你可能需要先 [安裝 Git](https://git-scm.com/downloads)。<br/>安裝過程相當簡單，通常只需要下一步即可。

首先我們得先確認 Git 已經準備好了，打開我們的命令視窗（Powershell 或是 CMD 之類的），輸入 `git --version`，你應該能夠看見 Git 的版本資訊

![](https://i.imgur.com/YUKIdit.png)


另外，也可以輸入 `git --help` 來查詢可用的指令。或是加上想用的指令，例如 `git --help clone` 就會開啟該指令的文檔，臨時要確認指令和參數的時候相當有用。

Git 現在已經有相當多的 GUI 可以使用，真的很多。我個人在家的時候是使用 [GitHub Desktop](https://desktop.github.com/)、在公司大多時候使用 Visual Studio 內建的 Git 工具。此外，也見過朋友和同事使用 [GitKraken](https://www.gitkraken.com/)、[Fork](https://git-fork.com/) 等等。關於這些 Git 的 GUI，可以參考官方整理的 [GUI Clients](https://git-scm.com/downloads/guis) 頁面。

但由於我個人忘記指令的狀況頗為嚴重，而且也不是每個環境都有 GUI 可以用。因此這篇有關 Git 的部分將會以 CLI 指令為主進行紀錄。使用 GUI 介面的朋友也不用擔心，現在的介面都做得很精簡，<s>而且這篇也寫得很淺，</s>很簡單就能找到各指令對應的操作。

確認我們已經有 Git 之後，接著就必須先跟 Git 說我們是誰、信箱是什麼：

```powershell
git config --global user.name "I am INEVITABL"
git config --global user.email "Thanos@Gemmai1.com"
```

這樣就會把設定好的名稱和信箱存到 config 裡面。如果要查看現在有的 config，可以輸入<br/> `git config --list`

> 另外也還能指定編譯器、新增別名等等，可以參照 [其它方便的設定 - 為你自己學 Git](https://gitbook.tw/chapters/config/convenient-settings.html)

> 由於 Git 是用這組名稱和信箱進行辨別，因此在 Github 上就可以做一些很酷的事。例如假冒人家和防止人家假冒（？），有興趣的可以看看：[用 Git 這麼久了，你知道 commit 是可以偽造的嗎](https://medium.com/starbugs/how-to-fake-the-author-of-git-commit-f44453b70afc)

> 順便打個廣告：好奇我的 Powershell 長得跟你的「有點不一樣」的朋友，可以參考本部落格的另一篇：[Powershell 美化作戰](/post/2020/08/powershell-beauty/)

### 建立一個新的儲存庫（Init）

現在讓我們從建立一個新的儲存庫開始。現在讓我們新增一個資料夾（在這邊我取名叫做 `hello-git`）當作這篇 Git 紀錄的遊樂場：

![](https://i.imgur.com/hPLoleB.png)

> 題外話：既然都打指令了，也可以試試來建立資料夾 <br/> Powershell 用 `New-Item C:\hello-git -ItemType "directory"` <br/>隔壁棚 Linux 請用 `mkdir` 來試試。<br/>不過基於懶惰，上面的示範是滑鼠右鍵建立的，耶嘿

接著讓我們先移動過去資料夾：
```powershell
cd C:\hello-git
```

並且**使用 `git init` 將 Git 初始化**：
```powershell
git init
```

![](https://i.imgur.com/WIckbA1.png)

如此一來，這個資料夾就成為了「**工作區（Working directory）**」，也就是「歸我 Git 管啦！」的意思

同時，我們也能在原本的 hello-git 資料夾中，發現多了一個 `.git` 的隱藏檔案：

![](https://i.imgur.com/3hgT4eP.png)

這個 `.git` 就是用來幫我們處理一堆版本控制工作的地方，也叫做<br/>「**儲存庫（Repository）**」。裡面會放一些設定值、已經確認變更的文件等等。

現在讓我們來確認一下這個工作區的狀況，`git status` 將會列出當前的狀態，這將會是我們很常使用的指令。

![](https://i.imgur.com/5XwsruS.png)

可以看到我們現在在 Master 分支，並且還沒有 Commit 任何東西。這兩個部分我們等等就會說明，現在就讓我們按照它的提示，來把檔案丟進去給 Git 試試吧。

### 加入變更（Add）

我們先到 hello-git 資料夾裡，新增一個 `A.txt`

![](https://i.imgur.com/X8M9I67.png)

並且加入一些內容，例如「Hello!」

![](https://i.imgur.com/erpTd0w.png)

現在讓我們再次使用 `git status` 觀察一下

![](https://i.imgur.com/J7ByoFh.png)

可以看到 Git 已經發現 A.txt 的存在了，但它也告訴我們，它還沒有把 A.txt 放在 <s>眼裡</s> 追蹤目標中

我們要用 `git add` 指令，Git 才會把這個變更納入這次的動作中：

```powershell
git add A.txt
```

![](https://i.imgur.com/fFlY2rK.png)

現在可以看到我們新增檔案這個動作已經被 Git 捕捉到了。

前面有提過，**我們在 `git init` 之後，當下的資料夾就會變成「工作區」，而 .git 則會成為「儲存庫」。在這兩者之間，還會有一層「暫存區（Staging Area）」**（有些朋友會叫做「索引區（index）」，對象是一樣的）。

這三者之間的工作流程就像是一條工廠輸送帶：
```
工作區（Working directory）→ 暫存區（Staging Area）→ 儲存庫（Repository）
```

而這個 **`add`** 加入檔案的過程，其實就是將我們 **在工作區所做的變更，加入到暫存區（Staging Area）**

所以我們新增檔案、變更內容等等，其實都要跟 Git 用 `add` 指令打聲招呼，說「我有動這個哦，幫我看著一下」，Git 才會把這些變更的對象放到暫存區裡，等待後續丟到儲存庫的動作。

那有些朋友可能就會問啦：我工作的時候處理的檔案一定很多個啊，每個都要 add 豈不是累死？

不用擔心，當有多個檔案要 `add` 的時候，我們可以加上參數 `-A`，也就是：

```powershell
git add -A
```

或是
```powershell
git add --all
```

這樣就會直接把所有變更抓進來囉。

當然，懶還要更懶，事實上現在的 GUI 工具，例如我接觸的 GitHub Desktop、Visual Studio 等等，其實都會自動幫忙 Add 了，真是貼心。

![](https://i.imgur.com/dARJCu3.png)

當我們把變更從工作區用 add 丟到暫存區之後，要怎麼再從暫存區丟進儲存庫呢？這時候就要使用 `Commit` 了！

### 提交變更（Commit）

完成了一項功能？　Commit！<br/>解了一個ＢＵＧ？　Commit！<br/>下班了？　Commit！<br/>地震了？　Commit！

如果你有在使用 Git，Commit 絕對是你使用最多次的功能。**當我們 Commit 之後，暫存區的變更就會寫入儲存庫，到這個步驟我們才真正地存檔成功。**

現在讓我們來完成這次變更吧：

```powershell
git commit -m "Add A.txt"
```

![](https://i.imgur.com/riSISaf.png)

有看到 ~ file changed 就代表我們已經成功 Commit，把變更存進儲存庫囉！

#### Commit 的訊息

這邊要特別提的是 `-m "Add A.txt"` 這個部分。`-m` 就是 Message 的 m（好順口），是用來輸入本次 Commit 的訊息，雖然可以省略，但**強烈建議 Commit 的時候都一定要加上訊息！**

實際想想就能理解了，既然 Git 是可以讓你隨時存檔讀檔的工具，那麼假設你看到這一排存檔：

- 存檔
- 存檔
- aaa
- a

和這一排存檔：

- 結局選項前
- 結局選項１
- 結局選項２
- 決戰前

哪一組比較能快速知道要讀取哪個檔案呢？

Git Commit Message 也是一樣的道理。

好的訊息可以快速了解每個版本的變更和背後原因，甚至讓後續接手的人（通常也就是幾天或幾個月後的自己）能迅速地掌握狀況。因此現在大多數的 Git 工具都會要求必須輸入 Commit 訊息，畢竟「訊息一條勝造七級浮屠」，不可不慎哪。

因此，這邊強烈推薦這篇 [Git Commit Message 這樣寫會更好，替專案引入規範與範例](https://wadehuanglearning.blogspot.com/2019/05/commit-commit-commit-why-what-commit.html)，內文用實際案例和 AngularJS 團隊的 Git Message 規範說明了好的 Message 該如何處理，相當清楚明瞭。

趁開始學習 Git 的時候就培養好的 Commit 習慣，將來一定都會派上用場。現在我也和同事一起嘗試著這套作法，畢竟通常救到的都是未來的自己嘛，哈哈。

#### Commit 的時機

另外，除了 Commit Message 以外，Commit 的時機和頻率也是時常被討論的議題。

再度用存檔來比喻，大概就像你要讀檔的時候發現只有這兩個存檔：

- 新手村對話１
- 魔王城決戰

這下完蛋，如果什麼關鍵道具還是劇情沒有拿掉，要嘛放棄，要嘛認命從頭開始。所以，我們在 Commit 的時候要盡量迴避這個狀況。對此，我的建議是：

**在所有你覺得「這是一個段落」的時候就 Commit。**

就像這小節的開頭：完成了一個小功能？ Commit；重構了一個變數的命名？ Commit。在你所有想要 Commit 的時候 Commit，畢竟 Commit 不用錢，真的不用省。

寧可多 Commit 幾次，等熟練 Git 的時候，<s>或是被靠夭洗版的時候</s>，再考慮用 rebase 之類的技能來把多個零碎的 Commit 整理成一個；也不要臨時出了什麼事，結果 Git 一打開，只能回到一個月前，那真的是欲哭無淚。阿彌陀佛，保護自己，就從 Commit 開始。

> 如果你現在已經有整理 Commit 的需求，可以參照以下幾篇：<br/>
> [【狀況題】修改 Commit 紀錄 - 為你自己學 Git ](https://gitbook.tw/chapters/using-git/amend-commit1.html)<br/>
> [送 PR 前，使用 Git rebase 來整理你的 commit 吧！ - 星巴哥技術專欄](https://medium.com/starbugs/use-git-interactive-rebase-to-organize-commits-85e692b46dd)<br/>
> [把多個 Commit 合併成一個 Commit - 為你自己學 Git](https://gitbook.tw/chapters/rewrite-history/merge-multiple-commits-to-one-commit.html)

#### Commit 和 Add 的 Combo 技

讓我們延續一下 Add 章節的「懶還要更懶」，現在當我們完成一個變更，就要先 Add 到暫存區，再 Commit 到儲存庫。如果覺得這個兩步驟驗證很麻煩的話要怎麼辦呢？

其實可以使用 Combo 技，一次搞定 Add 和 Commit，那就是 `-a`，例如

```powershell
git commit -a -m "Update A.txt"
```

但要注意這個招式只對已經在版本控制內的檔案有效，如果是新來的也還是要先去報到呦。

關於這段 `工作區 -Add→ 暫存區 -Commit→ 儲存庫` 的說明，也可以參見這篇為你自己學 Git 的[工作區、暫存區與儲存庫](https://gitbook.tw/chapters/using-git/working-staging-and-repository.html)，裡面用倉庫和廣場的比喻個人覺得很貼切。

#### 加入 .gitignore 忽略檔案

如果你跟我一樣，總是 `add -a` 無差別加入，或是 GUI 的一鍵 Commit 用太爽，很容易就會翻車。怎麼個翻車法呢？我在新訓的時候就有被問過：

「你把這推上來幹嘛？？？」

沒錯！有些東西我們是不需要加到版本控制中的，例如每次編譯都會產生的檔案（像是 .net 的 bin 資料夾之類的）、機密檔案、個人對編譯器的設定檔等等。

有些東西加入版本控制後，輕則讓你的 Git status 變得很雜亂、Commit 變得亂七八糟，重則影響到其他人的環境。因此我們要想辦法，讓這些東西不要加入版本控制裡。

這時候我們就可以加入 Git Igonre 來讓 Git 忽略這些東西。

現在讓我們來試試看，先新增一個 `Ignoreme.txt` 檔案，接著再新增一個 `.gitignore`：

![](https://i.imgur.com/4QWKh8Y.png)

然後在 `.gitignore` 中加上：

```
ignoreme.txt
```

接著，讓我們再次呼叫　`git status` 確認當前的狀態

![](https://i.imgur.com/k0sNgcN.png)

可以發現居然只有剛剛新增的 `.gitignore`，Git 真的就裝作沒看到 `Ignoreme.txt` 了！

藉由 .gitignore 我們就能讓 Git 知道哪些檔案它不要亂插手，通常來說每個專案底下都會有一個共用的 .gitignore 檔案，避免某些人沒跟到就把不該推的東西給推上去了。

至於哪些東西不該推呢，東西這麼多怎麼列得完呢？感謝社群，[github/gitignore](https://github.com/github/gitignore) 這兒都已經整理好了。就算裡面找不到的，只要 Google "你的語言或框架 + gitignore" 通常都會有結果，例如 [dotnet-ignore](https://github.com/Arasz/dotnet-ignore)，我們菜雞只要爽爽用，

最後別忘了讓我們也把 `.gitigonre` 也 Add 到我們的 Git 之中，並且 Commit 起來：

![](https://i.imgur.com/WTr6t7f.png)

### 提交紀錄（Log）

現在我們已經可以成功存檔了，但有玩過遊戲的都知道，通常都會有個地方讓你看當前的存檔和紀錄等等。那麼**在 Git 中要怎麼看我們的 Commit 記錄呢**？這時候就要用到 `log` 指令。

由於只有存過一次也太寒酸了，現在讓我們把 A.txt 打開，將原本的 Hello! 改成 Hello world! 並且再 Commit 一次：

![](https://i.imgur.com/uZY0Kjm.png)

![](https://i.imgur.com/IhLiS0z.png)

然後，讓我們試試 `git log`：

![](https://i.imgur.com/hD1c1h1.png)

就可以看到我們前面的 Commit 內容囉！

> 小提示：如果 Log 太多頁的話，可以用空白鍵前往下一頁、按 q 離開。 Git 的操作使用的是 Less，關於常用的操作方式可以參考 [Less (Unix) #Frequently used commands](https://en.wikipedia.org/wiki/Less_(Unix)#Frequently_used_commands)

**每次的 Commit 資訊通常會包含**：
- 作者
- 日期
- Commit Messaage
- Commit 用 SHA-1 計算出來的識別碼，可以當成是這個 Commit 的唯一 ID、身分證編號就行了
- 分支所在的 Commit
  - HEAD 是指向我們當前所在的分支，而我們 HEAD 所在的 master 分支則是在 743d... 這個 Commit 上
  - 關於 HEAD 我們會在 [關於 HEAD](#關於-head) 繼續說明
  - 關於 分支 我們會在 [分支](#分支branch) 繼續說明

> 題外話：如果有朋友在 windows 上，例如 powershell 使用 `git log` 或 git 相關指令有出現**中文亂碼**的情形，可以參考以下的語法進行調整：
>
> ```
> git config --global core.quotepath false         # 引用路徑的檔案名稱
> git config --global gui.encoding utf-8           # GUI 編碼
> git config --global i18n.commit.encoding utf-8   # Commit 編碼
> git config --global i18n.logoutputencoding utf-8 # Log 編碼
> $env:LESSCHARSET='utf-8'                         # Less 分頁的環境變數
> ```
> 其中環境變數需要直接前往 `系統內容 > 進階 > 環境變數` 並新增到系統變數上，否則每次重開 powershell 都要重打一次。
>
> 主要是因為一些歷史原因，Windows 在這塊並沒有全面支持 Utf-8，因此需要告訴 Git 我們要以 utf-8 作為編碼，並且也把 Git 用到的 Less 分頁等環境變數設定好。感謝 CSDN 的這篇 [PowerShell | git log 中文亂碼問題解決](https://blog.csdn.net/FollowGodSteps/article/details/96271359) 和 Github 上的 [Nightire 大大](https://gist.github.com/nightire/5069597)。
>
> 但是如果你是 [在 Windows 也要安裝 Ubuntu，我就是要 Bashhhhh](https://docs.microsoft.com/zh-tw/windows/wsl/install-win10) 的朋友就當我沒說。

不過比起這樣的文字，我們比較常用的還是 Git 那相當有特色的線圖。現在讓我們試試<br/> `git log  --oneline --graph`：

![](https://i.imgur.com/V9ysh2L.png)

可以看到 Log 變得相當精簡了。在我們後續會講到的[分支](#分支branch)情景下，線圖還會幫忙畫出不同分支路線（平常有使用 Git 的 GUI 工具的朋友們應該都很熟悉了）

這邊就借朋友的 Project 示範一下：

![](https://i.imgur.com/lHYIpqd.png)

除了直接看線圖以外，`git log` 也提供了相當多的參數可以運用，例如說可以用 `--committer` 來找某個人的 Commit、用 `-S` 尋找主旨等等、用 `-2` 來限制只看兩筆等等，其他像是搜尋某個時間區段、某個特定檔案的變更紀錄也可以做到，甚至可以用 `--pretty` 來自訂 log 要顯示的格式，基本上該有的功能都有。

通常 `git log` 最常見的場景大概就像這樣：

- 「Ｘ！這鬼東西誰寫的？？？」
-  `git log ShitCodeController.cs` 
- 「噢，是我啊…」

這部份有興趣的朋友可以參考以下兩篇的說明，尤其是 為你自己學 Git 這篇裡面的狀況題相當實用，每天抓戰犯（？）的時候都會用到，掌握：
- [檢視紀錄 - 為你自己學 Git](https://gitbook.tw/chapters/using-git/log.html)
- [Git log 進階應用 - Jame's Blog](http://jamestw.logdown.com/posts/238719-advanced-git-log)

掌握 `git log`，每天都可以更刺激有趣！

![](https://i.imgur.com/9BYAkeV.png)

### 版本差異（Diff）

有些朋友可能會問：「我知道每一次 Commit 了，但我還是不知道每個 Commit 到底動了哪些地方呀？」

或是更進一步的：「我知道可以用 `git log -p` 來看每次 Commit 的內容啦，也知道變動了哪些地方應該要直接能從 Commit Message 大致看出來啦。但 log 的雜訊太多了，我只想要確切知道兩個 Commit 間到底變動了哪些，該怎麼做？」

這時候我們就可以**使用 `git diff` 來看兩個 Commit 之間的差異**。

現在讓我們回到 `git log` 示範時的狀況：

![](https://i.imgur.com/hD1c1h1.png)

可以看到我們有三個 Commit，分別是 a657, 6fbe, 2b33

> 小提示：在告訴 git 某個 Commit 的 SHA1 值的時候，並不需要完整打完，只要到可區分的程度就可以了，畢竟這東西真的超級難重複嘛。如果 Git 分不出來，就會請你再打一次，不用擔心。

接著讓我們輸入 `git diff a657 6fbe` （請根據你的 Commit SHA1 值輸入）

![](https://i.imgur.com/HLjl7aj.png)

就可以看到這兩個 Commit 間差異的內容囉！像是從上面的圖中，我們就可以知道 A.txt 有被修改過，其中被移除了 Hello 這一行，同時新增了 Hello World! 這一行。

除了兩個 Commit 之間以外，`git diff` 也可以查看兩個分支間的差異。

我們前面有提到過每次 Commit 的流程是 工作區＞暫存區＞儲存庫，那如果我們現在要看工作區和儲存庫之間的差異，也就是「我們現在變更了什麼」該怎麼做呢？

現在讓我們把 A.txt 的 Hello World! 的驚嘆號改成句號（可能我們不想那麼激動，想冷靜一點），但先不要 Commit：

![](https://i.imgur.com/8WlJXkE.png)

接著讓我們用 `git status` 確認一下：

![](https://i.imgur.com/zOf9MDZ.png)

可以看到工作區已經有變更了，但我們尚未 `add` 到暫存區裡。

現在讓我們**使用 `git diff` 指令來看工作區和儲存庫間的差異**：

![](https://i.imgur.com/T2ajGae.png)

能看到 Hello World! 和 Hello World. 的差異有確實出現。平時就可以用 `git diff` 來看這次到底都改了些什麼。

現在我們試試看先用 `git -add A.txt`，把 A.txt 加入到
暫存中，再呼叫 `git diff` 試試：
![](https://i.imgur.com/g97cuY9.png)

卻發現沒有找到任何差異了！這是因為 A.txt 已經被收入到暫存區中，不在工作區了。如果我們要**確認暫存區和儲存庫之間的差異，要加上 `--cached` 參數**，現在再用 `git diff --cached` 試試：

![](https://i.imgur.com/POr2E1J.png)

就可以看到暫存區和儲存庫之間的差異囉。

最後，示範完了就養成好習慣，順便 Commit 來結束這一小節吧：

![](https://i.imgur.com/UdDzOOZ.png)

`git diff` 的使用場景相當簡單，最常用來確認版本間的差異，或是檢查當前變更的內容。雖然 Diff 常常跑出來落落長一大串，不過現在的 GUI 都做得相當一目了然了：

![](https://i.imgur.com/dpdLhZo.png)

如果手癢要自製 diff 介面的朋友也可以參考黑大的這篇 [Git 筆記 - 產生程式異動對照表(Compare List)](https://blog.darkthread.net/blog/diff2html-webpage/)。

另外，如果一次 Commit 包含的檔案太多，也可以像 `got log` 一樣加上檔名來比較單一檔案的差異，或是使用 `--stat` 來檢視簡單的變動檔案列表等等。

diff 相關的文章也可以參照這幾篇，也是本節的主要參考資料：
- [git diff 命令](https://www.1ju.org/git/git-diff)
- [30 天精通 Git 版本控管 (09)：比對檔案與版本差異](https://ithelp.ithome.com.tw/articles/10135441)
- [Git 版本控制系統 - 比對檔案版本差異與標示說明](https://awdr74100.github.io/2020-04-27-git-diff/)

但是每次 `diff` 的時候，我都還要查出兩個版本的 SHA1 碼，如果我只是要看這一版和前一版的差異，這麼簡單的場景卻要弄得那麼複雜，不是很麻煩嗎？這時候我們就可以借助 `HEAD` 的力量，讓語法變得更簡單！

### 關於 HEAD

從上面的許多操作中，例如 [顯示 Commit Log 時](#歷史紀錄log)，都可以看到 HEAD 這個關鍵字，例如 (HEAD -> master)。

**HEAD 基本上可以當作「目前位置」的概念，它是一個會指向當前分支的指標。**

通常來說，HEAD 會指向目前所在的分支最新的一個 Commit（除非發生[斷頭](#關於斷頭detached-head)）。

但由於我們還沒有認識分支，當然也沒有針對分支進行任何操作，因此 HEAD 就會指向我們的預設分支 master（Github 現在改叫做 main 了），所以現在直接把 HEAD 當成目前所在位置就可以了。

同時，也可以用 HEAD 搭配 `^`（上一個版本）、`~`（上幾個版本）來直接替代大多數需要給定 Commit SHA1 值的操作，例如說：

- `HEAD~5` = 目前位置往前五步
- `f74d^` = f74d 這個 Commit 的上一個 Commit 

這樣會讓整個 Git 指令操作變得相當簡便，當我們在 `git diff` 的時候，明明只是要看跟上一版或者前面幾版的差異，實在很不想再查 SHA1 碼，就可以用 `git diff HEAD HEAD^` 或是 `git diff HEAD HEAD~2` 的指令來加快查詢。

關於 HEAD 的延伸閱讀，可以參考這兩篇：
- [【冷知識】HEAD 是什麼東西？ - 為你自己學 Git](https://gitbook.tw/chapters/using-git/what-is-head.html)
- [深入 Git：HEAD refs - Titangene Blog](https://titangene.github.io/article/git-head-ref.html)

### Revert

> 「以前我沒得選，現在我想做個好人。」
> 
> 『好啊，去跟 Git 說，看他讓不讓你做好人』

既然我們已經學會存檔，也能看過去都存了哪些檔了，是時候該學學讀檔了吧！

大多數的遊戲都有「上一步」、「悔棋」、「恢復上一動！懷疑啊？」這類的動作，在 Git 中則是叫做 `Revert`。

不過「我這一步完全不算」，**`Revert` 比較像是「我再把棋子移回去就是了」的概念**。也就是說，雖然是悔棋，但是這個悔棋本身也算是一步的意思。

這個部份直接試看看會比較好了解，讓我們把 A.txt 打開，並且把內容改成

```
Hello world. HAHAHA.
```

緊接著直接 `Commit -a -m "HAHA"`，現在的 Git Log 應該會長得像這樣：

![](https://i.imgur.com/WWGzQQh.png)

然後可能我們去吃個飯，越想越不對勁，回來決定不要這次 Commit 了。

現在讓我們試試 Revert：

```
git revert HEAD  --no-edit
```

![](https://i.imgur.com/P7kJY5C.png)

接著再確認一次 Log：

![](https://i.imgur.com/lqLNNYX.png)

可以看到又多了一個 Commit，這個 Commit 用來撤回前一個 Commit。

![](https://i.imgur.com/PhPCFqm.png)

並且 A.txt 也變回沒有 HAHA 的版本囉。

> 補充：我們在前面 Revert 的時候，加上了 --no-edit 的參數，讓 Git 自動使用預設的 Revert 訊息去 Commit。
>
> 大多時候，Revert "XXX" 就很清楚表達要撤回這個 Commit 的意思了。但如果這次撤回有需要另外說明的部分，可以不加上 --no-edit 參數，會進入 Commit Message 的編輯頁面，包含這個 Commit 的變動內容，讓你輸入這次 Revert 的 Commit Message。輸入完之後就可以提交囉。
> 
> 如果你是誤入 Vim 的朋友，呃，保佑你能順利 :wq。跟我一樣認命看為你自己學 Git 的 [超簡明 Vim 操作介紹](https://gitbook.tw/chapters/command-line/vim-introduction.html) 吧，畢竟這是大多數人的必踩之坑…

如果突然又不想撤回了怎麼辦？你可以再 Revert 一次來 Revert 掉上個 Revert 藉此還原上個
 Revert 所 Revert 掉的 Commit，夠直覺吧！

> 「我識破你的識破！」—— 某風聲（桌遊）玩家

那如果並不是要 `Revert` 這種後悔一步的作法，不想要背負 Revert 的 Commit，而是想要重新出發，該怎麼辦呢？

這時候就要用 `Reset` 了。

### Reset

> 「要是能重來，我要選李白。幾百年前寫的 Bug 沒那麼多人幹」

`Reset` 的意思是 **把當前的位置 `Reset` 到新的位置**，就像時光機一樣，直接前往目標 Commit。

用起來有點像 Git 界的 `goto` 的感覺，現在就讓我們直接試試吧。

首先掌握一下狀況，現在我們的 Log 是這個樣子：

![](https://i.imgur.com/lqLNNYX.png)

然後 A.txt 裡面則是：

![](https://i.imgur.com/PhPCFqm.png)

現在假設我們想要取消前面對 A.txt 中 Hello 的所有操作，也就是回到還沒有做 `a657 fix: 修正 hello 為 hello world` 這個 Commit 之前。

我們可以從 `git log` 中看見還沒做 `a657` 的前一個 Commit 是 `6fbe Add gitignore`，或是我們也可以直接用 `a657^` 來直接指定 `a657` 的前一個 Commit。先讓我們用 `diff` 偷看一下目的地和當前的差異。

![](https://i.imgur.com/ltpNAm7.png)

可以看到 A.txt 的在當時的內容是 `Hello!`，這就是我們要的。這時候我們就可以使用 `reset`

```
git reset --hard a657^
```

![](https://i.imgur.com/TdP0vZf.png)

Git 跟我們說「你已經回到了 `6fbe` 這個 Commit 囉」接著讓我們確認看看 A.txt：

![](https://i.imgur.com/ijZA2K2.png)

的確也變回去了，大功告成。

眼尖的朋友應該已經發現了，我們在 `reset` 的時候，加上了 `--hard` 的參數，這是什麼意思呢？其實 `reset` 有分為三種模式：

- **Mixed：保留工作區，不保留暫存區**。沒有選模式的時候預設就是用 Mixed
- **Soft：保留工作區，也保留暫存區**。感覺會像是只是把 HEAD 往前移而已
- **Hard：不保留工作區，也不保留索引區**
    - 也就是所有變更都直接捨棄，完整地回到當時

用現在這個例子來說，我們知道 A.txt 的舊版會是 Hello!，新版會是 Hello world.，那麼這三個模式下就會變成：

- `Mixed`：A.txt 會是 `Hello world.` 但還沒有 Add 的狀態
- `Soft`：A.txt 會是 `Hello world.` 並且已經 Add 等待 Commit 的狀態
- `Hard`：A.txt 會是 `Hello!`

這三個模式可以用在不同的場景，例如說：

- 當我只是想要重新編輯 Commit Message，或是想要整理某些部分的 Code，就可以用 `Mixed` 或 `Soft`
- 當我發現一些東西不應該 Add 進去 Git 版控，就可以用 `Mixed`
- 當我打算把這整組 Commit 放生，什麼都不要了，只想著回到當時狀況，就可以用 `hard`

等等這種情景，再自己取捨一下要用哪種時空旅行方法囉。

關於 `reset` 和這三個模式，為你自己學 Git 的表格整理得很不錯。這邊加上延伸閱讀：
- [【狀況題】剛才的 Commit 後悔了，想要拆掉重做 - 為你自己學 Git](https://gitbook.tw/chapters/using-git/reset-commit.html)
- [【狀況題】不小心使用 hard 模式 Reset 了某個 Commit，救得回來嗎？ - 為你自己學 Git](https://gitbook.tw/chapters/using-git/restore-hard-reset-commit.html)

> 題外話：如果真的遇到要把目前的 Commit 整條捨棄回到某個時間點的狀況，建議還是用後面學到的分支，把不要的 Commit 先拉出一條分支留著一陣子。
>
> 經歷過一些已經宣告死刑的需求過陣子借屍還魂死灰復燃的神奇操作之後，真的會感謝 Git 救工程師於水火之中。阿彌陀佛。

> 補充：在 `reset` 的時候真的會強烈感覺到 Commit 的頻率和 Commit Message 的重要，想想前面 Commit 章節的那兩個例子：
> 
> 如果只有「專案初始化」、「功能完成」兩個 Commit，那 就算 `reset` 也無用武之地，如果 Commit 訊息一整排都是「Add」、「Add」、「Add」，那給你 `reset` 也不知道該往哪裡去。
> 
> 所以切記切記，魔鬼藏在 Commit 裡！

### 分支（Branch）

就算是超級英雄片也會有團隊合作各司其職的時候，程式開發上一定也會遇到多人合作的狀況，這時候我們就不能一路無腦 Commit 下去。

像是分組作業的時候，通常都會溝通好每個人負責的地方，最後再銜接起來；又或是要前往目的地的時候，可以大家各自約好用不同的方法前往，在目的地會合。在 Git 中，我們可以使用分支（Branch）的方式來達到一樣的效果。例如說，多個人的時候就會變成：

- 小明 拉了Ａ分支 負責開發甲功能
- 小華 拉了Ｂ分支 負責修復出問題的乙功能
- 小美 拉了Ｃ分支 負責開發另一個專案的丙功能

最後再進行合併。

就算是只有一個人開發，有時候也會遇到正在開發某一個功能的時候，發現其他功能的某個地方壞了，又不想混在一起做。這時候也可以用分支進行管理。

這個部份我推薦連猴子都能懂的 Git 指南中的 [什麼是分支？](https://backlog.com/git-tutorial/tw/stepup/stepup1_1.html) 這頁的[圖例](https://backlog.com/git-tutorial/tw/img/post/stepup/capture_stepup1_1_2.png)，可以看到各自開發不同功能之後，合併在一起取得有全部變更的網頁。

分支在大多範例中會被形容像是樹枝的分枝，從主幹發散出去。但我個人覺得比較像是**水流**，從任何一處我們都可以岔分出另一條水流，這兩條水流各自經歷了不同的地方，可能第一條水流帶來了泥沙，第二條河流被汙染了核廢料，而我們又能將這兩條水流會合在一起，得到同時有泥沙和核廢料的水流。

而個人認為，分支最重要的概念就是進行**環境的隔離**，大家可以在自己的分支上進行作業，又不會互相影響。就算想修改看看，也可以在自己的分支上先行測試，沒問題了再合併上去，這樣就能在工作流程上貫徹封裝、降耦合和單一職責的精神。並且，也因為多條分支可以同時進行不同的工作，也能達到平行處理般的效率，重要的是戰犯更好找了，豈不妙哉。

那麼，我們現在就來記錄一下分支的基本操作吧。

#### 新建分支（branch）與切換分支（checkout）

我們在預設狀況下，就會位於 Master（有些地方是 Main）分支上，首先讓我們從新建分支與切換分支開始嘗試。在 `hello-git` 中，新增兩個檔案：`B.txt` 和 `C.txt`，現在資料夾應該會是這個樣子：

![](https://i.imgur.com/gEWbVkJ.png)

接著將這兩個新檔案 Add 進來並且 Commit：

![](https://i.imgur.com/qXwq3WC.png)

可以看到我們的 master 分支位於最新的 Commit 07672f4 上了，接著我們要從這裡<br/> **用 `git branch` 拉出一條新的分支**：

```
git branch Branch-B
```

> 補充：**分支拉出來的 Commit 點稱為 `Base`**。由於任兩個 Branch 一定會找得到一個共同的 base，因此我們在對分支間做合併和比較時，並不是將整份拿出來做大量核對，而是從這個 Base 當作基準點來進行變更的比較。這個 base 的概念和我們後續的 [合併](#合併merge)、[衝突](#衝突conflict) 等操作會比較相關。

建立分支後，我們再**使用 `git checkout` 簽出分支**：

```
git checkout Branch-B
```

應該可以看到 Git 告訴你已經切換到 Branch-B 了，我們也可以用 `git status` 來進行確認：

![](https://i.imgur.com/mrektkG.png)

要注意，checkout 只能在已經 Commit 的情況下進行。如果萬不得已必須中斷手上工作切換到其他分支的時候，可以使用 `stach` 來做暫存的動作。

關於 `stach` 的使用方式，由於我個人比較少用，故不再贅述，可以參照以下兩篇：

- [Stash暫存 · GIT教學](https://kingofamani.gitbooks.io/git-teach/content/chapter_3_branch/stash.html)
- [菜鳥工程師 肉豬: Git stash 暫存正在修改的內容](https://matthung0807.blogspot.com/2019/11/git-stash.html)


> 題外話：用 `branch` 來建立新分支應該相當直覺，但為什麼是用 `checkout` 來切換分支呢？
> 
> 這是因為在 Git 的觀念中，比較像是「**在圖書館的櫃台，我們去借出某一本指定的書，完事後再歸還回去**」這樣的概念，因此我們才會使用 `checkout` 簽出的方式來取出我們要的分支。

確認我們現在是在 Branch-B 分支之後，現在請把 B.txt 刪除，並且 Commit：

![](https://i.imgur.com/2cxGS7c.png)

接著，讓我們切換回 Master 分支：

```
git checkout master
```

觀察一下，B.txt 是不是復活了？

現在我們可以再練習一次：新建一條 Branch-C 並簽出，並且刪除 C.txt 後 Commit。

如果覺得每次都要先新建分支，再跳過去做兩步很麻煩的朋友，可以試試 `git checkout -b Branch-C`，就可以在簽出的同時建立分支囉。

![](https://i.imgur.com/o68UUxJ.png)

接著移除 C.txt 之後 Commit：

![](https://i.imgur.com/r5eNNrE.png)

現在，可以試著用 `checkout` 在 Branch-B 和 Branch-C 之間來回切換並觀察一下，就能稍微體會到分支之力囉。

延伸閱讀：

- [【狀況題】我可以從過去的某個 Commit 再長一個新的分支出來嗎？ - 為你自己學 Git](https://gitbook.tw/chapters/branch/branch-from-old-commit.html)

#### 合併（Merge）

> 「真是太諷刺了紹安，你拉了新分支繞了一大圈，<br/>　最後做出來的 feature 竟然是你不想做的，你老闆的需求。<br/>
> 　所以說呢，分支最後終究是要回到 master 來的，<br/>　這四千個 Commit 的盡頭 Merge，或許正是你的極限也說不定。」

不管是新功能的開發，問題的修復，效能的測試等等，**大多數以上的分支最終都是需要合併回來的**。每個專案大概就像那種機器人合體戰隊的動畫的每一集一樣，最後都要來 Merge 一下，可以說是避無可避。

我們在前面一節有開出了兩個分支：Branch-B 和 Branch-C，現在讓我們把他們合而為一。首先，讓我們先簽出到 Branch-B：

```
git checkout Branch-B
```

接著**使用 `git merge` 指令來用 Branch-B 分支把 Branch-C 分支給吃進來**：

![](https://i.imgur.com/9G1qj6g.png)

這時候可以觀察看看 `hello-git` 資料夾，B.txt 和 C.txt 應該都已經消失了。

我們可以用 `git log` 來觀察一下：

![](https://i.imgur.com/XHzq9yk.png)

可以看見，我們在 Branch-C 所做的 `Delete C.txt` 這個 Commit 也出現在 Branch-B 的記錄中的，並且 C.txt 也確實消失了。

這邊可以注意，當我們用 Branch-B 把 Branch-C 吃進來的時候，Branch-C 本身並沒有任何改變，仍然停留在 `Delete C.txt` 這個 Commit 上，而用來吃掉對方的 Branch-B 則取得了所有 Branch-C 的變更。

所以合併的時候要注意一下當前的分支，是**將目標分支匯入到當前所在的分支**，不要弄反了，不然把一堆測試用的東西合併上去正式環境就完蛋啦。

現在我們再把 Branch-B 的變更也合併進 master 吧。

同樣地，先簽出 master，再 `git merge Branch-B` 

![](https://i.imgur.com/pEySmri.png)

這樣就成功將兩個負責不同項目的分支裡的變更都合併回 master 囉！

#### 衝突（Conflict）

當然，不是每一次合併都能順利，就像不是每次合作都能順利一樣。只要兩個分支**有共同增刪改同一份文件同一個區塊，在合併的時候就會發生衝突**。

衝突的時候我們得要開啟檔案逐一審查有衝突的區塊，並選擇使用對方還是自己還是手動合併成新的版本。

現在讓我們來實際操作看看吧：

首先，讓我們先確認自己在 master，然後新建並簽出一支新的分支 `Branch-X`：

```
git checkout -b Branch-X
```

然後，將 A.txt 的內容改成 `Hello X!`：

![](https://i.imgur.com/WeFQiSM.png)

並且 Commit：

```
git commit -a -m "Hello X!"
```

緊接著，讓我們回到 master，重新建立並簽出另一支分支 `Branch-Y`：

```
git checkout master
git checkout -b Branch-Y
```

這次我們把 A.txt 的內容改成 `Hello Y!`：

![](https://i.imgur.com/us5hqzk.png)

並且 Commit：
```
git commit -a -m "Hello Y!"
```

現在，我們手頭上有 Branch-X 和 Branch-Y 兩條分支了，並且他們都變更過了 A.txt，現在我們直接用 Branch-Y 來合併 Branch-X：

```
git merge Branch-X
```

![](https://i.imgur.com/4j9GJmA.png)

可以看到 Git 跳出訊息跟你說**自動合併失敗（Automatic merge failed）**，要求你進行修復。

並且 `git status` 也會告訴你，你有未處理的 merge 問題，A.txt 這個檔案都有被變更到：

![](https://i.imgur.com/14o8dB9.png)


這時候讓我們去看看 A.txt 的內容：

![](https://i.imgur.com/etAf0gj.png)

會看到 Git 標示出發生衝突的部分，上半部是我們當前 HEAD 也就是 Branch-Y 的內容，下部分則是 Branch-X 的內容。

我們**必須把這個衝突的部分處理好，並且把這些標示拿掉，再重新 Commit 一次**。假設Ｘ跟Ｙ兩位同事經過 ~~扭打~~ 討論之後，決定改成兩個人的名字都要標在上面，變成 `Hello X & Y!`，我們就可以進行 commit 來結束衝突囉：

![](https://i.imgur.com/1HXkePJ.png)

通常來說，如果是使用 GUI 工具的朋友，應該會像 `Diff` 那樣給一個對照的列表讓你選擇，例如說我平常用的 Visual Studio 就會並排讓你勾選要使用哪一邊的，或是自己修改（這邊借用一下官方文件的圖）：

![](https://docs.microsoft.com/zh-tw/visualstudio/ide/media/git-merge-editor.png?view=vs-2019)

又或是可以直接選擇「使用當前分支」、「使用對方分支」的這類方便選項。但要特別注意一點，就是衝突一定要好好解開再上傳，**絕對不要沒解衝突就直接 Commit 掉推上來造成大家困擾**，這個一定要特別記得，弄不好可是會折壽的，物理上的折壽。

通常來說，發生衝突的時候必須了解兩方修改的內容和意圖，並且協調好處理的方式，再編寫成程式碼並解除衝突。這時候我就很佩服幫忙合併的前輩，我真的會看到眼花。

> 補充：如果衝突的檔案不是文字檔而是圖片檔，又或者是接下來要說明的 rebase 造成的衝突，處理的方式會不太一樣。可以參考為你自己學 Git 的這篇 [合併發生衝突了，怎麼辦？](https://gitbook.tw/chapters/branch/fix-conflict.html)

#### Rebase 

在處理分支的時候還有另一種合併方式，就是使用 Rebase。

我們在前面分支的章節有稍微提起過，分支的 base 是建立出分支的 Commit，也是分支和其他分支比較的基準點。**而 rebase 就是「重新設定基準點」的意思**。

剛剛在 merge 的時候，我們已經將 Branch-X 併入了 Branch-Y ，現在就試試用 rebase 的方式，將 Branch-X 併入 master 吧。

```
git checkout master
git rebase Branch-Y
```

這樣就會將 master 的 base 重新設定到 Branch-Y 分支上囉，因為我們的狀況比較簡單，所以比較像是 master 推進到了  Branch-Y 的進度，讓我們用 `git log` 確認一下當前的狀況吧：

![](https://i.imgur.com/sBPfJRR.png)

可以看到剛剛在 Branch-Y 上的操作，例如解衝突的 Commit 都已經進到 master 來了。

Rebase 跟 Merge 最明顯的差別在於 Rebase 是將這個分支 **逐步移植** 到另一個分支上，而不是像 Merge 將兩條水流引流成一條，所以並不會有合併時的 Commit。

但也由於是從切出分支的基準點開始做移植和計算的動作，我個人是覺得相對比較危險的，例如說你的 Git 分支樹就會和別人的那一份產生差異等等，畢竟這是一個變更歷史的行為，因此還是小心使用比較好，我個人是盡量能用 Merge 就用 Merge 的流派啦。

關於 Rebase 的部分，為你自己學 Git 的這篇 [另一種合併方式（使用 rebase）](https://gitbook.tw/chapters/branch/merge-with-rebase.html) 相當詳細，有影片還有圖解及 rebase 的步驟說明，失敗救回的方法。如果有需要用到 rebase 來處理，可以先稍微閱讀一下。

還有 Rebase 和 Merge 兩種合併方法的差異，可以參見猴子都會的 Git 的這篇 [分支的合併](https://backlog.com/git-tutorial/tw/stepup/stepup1_4.html)，有針對 Merge 和 Rebase 代表的操作進行圖解。

此外，Rebase 也可以用來修改先前的 Commit 之類的，例如搭配 `squash` 來把多個 Commit 壓縮成一個 Commit，請參見 [使用 rebase -i 合併提交](https://backlog.com/git-tutorial/tw/stepup/stepup7_6.html)、[送 PR 前，使用 Git rebase 來整理你的 commit 吧！](https://medium.com/starbugs/use-git-interactive-rebase-to-organize-commits-85e692b46dd)

#### 查看分支列表、刪除分支

現在我們針對分支的基本操作已經告一段落。可以用 `git branch` 來稍微看一下都開了哪些分支。

![](https://i.imgur.com/QPcfALx.png)

前面加上 * 號的就是當前所在的分支，如果已經和遠端儲存庫連線的朋友，也可以使用 `-r` 參數來看遠端儲存庫上的分支，用 `-a` 來看所有分支，遠端儲存庫的分支將會用 `remotes/` 開頭。

由於前面示範合併和衝突的分支們的工作都已經告一段落了，我們現在就要稍微清理一下它們。要刪除分支，只需要加上 `-d` 參數就可以了，像現在這個例子就是：

```
git branch -d Branch-B
git branch -d Branch-C
git branch -d Branch-X
git branch -d Branch-Y
```

![](https://i.imgur.com/CaYIfDt.png)

接著再讓我們用 `git branch` 確認一下吧：

![](https://i.imgur.com/2aslDOI.png)

#### 關於斷頭（detached HEAD）

我們在前面切換分支的時候，主要是使用 `checkout` 來進行。但如果我們並不是針對某個 branch 去做 `checkout`，而是對某個 commit 去做的時候，就會變成斷頭狀態。

因為 HEAD 會指向當前的分支，所以當我們用 checkout 並不是用來切換分支，而是單純回到某個 Commit 的時候，Git 實際上是幫我們建立一個未命名的分支，這時候如果又做了一些事情並 Commit ，又來回跳（例如說 `checkout` 到別的地方去），我們就會找不到這個仍未取名的陌生分支，並遺失掉這些變更。

這也就是為什麼上面的 checkout 範例，會有一大段警告，告訴你你的 HEAD 掉了，現在在斷頭狀態。

這時候如果要做什麼變更，就開條新分支吧。畢竟，[分支標籤不用錢，成本超低廉](https://gitbook.tw/chapters/branch/why-branch-is-cheap.html)，各位還是在分支間跳來跳去吧，至少跳完了找得回來嘛。

延伸閱讀：
- [淺入 Git：detached HEAD - Titangene Blog](https://titangene.github.io/article/git-detached-head.html)
- [【冷知識】斷頭（detached HEAD）是怎麼一回事？ - 為你自己學 Git](https://gitbook.tw/chapters/faq/detached-head.html)
- [【冷知識】為什麼大家都說在 Git 開分支「很便宜」？ - 為你自己學 Git](https://gitbook.tw/chapters/branch/why-branch-is-cheap.html)

#### 關於分支策略

既然是團隊合作，雖然大家各自拉了一條分支出去做事，但還是要有點 SOP，這就叫做分支策略。

以最常見的 Git Flow 來說，**會有兩個最重要的分支：正式服務的主要分支（master, main）、開發用的測試分支（develop）。接著再衍生出相關的分支，例如 功能（feature）、修復（hotfix）等等**。

當然針對這些分支，也會有一些相關的規定，主要會有命名規定和合併的規定。例如微軟的分支命名建議就比較像 `{分支種類}/{人員}/{描述}`，例如說小明拉了一個產品功能的分支，就是 `feature/Ming/product` 等等。

流程的部分就有多個流派，目前我們常用的方式和上面提到的 Git Flow 比較相像，是從 master 拉出 develop，然後在 develop 上拉出多個 feature 進行開發。

這些 feature 分支開發完畢後，再匯回 develop 進行測試，當專案完成、develop 的測試通過後，就可以推上 master。

而當線上有問題發生的時候，會從 master 拉出 hotfix 分支，並修復完成之後同步更新給 master 和 develop。藉此保持 master 分支的穩定和乾淨。

不過當然這是我所屬的團隊常用的方法，這些流程還是很吃團隊文化的，記得入境隨俗哪。

關於這些分支的介紹和協作，可以參照這些文章：

- [Git Flow 是什麼？為什麼需要這種東西？](https://gitbook.tw/chapters/gitflow/why-need-git-flow.html)
- [淺談 Git Flow 與 commit 規範 | Welcome.Web.World (hsiangfeng.github.io)](https://hsiangfeng.github.io/git/20200914/1124442109/)

另外，關於分支的處理和觀念，特別推薦這個系列，獲益良多：

- [團隊的 GIT 分支管理策略 (1) ： 基本概念](https://medium.com/%E5%93%88%E5%98%8D-%E4%B8%96%E7%95%8C/%E5%9C%98%E9%9A%8A%E7%9A%84-git-%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86%E7%AD%96%E7%95%A5-449bc229c957)
- [團隊的 GIT 分支管理策略 (2) ： 主線整合與功能分支](https://medium.com/%E5%93%88%E5%98%8D-%E4%B8%96%E7%95%8C/%E5%9C%98%E9%9A%8A%E7%9A%84-git-%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86%E7%AD%96%E7%95%A5-2-%E6%95%B4%E5%90%88%E9%A0%BB%E7%8E%87%E5%B0%8D%E5%9C%98%E9%9A%8A%E6%95%88%E7%8E%87%E7%9A%84%E5%BD%B1%E9%9F%BF-bedbbdd3e70e)

### 遠端儲存庫（Remote）、推送（Push）

> 開始本章節之前，你可能需要先註冊好 [Github](https://github.com/)，註冊過程挺簡單的，不用擔心

既然都已經使用 Git 了，當然要推送到遠端儲存庫啦！作為儲存庫的服務有蠻多的，例如 Github、GitLab、Gitea 等等，本篇會以最知名的工程師交友網站 Github 為例。

首先讓我們在 Github 上新建一個儲存庫，從 My Repositories 或是首頁進去都可以：

![](https://i.imgur.com/RXL3p7f.png)

接著讓我們填寫一些基本訊息，像是專案名稱和專案敘述：

![](https://i.imgur.com/6b4sjKx.png)

底下會有一些選項，像是是否加入 .gitignore 啦、是否加入 readme（說明文檔）等等，這邊暫時還不會用到。

完成後就可以按下 `Create Repository`，接著就會來到專案啟動畫面：

![](https://i.imgur.com/ykHLsko.png)

這個畫面已經說明了不同狀況下的操作，像我們已經在本機已經存在儲存庫了，所以我們可以參考「…or push an existing repository from the command line」這個部分進行操作。

其中最重要的是上面那串 `.git` 結尾的網址，我們接著就要使用這個網址來將剛剛的 `hello-git` 資料夾推送到這個儲存庫。

現在讓我們鏡頭回到棚內的 git 指令，首先 `git status` 確保 Commit 等動作都已經完成：

![](https://i.imgur.com/Nci6Yvd.png)

接著讓我們**用 `git remote add` 來增加叫做 `origin` 的遠端儲存庫到這個儲存庫中**，完成了之後再用 `git remote` 確認遠端儲存庫列表是不是已經有 `origin` 了：

```
git remote add origin https://github.com/yourGithubName/yourGithubRepository.git
git remote
```

![](https://i.imgur.com/WFqrEAu.png)

因為 Github 的主要分支已經改成使用 `Main` 了，因此我們順應一下，也用 `git branch -M` 把 master 分支改名成 Main 吧

```
git branch -M main
```

![](https://i.imgur.com/vNFIUiQ.png)

完成之後，就可以開始上傳囉。在 Git 中，**推送到遠端儲存庫只要使用 `push` 指令就可以了，語法是 `git push {遠端儲存庫名稱} {要推送的分支}`**，例如我們現在的推送就是：

```
git push origin main
```

![](https://i.imgur.com/Ubdt85H.png)

推送完成後，讓我們再回到 Github 上查看：

![](https://i.imgur.com/SNLyXDy.png)

可以看見東西都已經推送上去了。這時候按下旁邊的 Commit，也能夠查看這個儲存庫的 Commit Log 囉：

![](https://i.imgur.com/DY9AStm.png)

到這邊就成功把存檔弄上雲端啦！

> 補充：如果儲存庫曾經有用 `reset` 之類的往後跳，並且遠端儲存庫的版本又相對較新，例如遠端是第五版，抓下來之後 `reset` 回到第三版，這時候的 `push` 就會被擋下來，要求取得遠端最新的版本。
>
> 諸如此類的狀況，造成無法推送的時候，如果 十、分、確、定 手上的這份才是對的，必須推送上去，可以使用 `-f` 參數來進行強制推送。可以參見：[狀況題】聽說 git push -f 這個指令很可怕，什麼情況可以使用它呢？](https://gitbook.tw/chapters/github/using-force-push.html)
>
> 不過說實在的，還是祈禱不會遇到需要 `-f` 的那一天吧…

### 擷取（Fatch）、提取（Pull）

上傳到雲端已經沒問題了，那從雲端下載到本機呢？這時候我們就需要用到 `fetch` 和 `pull` 這兩個指令。

現在讓我們回到 Github 的畫面，並且進入 A.txt

![](https://i.imgur.com/cVtjDCx.png)

然後按下編輯：

![](https://i.imgur.com/9SU7PR1.png)

把內文稍微修改一下，例如改成 `Hello X & Y & Z!`

![](https://i.imgur.com/TRv8i93.png)

拉到最下面，加上 Commit Message，並且按下 Commit Changes

![](https://i.imgur.com/GRpH0el.png)

現在遠端儲存庫已經有了一個變更，但是我們本機還不知道呢！接著就讓我們實際操作一下，首先我們需要取得遠端的資訊，這時候就可以**用 `fetch` 來擷取遠端儲存庫的資訊**回來：

```
git fetch
```

![](https://i.imgur.com/InviqZr.png)

可以看見 Git 從遠端儲存庫抓了資訊回來，這時候我們如果查看 `git log`，並加上 `--all` 來顯示全部分支的話：

![](https://i.imgur.com/JBEalid.png)

可以看見有一個 `origin/main` 的分支超越了我們的 `main` 分支了，這也就是在 `origin` 遠端儲存庫的 `main` 分支的意思。這時候我們就可以知道，遠端儲存庫的進度比起我們本機的還要更新。

那麼要怎麼讓本機的進度追上呢？其實 `main` 和 `origin/main` 也就是兩條分支，所以只要使用 `merge` 就可以把進度往前推。

當然我們也可以不用這麼麻煩，可以直接**使用 `pull` 指令來提取遠端儲存庫對應的分支直接和本機現在的分支進行合併，也就是 `fetch` + `merge`**。這樣就是從雲端下載最新檔囉！現在就讓我們試試吧：

```
git pull origin main
```

`pull` 語法的邏輯和 `push` 是一樣的。當我們 `pull` 下來後，就可以 `git log` 來確認一下記錄囉：

![](https://i.imgur.com/945NY8u.png)

可以看到我們的 `main` 分支已經跟上 `origin/main` 的進度囉！

### Clone

上面我們已經嘗試過「本機有儲存庫，上傳到遠端儲存庫」的場景了。但是大多數時候，像是我們在公司會需要接手開發專案啦、使用人家已經寫好的工具啦等等，都是「**本機沒有任何東西，要從遠端進行下載**」的場景。

這個時候就是 Github 大家最常做的 `Clone` 出場的時候啦！

現在就讓我們再新增一個 `GitRepos` 資料夾，我們的目標就是把儲存庫拉下來這裡：

![](https://i.imgur.com/ryaEGB2.png)

接著讓我們回到 Github 上的 Repository，畫面中間靠右會有一個綠色的 `↓ Code` 按鈕（這應該是整個 Github 大家按最多次的按鈕）點開就會出現下載選項：

![](https://i.imgur.com/WY6HnKT.png)

分別是各個命令列下載方式對應的連結、Github Desktop 專用的開啟方式，還有最常用到的 ZIP 下載方式。

我個人是比較常下載壓縮檔來解壓縮啦，不過都已經用指令到這裡了，就讓我們來試試 **`git clone`** 吧：

```
git clone https://github.com/yourGithubName/yourGithubRepository.git
```

![](https://i.imgur.com/TmPSidi.png)

接著在 `GitRepos` 裡面就生出來一個 `hello-git` 啦，進入後檔案也都在呢：

![](https://i.imgur.com/qJ9RVQN.png)

這樣就成功把儲存庫 Clone 下來囉！

**Clone 下來的儲存庫會自動建立和遠端儲存庫的繫結**，也就是已經是 `git remote add ` 好的狀態，相當方便。接著後續就和基本的 Git 操作一樣囉。

### 關於提取要求（pull request）

如果你 Clone 的是自己的儲存庫，當然就可以開始工作後 `Add` `Commit` `Push` 連發，但如果不是你自己的儲存庫，又想要幫忙修改東西，或是工作上有審核機制，該怎麼辦呢？

這個時候我們就要用提取要求（pull request）的方式來互動啦。

**Pull request 就是指發出一個 request 請對方來 Pull，如果對方覺得 OK，就會同意提取要求並把變更合併到自己的儲存庫裡**。不過發 PR 這個動作似乎在每個平台都有點微妙的不同。

例如說平常工作時候使用 TFS（現在叫做 Azure DevOps）的發 PR 方式，是將分支推送上去之後，發起 PR 並請主管審核，確認沒問題後再進行 Merge；但在 Github 上，則是要先叉（Fork）一份到自己家，Clone 下來修改完之後推上去，再發出 PR 請對方來提取合併。

PR 比較常見於社群協作，還有工作上的提交審核等等，也由於這個部分已經是社群互動的進階動作了，故先按下不表。附上 Azure DevOps 和 Github 的 Pull Request 操作方式的介紹文章，有興趣的朋友可以去看看：

- [與其它開發者的互動 - 使用 Pull Request（PR）- 為你自己學 Git](https://gitbook.tw/chapters/github/pull-request.html)
- [[02][讓團隊彼此知道程式碼走向]何爲Pull Request並且如何建立 - 以Azure DevOps爲例](https://blog.alantsai.net/posts/2019/05/code-review-02-what-is-pull-request-and-how-to-create-it-in-azure-devops)


### 小結

本來只是想介紹一下 Git 的一些基本操作，沒想到越弄越長（汗），儘管如此，還是有很多說明不到的地方（例如提取要求、分支策略等等），留待各位實戰的時候細細體會了。

最後還是要說句，真的要買一本為你自己學 Git 放在手邊哪，有夠實用。感謝作者，感謝網路上的各位大大，感謝公司圖書櫃，各位一生平安。

那麼，這篇就先到這裡啦，這個系列終於還是開坑了，希望這次也能順順利利了…吧。那麼，我們下次見！

### 本系列文章

- [菜雞新訓記 (0): 目錄](https://igouist.github.io/post/2021/04/newbie-0-menu)
- [菜雞新訓記 (1): Git 入門這樣做](https://igouist.github.io/post/2021/04/newbie-1-hello-git/)

### 參考資料

- [為你自己學 Git](https://gitbook.tw/)
- [連猴子都能懂的 Git 入門指南](https://backlog.com/git-tutorial/tw/)
- [版本控制使用 Git](https://www.tenlong.com.tw/products/9789862766699)
- [30 天精通 Git 版本控管](https://ithelp.ithome.com.tw/users/20004901/ironman/525)
- [Pro Git](https://git-scm.com/book/zh-tw/v2)
- [黑暗執行緒的 Git 分類文章](https://blog.darkthread.net/blog/category/Git)
- [黑暗執行緒的 Git 指令筆記](https://blog.darkthread.net/blog/my-git-cheatsheet/)
- [SLMT's Blog | 漏洞筆記 - 別讓你的 .git 資料夾公開在網路上啊！](https://www.slmt.tw/blog/2016/08/21/dont-expose-your-git-dir/)
- [Git Commit Message 這樣寫會更好，替專案引入規範與範例 (wadehuanglearning.blogspot.com)](https://wadehuanglearning.blogspot.com/2019/05/commit-commit-commit-why-what-commit.html)
- [如何寫一個 Git Commit Message | louie_lu's blog](https://blog.louie.lu/2017/03/21/如何寫一個-git-commit-message/)
- [git diff命令 - Git教程教學 | 程式教程網 (1ju.org)](https://www.1ju.org/git/git-diff)
- [30 天精通 Git 版本控管 (09)：比對檔案與版本差異 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天 (ithome.com.tw)](https://ithelp.ithome.com.tw/articles/10135441)
- [Git - 查看提交历史 (git-scm.com)](https://git-scm.com/book/zh/v2/Git-基础-查看提交历史)
- [Git 筆記 - 產生程式異動對照表(Compare List)-黑暗執行緒 (darkthread.net)](https://blog.darkthread.net/blog/diff2html-webpage/)
- [Git 版本控制系統 - 比對檔案版本差異與標示說明 | Roya's Blog (awdr74100.github.io)](https://awdr74100.github.io/2020-04-27-git-diff/)
- [git checkout 移動 HEAD 指標 - Git 分支(branch) | W3HexSchool](https://w3c.hexschool.com/git/9a164fbe)
- [深入 Git：HEAD refs | Titangene Blog](https://titangene.github.io/article/git-head-ref.html)
- [淺入 Git：detached HEAD | Titangene Blog](https://titangene.github.io/article/git-detached-head.html)
- [集中式vs分布式 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600/896202780297248)
- [送 PR 前，使用 Git rebase 來整理你的 commit 吧！ - 星巴哥技術專欄](https://medium.com/starbugs/use-git-interactive-rebase-to-organize-commits-85e692b46dd)
- [PowerShell | git log 中文乱码问题解决](https://blog.csdn.net/FollowGodSteps/article/details/96271359)
- [Git log 進階應用 - Jame’s Blog](http://jamestw.logdown.com/posts/238719-advanced-git-log)
- [Stash暫存 · GIT教學](https://kingofamani.gitbooks.io/git-teach/content/chapter_3_branch/stash.html)
- [菜鳥工程師 肉豬: Git stash 暫存正在修改的內容](https://matthung0807.blogspot.com/2019/11/git-stash.html)
- [Git上的三種工作流程 - 儲思盆 | Pensieve](https://medium.com/i-think-so-i-live/git%E4%B8%8A%E7%9A%84%E4%B8%89%E7%A8%AE%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B-10f4f915167e)
- [Git flow 分支策略 - Practical guide for git users](https://git-tutorial.readthedocs.io/zh/latest/branchingmodel.html)
- [TFS Git 筆記 - 分支管理策略 - 黑暗執行緒](https://blog.darkthread.net/blog/git-branching-strategies/)
- [Git flow 分支管理策略 - 程序員大本營](https://www.pianshen.com/article/30471944392/) 
- [團隊的 GIT 分支管理策略 (1) ： 基本概念](https://medium.com/%E5%93%88%E5%98%8D-%E4%B8%96%E7%95%8C/%E5%9C%98%E9%9A%8A%E7%9A%84-git-%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86%E7%AD%96%E7%95%A5-449bc229c957)
- [團隊的 GIT 分支管理策略 (2) ： 主線整合與功能分支](https://medium.com/%E5%93%88%E5%98%8D-%E4%B8%96%E7%95%8C/%E5%9C%98%E9%9A%8A%E7%9A%84-git-%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86%E7%AD%96%E7%95%A5-2-%E6%95%B4%E5%90%88%E9%A0%BB%E7%8E%87%E5%B0%8D%E5%9C%98%E9%9A%8A%E6%95%88%E7%8E%87%E7%9A%84%E5%BD%B1%E9%9F%BF-bedbbdd3e70e)
- [Git Flow 是什麼？為什麼需要這種東西？](https://gitbook.tw/chapters/gitflow/why-need-git-flow.html)
- [淺談 Git Flow 與 commit 規範 | Welcome.Web.World (hsiangfeng.github.io)](https://hsiangfeng.github.io/git/20200914/1124442109/)
- [與其它開發者的互動 - 使用 Pull Request（PR）- 為你自己學 Git](https://gitbook.tw/chapters/github/pull-request.html)
- [[02][讓團隊彼此知道程式碼走向]何爲Pull Request並且如何建立 - 以Azure DevOps爲例](https://blog.alantsai.net/posts/2019/05/code-review-02-what-is-pull-request-and-how-to-create-it-in-azure-devops)
