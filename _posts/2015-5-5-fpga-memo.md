---
layout: post
title: FPGAメモ3個目-de0-nanoというFPGAを買ってみたのでおだやかに戯れる
---

今回はラジコンサーボ用のPWMを生成してみます．  
前回1秒ごとのLチカをやってみましたが，オシロで見たら変な感じでした．1秒になって無くなんちゃってでした．  
**でも今回はちゃんとした波形になってます．**(もろたで工藤!)  

とりあえずラジコンサーボなんてだいたい20msecだろってノリでやってきます．  
以下にソースとその説明とかいろいろ(基本ソースは使い回し)  


	// module宣言
	// pwmのテスト

	module pwm_test (
		LED,
		KEY,
		CLOCK,
		PIN,
	);

	output reg [1:0] LED = 0;
	input KEY;
	input CLOCK;
	output reg PIN=0;
	reg [25:0]counter_time=0;

	always@ (posedge CLOCK)begin

	        // 20msec経過したらリセット
	        if(counter_time >= 26'd01000000)begin
		counter_time <= 26'd0;
		end
		
		// デューティ比の後半
		else if(counter_time > 26'd00500000)begin
		PIN <= 0;
		counter_time <= counter_time + 26'd1;
		end
		
		// デューティ比の前半
		else if(counter_time <= 26'd00500000)begin
		counter_time <= counter_time + 26'd1;
		PIN <= 1;
		end

	end

	endmodule

ソースの説明  
PINは適当なGPIOに割り振っています．  
今回は20msecがほしいのでcounter_timeをつかって26'd01000000まで数えます．  
26'd01000000=20msec，26'd49999999=1secです．(よね？？？)  
次にデューティ比ですがまずは50%でやってみます．  
なのでcounter_timeで0から26'd00500000まではHigh，26'd00500000から26'd01000000まではLowになります．んで26'd01000000以上になったらcounter_timeをリセットしてまた数えます．  

実際のオシロの波形はこんなかんじです．  

![pinplanner](/images/duty50.jpg)

また26'd00500000を26'd00700000に変えるとデューティ比70%となります．  

![pinplanner](/images/duty70.jpg)

最後にpinplanner  

![pinplanner](/images/pwmtest.PNG)


ん〜、もっとスマートに書けるかな？  











