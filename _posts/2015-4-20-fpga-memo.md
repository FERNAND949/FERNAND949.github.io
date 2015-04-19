---
layout: post
title: FPGA����2��-de0-nano�Ƃ���FPGA�𔃂��Ă݂��̂ł����₩�ɋY���
---

����̓^�N�g�X�C�b�`�ł�L�`�J�ƃ^�C�}�[���g���Ă�L�`�J�ł��D  
�����Ԏg����悤�ɂȂ��Ă�������(������)  

�܂��ŏ��̓^�N�g�X�C�b�`���g���܂��D  
�^�N�g�X�C�b�`���q�����Ă�s����**FPGA����1��**������Ƃ��āC��H�}������ƃv���A�b�v����Ă�D  


�ȉ��̃\�[�X�͑O��̂��̂ɂ���т��ƒǉ��������̂ł�

	// module�錾
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


�O��̃\�[�X��input KEY���ǉ����ꂽ�����ł��D  
���������ƁC�X�C�b�`�������ĂȂ��ƃv���A�b�v��High�Ȃ̂�if�ɓ����ď����Ă�D  
������Low�ɂȂ�̂�else�ɓ����Č���D  
pin planner�̐ݒ�͈�Ԃ����Ƀ^�C�}�[���g���Ă�̂Ƃ܂Ƃ߂ĉ摜�ōڂ��Ă��܂��D  



���Ƀ^�C�}�[���g����l�`�J�D  
de0-nano�ɂ�50MHz�̃N���b�N���o�Ă�̂ł�����g���D  
�Ƃ肠������������1�b���Ƃ�LED���_�ł���悤�ɂ��Ă݂�D  

50MHz��2�i���ɂ��ĉ��r�b�g�ɂȂ�̂��v�Z���Ă݂��26bit�ɂȂ�D  
�������g����1Hz(�P�b�𐔂���)���̂����D  

�ȉ��\�[�X�R�[�h


	// module�錾
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

�\�[�X�̉��  
��{�I�ɂ͍��܂Ŏg���Ă����̂��g����  

�܂��N���b�N�̒l��������Ă���̂�CLOCK_50  
����counter_time�����𐔂��邽�߂̂��́C�O�q�ɂ��26bit�K�v  
��always�̂Ƃ��낪
		
	always @(posedge CLOCK_50)begin

�ƂȂ��Ă��邪�C�����CLOCK_50�̗����オ��G�b�W�̎��ɂ��̏����ɓ���D  
�t�ɗ���������̎���negedge�ɂ���D  
���g�̏�����else�̕��Ő��𐔂��āC��b��������if�̕��ɓ�����LED�̒l�𔽓]����̂�counter_time�̏�����������D  
�r�b�g�̒l������Ƃ�
		
	counter_time <= 26'd0;

�E�ӂ̒l����26��bit���Cd��10�i���C2�i���̏ꍇ��b,16�i����h�Dbit�ƃA���t�@�x�b�g�Ƃɂ�'���K�v�D���ׂ̗�0���l.  


pin planner�̐ݒ�͈ȉ��̒ʂ�
![pinplanner](/images/fpga_pinplanner_clock_50.PNG)

�܂������ƂȂ�ƂȂ��킩���
�ELED�̒l�𔽓]����Ƃ���Verilog HDL�ł�~��not����������!�ł��ł��Ă�  
�Ealways���ł�<=�����ӂɑ��  
�E�^�C�}�[�̏��Ŏ����Ă͖��������̃u���O�Ƃ���CLOCK_50�̏���ʂ̖��O�ł������ł��Ȃ�����������(�������΂�����  








