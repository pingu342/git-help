# Gitコマンド例集

## masterブランチのHEADを基点として新規ブランチを作り、新規ブランチに移動する

masterブランチに移動する

	git checkout master

作業ディレクトリの「現在のブランチ」を確認する

	git branch

> masterブランチのHEADに居ることを確認する

新規にhogeブランチを作成する

	git branch hoge

> 作成されたhogeブランチのHEADは、masterブランチのHEADと同じ

hogeブランチに移動する

	git checkout hoge

## 新規ブランチの作成と移動を一気に行う

	git checkout -b hoge

[参考](http://www.backlog.jp/git-guide/stepup/stepup1_1.html)


## 共有ファイルサーバー上にリモートリポジトリを作成してpushする

まずローカル側で、ローカルリポジトリを作成する

	mkdir hoge
	cd hoge
	git init
	echo "hoge" > hoge.txt
	git add hoge.txt
	git commit -m "initial"

共有ファイルサーバー側で、hogeという名前のリモートリポジトリを作成する

	mkdir hoge.git
	cd hoge.git
	git init --bare --shared

ローカル側で、リモートリポジトリ`hoge.git`をローカルリポジトリにoriginという名前で追加する  

	git remote add origin /path/to/hoge.git
	git remote
	git remote show origin

ローカルリポジトリのmasterブランチをリモートリポジトリ`origin`にpushする

	git push origin master


## 共有ファイルサーバー上のリモートリポジトリからローカルリポジトリを作成する

cloneする

	git clone /path/to/hoge.git

> [Gitサーバー - Localプロトコル](https://git-scm.com/book/ja/v1/Git-%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC-%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB)


## 作成したブランチをリモートにpushする

新規にhogeブランチを作成して移動する

	git checkout -b hoge

hogeブランチで作業する

	git add hoge.txt
	git commit -m "add hoge.txt"

このhogeブランチはローカルリポジトリに作成されている  
hogeブランチを誰かと共有したければ、リモートリポジトリにpushする必要がある

	git push origin hoge


> [参考](http://blog.basyura.org/entry/20100323/p1)


## リモートにpushしたブランチをpullする

新規にhogeブランチを作成して移動する

	git checkout -b hoge

hogeブランチで作業する

	git add hoge.txt
	git commit -m "add hoge.txt"

hogeブランチをリモートにhogeブランチという名前でpushする

	git push -u origin hoge

> ローカルとは異なるブランチ名を付けることができる  
> また、-uオプションを付けることで、リモートからhogeブランチをpullできるようになる

自分以外がリモートのhogeブランチに変更をpushする  
その変更を取り込むために、hogeブランチをpullする

	git pull

> [参考](http://rcmdnk.github.io/blog/2014/01/31/computer-git/)


## 誰かがリモートにpushしたブランチをpullする

リモートに存在するブランチを確認する

	git branch -r

ローカルに新しくブランチを作って、そこにリモートのブランチを持ってくる  
ブランチ名はリモートとローカルで異なっていても良い(ローカルのブランチ名に、リモートのブランチ名とは異なる名前をつけてよい)

	git branch new-branch origin/new-branch

> new-branchのチェックアウトは行われないので、ローカルの作業ディレクトリはnew-branchに移動しない

新しいブランチの作成と、リモートからのチェックアウトを同時に行う

	git checkout -b new-branch origin/new-branch

> new-branchのチェックアウトも行われるので、ローカルの作業ディレクトリがnew-branchに移動する

ローカルの既存のブランチに、リモートのブランチをpullするには、pullする前に、--set-upstream-toでローカルのブランチをリモートのブランチに接続する必要がある

	git branch --set-upstream-to=origin/new-branch new-branch
	git branch -vv
	git pull


## 新規ブランチの作成時、作業ディレクトリ内に未commitの変更があったら?

masterブランチに移動

	git checkout master

作業ディレクトリのhoge.txtに対して変更を加える

作業ディレクトリの状態を確認

	git status

> hoge.txtが、変更が加えられた未commitのファイルとして表示される

新規ブランチhogeを作成する

	git branch hoge

> masterブランチのHEADを基点としてhogeブランチが作成される  
> 作業ディレクトリ内では、hoge.txtの状態は変わらない

hogeブランチに移動する

	git checkout hoge

> 作業ディレクトリ内では、hoge.txtの状態は変わらない
> hoge.txtの変更を、hogeブランチにcommit可能


## ローカルのブランチを削除する

ローカルのhogeブランチを削除する

	git branch -d hoge

> もしhogeブランチがリモートリポジトリにpushされていた場合、リモートリポジトリ上のhogeブランチは削除されていない  


## リモートのブランチを削除する

リモートのhogeブランチを削除する

	git push --delete origin hoge


## リモートのブランチをローカルに取得する

クローンする

	git clone file://path/to/remote.git

ブランチのリストを確認する

	git branch --all
	* master
	  remotes/origin/HEAD -> origin/master
	  remotes/origin/hoge

> クローン直後、ローカルにはmasterブランチのみ  
> リモートにはhogeブランチがある

リモートのhogeブランチを、ローカルのhogeブランチとしてチェックアウトし、hogeブランチに移動する

	git checkout -b hoge origin/hoge

> ローカルのブランチ名には、リモートのブランチ名とは異なる名前を付けても良い  
> 例えば、リモートのhogeブランチを、ローカルのhoge1ブランチとしてチェックアウトできる  
> 更にその後に、名前を変えて、ローカルのhoge2ブランチとしてチェックアウトできる

## ブランチやコミットを渡り歩く

	git checkout <branch>
	git checkout <commit>

> branchの場合、そのブランチのHEADに移動する（ブランチの移動とは、各ブランチのHEADを行き来することである）  
> commitの場合、そのコミットに移動する (detatched HEADとなる)


## hogeブランチの変更をmasterブランチにマージする

masterブランチに移動する

	git checkout master

hogeブランチを作成して移動する

	git checkout -b hoge

hogeブランチで作業する

	git add .
	git commit -m "..."
	git push origin hoge

masterブランチに移動する

	git checkout master

マージする

	git merge --no-ff hoge

> --no-ffオプション：fast-forwardの関係であっても、必ずマージコミットを作る


## fast-forwardとはなにか

### fast-forwardが可能なケース

masterブランチから、hogeブランチを作成する

hogeブランチに変更をcommitする

この間、masterブランチは変更しない（なにもcommitしない）

hogeブランチの変更を、masterブランチにマージする

このケースのマージは、masterブランチのHEADを、hogeブランチのHEADに移動するだけで、マージが完了

すなわち、fast-forwardが可能


### fast-forwardが不可能なケース

masterブランチから、hogeブランチを作成する

hogeブランチに変更をcommitする

この間、masterブランチにも変更をcommitする

hogeブランチの変更を、masterブランチにマージする

このケースのマージは、masterブランチのHEADを、hogeブランチのHEADに移動するだけでは、マージが完了しない

すなわち、fast-forwardが不可能

このケースでは、masterブランチにcommitされた変更と、hogeブランチにcommitされた変更を手動でマージしたものを、masterブランチにcommitする

つまり、masterブランチとhogeブランチを合流させる

> [参考](http://linux.keicode.com/prog/git-resolve-non-fast-forward-push-problem.php)


## fast-forwardが可能なケースのマージ時に--no-ffを指定するとどうなるか

masterブランチに移動して作業する

	git checkout master
	git add hoge.txt
	git commit -m "add hoge.txt"

hogeブランチを作成して移動する

	git checkout -b hoge

hogeブランチで作業する

	git add hoge.txt
	git commit -m "change hoge.txt"
	git add hoge.txt
	git commit -m "change hoge.txt"
	git push origin hoge

> hoge.txtの変更を2回commitした

masterブランチに移動する

	git checkout master

(a-1) fast-forwardでマージする

	git merge hoge

(a-2) マージ後のmasterブランチのログを確認する

	git checkout masger
	Switched to branch 'master'

	git log
	commit baa9914e088d9776db7cc3b01ae74d04e95d089d
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:51:01 2016 +0900

		change hoge.txt

	commit 07bc28462d52aea2388b26b1c35d73c369f11c08
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:50:38 2016 +0900

		change hoge.txt

	commit f6134e209f59d5cb289b08b4b3c211be281cc618
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:50:08 2016 +0900

		add hoge.txt

(a-3) マージ後のhogeブランチのログを確認する

	git checkout hoge
	Switched to branch 'hoge'

	git log
	commit baa9914e088d9776db7cc3b01ae74d04e95d089d
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:51:01 2016 +0900

		change hoge.txt

	commit 07bc28462d52aea2388b26b1c35d73c369f11c08
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:50:38 2016 +0900

		change hoge.txt

	commit f6134e209f59d5cb289b08b4b3c211be281cc618
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:50:08 2016 +0900

		add hoge.txt

> マージ前、masterブランチのHEADは、`f6134e2`であった  
> マージ後、masterブランチのHEADは、hogeブランチのHEADである`baa9914`に移動している  
> HEADが移動したことにより、hogeブランチでhoge.txtの変更を2回commitしたログ`07bc284`,`baa9914`が、masterブランチのログでも確認可能となった  
> しかし、このログを見る限りでは、`07bc284`,`baa9914` が、hogeブランチからマージされたものであるとは気づけない  
> この2つのログは、masterブランチへダイレクトにcommitされたものに見える  
> つまり、fast-forwardでマージした場合、ログにhogeブランチの存在が残らない（もしhogeブランチが削除されたら、`07bc284`,`baa9914` が、hogeブランチからマージされたことが、完全にわからなくなる)
>

(b-1) `--no-ff`でマージする

	git merge --no-ff hoge

(b-2) マージ後のmasterブランチのログを確認する

	git checkout masger
	Switched to branch 'master'

	git log
	commit 0d4327fc10b35de61f4c0a0f91c1e5b6f2a9b511
	Merge: a27ce20 9b51383
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:58:13 2016 +0900

		Merge branch 'hoge'

	commit 9b51383a0b4c8ffc17a98ec4c41db18bea77114c
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:57:44 2016 +0900

		change hoge.txt

	commit d694717fba39181fb3b690bb2dd10d9517bae6e8
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:57:37 2016 +0900

		change hoge.txt

	commit a27ce201683a2675d94ce8d012b7bbed65494ada
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:57:13 2016 +0900

		add hoge.txt

(b-3) マージ後のhogeブランチのログを確認する

	git checkout hoge
	Switched to branch 'hoge'

	git log
	commit 9b51383a0b4c8ffc17a98ec4c41db18bea77114c
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:57:44 2016 +0900

		change hoge.txt

	commit d694717fba39181fb3b690bb2dd10d9517bae6e8
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:57:37 2016 +0900

		change hoge.txt

	commit a27ce201683a2675d94ce8d012b7bbed65494ada
	Author: pingu342 <pingu342@users.noreply.github.com>
	Date:   Tue Aug 2 14:57:13 2016 +0900

		add hoge.txt

> マージ後のmaterブランチのログを見ると、hogeブランチの`d694717`,`9b51383`をfast-forwardで取り込んだ後に、`0d4327f`のcommitを行っている  
> hogeブランチから`d694717`,`9b51383`をマージしたことが、`0d4327f`のコミットログで残っている  
> つまり、--no-ffでマージした場合、ログにhogeブランチの存在が残る (もしhogeブランチが削除されても、hogeブランチからマージされたことがわかる)

## ファイルを特定のcommitに戻す

	git checkout commit番号 ファイル


## リモートにPushしようとしたらRejectされたときの対処

リモートのmasterブランチをpullする

ローカルのmasterブランチに変更をcommitする

その間、リモートのmasterブランチに、他の誰かが変更をcommitする

リモートのmasterブランチに変更をcommitする

このケースでは、fast-forwardによるマージができないので、Rejectされる

変更をマージする手順

	git fetch

	git merge origin/master

	git mergetool

	git commit -m "Fixed conflict"

	git push

> [参考](http://linux.keicode.com/prog/git-resolve-non-fast-forward-push-problem.php)


## 競合している変更をリモートからpullする

Aさんが競合する変更をリモートにpushする

リモートの変更履歴を確認する

	git fetch
	git log origin/master

> `git fetch`はデフォルトでoriginをfetchする

fetchはリモートから変更履歴とタグを取得する

Aさんの変更をマージする

	git merge origin/master

pull = fetch + merge である

> [参考](http://www.backlog.jp/git-guide/stepup/stepup3_2.html)


## コミットのauthorを書き換える

まずgit configを修正する

	git config --local user.name nanashi
	git config --local user.email nanashi@mail.com

直近のコミットだけなら、

	git commit --amend --reset-author

> [参考](http://sohtaro.com/blog/2017/06/11/git-author-email-replace/)


過去のすべてのコミットが対象なら、

	git filter-branch -f --env-filter \
	"GIT_AUTHOR_NAME='nanashi'; \
	 GIT_AUTHOR_EMAIL='nanashi@mail.com'; \
	 GIT_COMMITTER_NAME='nanashi'; \
	 GIT_COMMITTER_EMAIL='nanashi@mail.com';" \
	HEAD

> [参考](http://sohtaro.com/blog/2017/06/11/git-author-email-replace/)


## GitHubでFork/cloneしたリポジトリを本家リポジトリに追従する

	git remote add upstream https://github.com/xxx/yyy.git
	git remote -v
	git fetch upstream
	git merge upstream/master
	git push origin master

> [参考](https://qiita.com/xtetsuji/items/555a1ef19ed21ee42873)

