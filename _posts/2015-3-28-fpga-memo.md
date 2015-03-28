---
layout: post
title: 第1回de0-nanoというfpgaを買ってみたので遊んでみる
---

FPGAおもしろそうだなと思いDE0-nanoが安かったので買ってみました．最近少しずつやっているのでそのメモ．  

さあ，最初にやることは何でしょう？  
はいそうですね．もちろんLチカです．  
以下プロジェクトの作り方からプログラムの作成と書き込みまでのメモ．環境はWindows7 64bitです．  
(今回は長くなりそうだな～)  


ここに書いてあるのは(多分本家にpdfがあると思うけど)私がDE0-nanoを買った時についてきたCDに入ってたDE0-Nano_My_First_Fpga_v1.0.pdfっ英語で書かれたpdfから取ってきた内容も含みます．  


まずはプロジェクトの作りかた  

・File > New > NewってウィンドウでNew Quartus Ⅱ Projectをクリック  
・New Project Wizard(下の図)でまずNext  
![create1](https://github.com/FERNAND949/FERNAND949.github.io/blob/master/_images/project_create01.PNG)

・[page 1 of 5]上から、プロジェクトを保存するフォルダ、プロジェクトの名前、最後はプロジェクト名を入力すると勝手に入力される  
![create2](https://github.com/FERNAND949/FERNAND949.github.io/blob/master/_images/project_create02.PNG)


・[page 2 of 5]はNext  
![create3](https://github.com/FERNAND949/FERNAND949.github.io/blob/master/_images/project_create03.PNG)


・[page 3 of 5]ここでは使用するfpgaの種類を選択。今回はDE0-nanoを使用しているので以下のようにしてます。  

Family: Cyclone Ⅳ E  
Target device: Specific device～(略)  
Avalable devices:  
Name:EP4CE2F17C6  
んでFinish  

![create4](https://github.com/FERNAND949/FERNAND949.github.io/blob/master/_images/project_create04.PNG)


これでプロジェクトができた  


次にLチカ用のプログラムを作成します．この時につまったこととかもメモ．  
言語はVerilog HDL．私は今までc/c++，c#(ちょっと)，とかしかやってなかったのでほとんど知識がないです．一応動作は確認済です．  
File > New > NewってウィンドウでVerilog HDL Fileを選択  
以下ソースコード(適当にLEDを2個位光らせてみる)  

	module led_test (
		LED,	
	);

	output reg [1:0] LED = 0;

	always 
	
		begin

		LED[0] = 1;
		LED[1] = 1;

		end

	endmodule

以下ソースの説明  

	module 名前(入出力ポート名);
	～
	回路の記述
	～
	endmodule

verilog-HDLの場合はこのmoduleからendmoduleの間にプログラム(回路)を書く.  
次に  

	input
	output

これは入力信号と出力信号．今回はLEDを光らせるだけなのでoutput  
次に  

	wire もしくは wire[MSB:LSB]
	reg  もしくは reg[MSB:LSB]

wireがネット宣言,regがレジスタ宣言  
wire宣言はモジュール内で使用する信号線の定義．assign 文のみ使用可能.  
reg宣言は値を保持できる信号線を定義．always 文，initial 文，task，function で使用可能  

次に[MSB:LSB]の部分は配列の宣言みたいなもの,とりあえず0で初期化.  


次にalwaysの部分，cで言うwhile文みたいなもんだと思ってます．処理の内容が1文だけなら問題ないが，大抵は複数になるでしょう．そんな時は beginとendの間に処理を書く．  
alwaysの後に何か書くっぽいけど無視していくスタイル．別になくてもいいらしいがココらへんは後で(やるとは言ってない)  

一応LEDとボタンのピンアサインも載せておく．  
基板のシルクと比較すると  

LED7個  
LED[0]=[PIN_A15]  
LED[1]=[PIN_A13]  
LED[2]=[PIN_B13]  
LED[3]=[PIN_A11]  
LED[4]=[PIN_D1]  
LED[5]=[PIN_F3]  
LED[6]=[PIN_B1]  
LED[7]=[PIN_L3]  

タクトスイッチ  
KEY0=[PIN_J15]  
KEY1=[PIN_E1]  

と配線されている  


保存したらツールバーのAssignments > Pin Plannerを起動.  
locationのところをダブルクリックでピンの選択．今回は0番と1番.  
![create5](https://github.com/FERNAND949/FERNAND949.github.io/blob/master/_images/pin_planner.PNG)


そしたらQuartusの方に戻ってツールバーのProcessing > Start Compilation もしくはStart > Start Analysis & Elaboration(どっちが何だったが忘れたが今回はソースのみなので前者でできるはず)   
警告は沢山出るけどエラーが無ければ良しとしよう!  

最後に書き込み  
ツールバーのTools > Programmer  
Hardware Setupを選択してUSB-Blasterに設定．後はfileがちゃんと作ったのが選ばれてるのかを見てStart．  
![create6](https://github.com/FERNAND949/FERNAND949.github.io/blob/master/_images/Programmer.PNG)

Successfulと出れば成功．写真のようになってるはず．  
![create7](https://github.com/FERNAND949/FERNAND949.github.io/blob/master/_images/de0-nano-Lchika.jpg)


ちなみに電源落としたら消えますよ  

長かった，メモとるのも一苦労だな  


