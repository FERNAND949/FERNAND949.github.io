---
layout: post
title: ROSというダークマター2個目-catkin_makeができなくなった！！！+githubとsshの設定メモ
---

turtlebotをいじっている時に突然catkin_makeできなくなった！なんとなく原因はわかってる．  

以下やってしまったこと．  
1・自分でノードを作ってcatkin_makeする．  
   ちなみに作ったパッケージはturtlebot/src/ディレクトリに作成．  
2・実行ファイル名をミスったので修正してcatkin_makeする．  
3・rosrunで実行するときにミスした時のが残ってる．なんか気持ち悪い．  
4・消し方がわからないので、編集したCMakeLists.txtとソースファイルはコピーして残しておきパッケージを作り直す．(コピー先をturtlebot/src/へしてしまった)  
5・この時turtlebot/src/に元からあるCMakeLists.txtを書き換えてしまったorz. 完全にこれが原因だと思う．  
6・そのままcatkin_makeしたらturtlebotだけでなく，他のワークスペースでもcatkin_makeできなくなってしまった.  


パッケージのpathも通し直してやったがダメだったのでインストールし直したがどうしてもインストールできない．パッケージのpathを通すsetup.~が無い.  
依存関係がなんやかんやしてるのだろうが，5回くらい試してもダメだったので最終手段のUbuntuをクリーンインストールした．  
まあインストールスクリプトとか作ってあるのでそれを試したかったのもあるが・・・(←完全なる言い訳  
とりあえず環境は無事復活した．インストールスクリプトでダメだった所とか足りないのとか追加できたので良しとしよう．  

それにしても，たったそれだけでダメになるなんてなんて，まあ私が無知なだけで何か解決策があったでしょう．(マジで何なのこのクソみたいなシステムは！)  



以下はクリーンインストールし直した時にgitとsshの設定を保存し忘れたのでその設定のメモ  
以下にそのコマンドを記載  

githubのユーザーとメールの設定  

	 git config --global user.name "user"  
	 git config --global user.email mail.com  

userはgithubのユーザー名,mail.comは登録したメールアドレス  
おそらくホームディレクトリに.gitcongigが出来ていて中には  

	[user]
		name = user
		email = mail.com

と書かれているはず.  

次にsshの設定  

	ssh-keygen -t rsa -C "email@example.com"

"email@example.com"はgithubのメールアドレス  
そのままの流れでEnterを押していく．途中パスワードの設定があるが無視していく.  
そうするとホームディレクトリに.sshディレクトリが作成され，その中にはid_rsaとid_rsa.pubが作成されている.  

次にgithubでしようするsshを登録する．  
	
	ssh-add ~/.ssh/id_rsa

最後にgithubのページでsshを登録するところにid_rsa.pubの中身をコピペして登録  

最後につながるかどうか確認のコマンド  
	
	ssh -T git@github.com

打ったあとに  

	省略
	〜
	RSA key fingerprint is 16:27:ac:~(長いので省略)
	Are you sure you want to continue connecting (yes/no)? yes

最終的に上記のようなyes/noを求められるのがでてきたらyes  

	Hi githubのユーザー名! You've successfully authenticated, but GitHub does not provide shell access.

こんなのが出てきたら成功.  
もしダメだったら以下のを試す.  

ポートが問題の場合  
~/.ssh/にconfigファイルを作成．中身は  


	Host github.com
    	    User git
      	    Hostname ssh.github.com
    	    Port 443
    	    IdentityFile ~/.ssh/id_rsa

このファイルの中身は上記のまま変更する部分が無い.  

実行権限の場合  

	chmod 600 ~/.ssh/config

をやって見るといいらしい．これはなったことがないのでわからないが・・・














