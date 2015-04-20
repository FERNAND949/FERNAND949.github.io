---
layout: post
title: FPGAメモ2個目-de0-nanoというFPGAを買ってみたのでおだやかに戯れる
---

今回はタクトスイッチでのLチカとタイマーを使ってのLチカです．  
だいぶ使えるようになってきたかな(小並感)  

まず最初はタクトスイッチを使います．  
タクトスイッチが繋がってるピンは**FPGAメモ1個目**を見るとして，回路図を見るとプルアップされてる．  

以下のソースは前回のものにちょびっと追加したものです

	// module宣言
	module led_test (
		LED,
		KEY,
	);

	output reg [1:0] LED = 0;
	input KEY;

	always begin

		if(KEY) begin
		LED[0] = 0;
		LED[1] = 0;
		end
	 
		else begin	
		LED[0] = 1;
		LED[1] = 1;
		end
	end

	endmodule


前回のソースにinput KEYが追加されただけです．  
解説をすると，スイッチを押してないとプルアップでHighなのでifに入って消えてる．  
押すとLowになるのでelseに入って光る．  
pin plannerの設定は一番したにタイマーを使ってるのとまとめて画像で載せています．  



次にタイマーを使ったlチカ．  
de0-nanoには50MHzのクロックが出てるのでそれを使う．  
とりあえず分周して1秒ごとにLEDが点滅するようにしてみる．  

50MHzを2進数にして何ビットになるのか計算してみると26bitになる．  
これらを使って1Hz(１秒を数える)ものを作る．  

以下ソースコード


	// module宣言
	module led_test (
		LED,
		KEY,
		CLOCK_50,
	);

	output reg [1:0] LED = 0;
	input KEY;
	input CLOCK_50;
	reg [25:0]counter_time=0;

	always @(posedge CLOCK_50)begin

		if(counter_time==26'd49999999)begin
		counter_time <= 26'd0;
		LED[0] <= !LED[0];
		end
		
		else counter_time <= counter_time + 26'd1;

	end

	endmodule

ソースの解説  
基本的には今まで使ってきたのを使い回し  

まずクロックの値をもらっているのがCLOCK_50  
次にcounter_timeが数を数えるためのもの，前述により26bit必要  
でalwaysのところが
		
	always @(posedge CLOCK_50)begin

となっているが，これはCLOCK_50の立ち上がりエッジの時にこの処理に入る．  
逆に立ち下がりの時はnegedgeにする．  
中身の処理はelseの方で数を数えて，一秒立ったらifの方に入ってLEDの値を反転するのとcounter_timeの初期化をする．  
ビットの値を入れるとき
		
	counter_time <= 26'd0;

右辺の値だが26がbit数，dが10進数，2進数の場合はb,16進数はh．bitとアルファベットとには'が必要．その隣の0が値.  


pin plannerの設定は以下の通り
![pinplanner](/images/fpga_pinplanner_clock_50.PNG)

つまった所となんとなくわからん所
・LEDの値を反転するときにVerilog HDLでは‾がnotだそうだが!でもできてる  
・always文では<=が左辺に代入  
・タイマーの所で試しては無いが他のブログとかでCLOCK_50の所を別の名前でやったらできなかったそうな(←試せばいだろ  








