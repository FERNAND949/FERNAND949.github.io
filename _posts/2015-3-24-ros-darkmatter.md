---
layout: post
title: ROSというダークマター１個目-turtlebotをいじってみる1
---

ROSを強いられたためインストールをした．  
やりたいことはナビゲーションの部分だけど，とりあえずチュートリアルをやった．  
んで早速どっかで躓いたけどどこか忘れてしまったorz(自分のためのメモとかいって置きながらこの有り様)．  
たしかこんなのできて当たり前だろとかそんな感じで進められててROSが早くもキライになりました．  
チュートリアルを終えて具体的に何しようかなみたいなので，とりあえずturtlebotをいじってみようかなみたいな・・・  
ROSと言ったらturtlebotのイメージがあったので．  


ここを見ながらソースからビルドしていった．
<http://wiki.ros.org/turtlebot/Tutorials/indigo/Turtlebot%20Installation>


実物は持ってないからできない部分(4.Navigate the playgroundとか)もあるがここを見ながらシミュレーション上で進めてい行く．  
<http://wiki.ros.org/turtlebot_gazebo/Tutorials/indigo/Explore%20the%20Gazebo%20world>  
<http://wiki.ros.osuosl.org/turtlebot_navigation/Tutorials/Autonomously%20navigate%20in%20a%20known%20map>  
バージョンにより多少コマンドに違いがあるので注意

tutorialを進めていて思ったのが，roslaunchを実行するときに，例えば2Dのマップでナビゲーションするときにgazeboの方を実機の代わりとして実行ずるけど，

roslaunch turtlebot_gazebo turtlebot_world.launch  
roslaunch turtlebot_navigation amcl_demo.launch map_file:=/tmp/my_map.yaml  

の２つのコマンドを実行するとき，実行するタイミングによってはcostmapがちゃんと読まれなかったり，gazeboと接続できなかったりした．

  何だこのクソ使用は・・・  

と思った．そんなんどこに書いてあんだよと．なんだよこのタイミングは，  
ROSは初心者一見様お断りだからしょうがないと言い聞かせる．
できたから良しとするか．


