---
layout: post
title: ROSというダークマター３個目-turtlebotにプラグインを作って実装してみる
---

ROSで既存のシステムのある部分を自分で編集したい、または自作したものと置き換えたいって思ったんですよ．いや，やらねばならなくなったのだ．  
そんな時にどうすればいいのか？プラグインを作って置き換えるらしいとのこと・・・  

どうやって作ればいいのやら，チュートリアルはあるけどやった後にいざオリジナルのものを作ってやるとなると，う〜ん・・・  

ROSの**初心者**である私が結果的にプラグインとして置き換えることができたので，その時の手順を載せておきます．  
**どうせ忘れるからね!!**  

対象としたロボットはturtlebot  
今回プラグインとして置き換えたのはbase_local_plannerのdwa_local_plannerの部分を置き換えた．  

とりあえず参考URLとして以下のものを参考にした(一割くらい)  
<http://wiki.ros.org/navigation/Tutorials/Writing%20A%20Global%20Path%20Planner%20As%20Plugin%20in%20ROS>  
<http://wiki.ros.org/pluginlib>  
特に１個目の参考URLはGlobal_Plannerを自分で作ったのと置き換えようって感じでやっている．

まずはプラグイン用のパッケージを作りましょう、ということで何かパッケージを作ります．特に専用のコマンドとかはなく普通に作ります．  
ただ，今回はdwa_local_plannerの部分を置き換えるのでベースのクラスとしてbase_local_plannerとなります．  

なんと言うかゼロから作るのは面倒だし，dwaもプラグインとして作ってあるっぽいからそれを取ってきて書き換えればいいだろ理論でやってみた．  
githubにあるros_navigation見たいのをcloneしてその中にあるdwaのパッケージをturtlebot/src/にコピーしてきた．  

とりあえず書き換えた部分として以下のところだけ，  

フォルダ名  
~plugin.xmlのファイル名  
それに伴い，package.xmlのプラグインのexportの部分  
CMakeLists.txtのプロジェクトと実行ファイルの名前  
あと，置き換えられたかどうか確認したいのでsrc/にあるcppファイルのうちdwa_planner_ros.cppでノードの名前を変更  

で実行してみた結果  

置き換える前は  
![rostopic list before](/images/ros_plugin_before.png)

置き換えた後は  
![rostopic list after](/images/ros_plugin_after.png)


local_planの名前がフザけた名前になっているのがわかります．  

実際に速度を与えている部分をいじったりしても動きが変わっているのでこれで出来ている思われます．  




あ〜，漫画でわかるROSとか出ないかな・・・(一部の層には売れると思う)
















