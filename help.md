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

(参考)[http://www.backlog.jp/git-guide/stepup/stepup1_1.html]


## 作成したブランチをリモートにpushする

新規にhogeブランチを作成して移動する

	git checkout -b hoge

hogeブランチで作業する

	git add hoge.txt
	git commit -m "add hoge.txt"

このhogeブランチはローカルリポジトリに作成されている  
hogeブランチを誰かと共有したければ、リモートリポジトリにpushする必要がある

	git push origin hoge


> (参考)[http://blog.basyura.org/entry/20100323/p1]


## リモートにpushしたブランチをpullする

新規にhogeブランチを作成して移動する

	git checkout -b hoge

hogeブランチで作業する

	git add hoge.txt
	git commit -m "add hoge.txt"

hogeブランチをリモートにpushする

	git push -u origin hoge

> -uをつけることで、hogeブランチをpullすることができるようになる

誰かがhogeブランチにpushしたとする  
hogeブランチをpullする

	git pull

> (参考)[http://rcmdnk.github.io/blog/2014/01/31/computer-git/]


## 新規ブランチの作成時、作業ディレクトリ内に未コミットの変更があったら?

masterブランチに移動

	git checkout master

作業ディレクトリのhoge.txtに対して変更を加える

作業ディレクトリの状態を確認

	git status

> hoge.txtが、変更が加えられた未コミットのファイルとして表示される

新規ブランチhogeを作成する

	git branch hoge

> masterブランチのHEADを基点としてhogeブランチが作成される  
> 作業ディレクトリ内では、hoge.txtの状態は変わらない

hogeブランチに移動する

	git checkout hoge

> 作業ディレクトリ内では、hoge.txtの状態は変わらない
> hoge.txtの変更を、hogeブランチにコミット可能


## ブランチを削除する

hogeブランチを削除する

	git branch -d hoge


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


## マージ時に--no-ffを指定するかどうか

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

> hoge.txtの変更を2回コミットした

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
> HEADが移動したことにより、hogeブランチでhoge.txtの変更を2回コミットしたログ`07bc284`,`baa9914`が、masterブランチのログでも確認可能となった  
> しかし、このログを見る限りでは、`07bc284`,`baa9914` が、hogeブランチからマージされたものであるとは気づけない  
> この2つのログは、masterブランチへダイレクトにコミットされたものに見える  
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

> マージ後のmaterブランチのログを見ると、hogeブランチの`d694717`,`9b51383`をfast-forwardで取り込んだ後に、`0d4327f`のコミットを行っている  
> hogeブランチから`d694717`,`9b51383`をマージしたことが、`0d4327f`のコミットログで残っている  
> つまり、--no-ffでマージした場合、ログにhogeブランチの存在が残る (もしhogeブランチが削除されても、hogeブランチからマージされたことがわかる)


