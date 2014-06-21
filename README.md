Music-visualiser-using-FPGA
===========================

/*2 point dft*/

module dft2p(
    input clkmain,
	 input hfclk,
	 input rst2,
	 input [2:0]en,
	 input [3:0]ana,
	 output reg signed [15:0]r_1_2,r_2_2
    );

reg signed [15:0]first[7:0];
reg signed [15:0]second[7:0];
integer tp2;

initial
begin
   for (tp2=0;tp2<8;tp2=tp2+1) begin
      first[tp2]=16'd0;
		second[tp2]=16'd0;
   end
end

always @(posedge hfclk)
begin
    if (rst2==1'd1) begin
        for (tp2=0;tp2<8;tp2=tp2+1) begin
        first[tp2]<=0;
		  second[tp2]<=0;
        end
	 end
	 else begin
	 case(en)
	    0:begin
		   first[0]<=first[0]+ana;
			second[0]<=first[0]-ana;
		   end
		 1:begin
		   first[1]<=first[1]+ana;
			second[1]<=first[1]-ana;
			end
		 2:begin
		   first[2]<=first[2]+ana;
			second[2]<=first[2]-ana;
		   end
		 3:begin
		   first[3]<=first[3]+ana;
			second[3]<=first[3]-ana;
		   end
		 4:begin
		   first[4]<=first[4]+ana;
			second[4]<=first[4]-ana;
		   end
		 5:begin
		   first[5]<=first[5]+ana;
			second[5]<=first[5]-ana;
			end
		 6:begin
		   first[6]<=first[6]+ana;
			second[6]<=first[6]-ana;
		   end
		 7:begin
		   first[7]<=first[7]+ana;
			second[7]<=first[7]-ana;
		   end
	 endcase
	 end
end

always @(en)
begin
    case(en)
	    0:begin
		   r_1_2=first[0];
			r_2_2=second[0];
		   end
		 1:begin
		   r_1_2=first[1];
			r_2_2=second[1];
		   end
		 2:begin
		   r_1_2=first[2];
			r_2_2=second[2];
		   end
		 3:begin
		   r_1_2=first[3];
			r_2_2=second[3];
		   end
		 4:begin
		   r_1_2=first[4];
			r_2_2=second[4];
		   end
		 5:begin
		   r_1_2=first[5];
			r_2_2=second[5];
		   end
		 6:begin
		   r_1_2=first[6];
			r_2_2=second[6];
		   end
		 7:begin
		   r_1_2=first[7];
			r_2_2=second[7];
		   end
	 endcase
end

endmodule



 /*4point dft*/


module dft4p(
    input clkmain,
	 input hfclk,
	 input rst2,
	 input [2:0]en,
	 input [3:0]ana,
	 input [1:0]j,
	 
	 output signed [15:0]r_1_4,
	 output signed [15:0]r_2_4,
	 output signed [15:0]r_3_4,
	 output signed [15:0]r_4_4,
	 output signed [15:0]i_1_4,
	 output signed [15:0]i_2_4,
	 output signed [15:0]i_3_4,
	 output signed [15:0]i_4_4
    );

wire signed [7:0]wreal[1:0];
wire signed [7:0]wimag[1:0];

reg signed [15:0]evenr[1:0];
reg signed [15:0]oddr[1:0];

reg signed [15:0]first[3:0];
reg signed [15:0]second[3:0];
reg signed [15:0]third[3:0];
reg signed [15:0]fourth[3:0];
integer tp4;

wire signed [15:0]r_1_2,r_2_2;

dft2p dft201(
    .clkmain(clkmain),
	 .hfclk(hfclk),
	 .rst2(rst2),
	 .en(en),
	 .ana(ana),
	 .r_1_2(r_1_2),
	 .r_2_2(r_2_2)
);

assign wreal[0]=8'd1;
assign wimag[0]=8'd0;
assign wreal[1]=8'd0;
assign wimag[1]=-8'd1;

assign r_1_4=(evenr[0]+wreal[0]*oddr[0])/4;
assign i_1_4=(wimag[0]*oddr[0])/4;
assign r_2_4=(evenr[1]+wreal[1]*oddr[1])/4;
assign i_2_4=(wimag[1]*oddr[1])/4;
assign r_3_4=(evenr[0]-wreal[0]*oddr[0])/4;
assign i_3_4=(-wimag[0]*oddr[0])/4;
assign r_4_4=(evenr[1]-wreal[1]*oddr[1])/4;
assign i_4_4=(-wimag[1]*oddr[1])/4;

initial
begin
for (tp4=0;tp4<4;tp4=tp4+1) begin
   first[tp4]=16'd0;
	second[tp4]=16'd0;
	third[tp4]=16'd0;
	fourth[tp4]=16'd0;
end
end


always @(negedge clkmain)
begin
	 if ((hfclk==1'd1)&&(j==2'd1)) begin
	 case(en)
	   0:begin
        first[0]<=r_1_2;
        second[0]<=r_2_2;
        end
      1:begin
        third[0]<=r_1_2;
        fourth[0]<=r_2_2;
        end
		2:begin
        first[1]<=r_1_2;
        second[1]<=r_2_2;
        end
      3:begin
        third[1]<=r_1_2;
        fourth[1]<=r_2_2;
        end
		4:begin
        first[2]<=r_1_2;
        second[2]<=r_2_2;
        end
      5:begin
        third[2]<=r_1_2;
        fourth[2]<=r_2_2;
        end
		6:begin
        first[3]<=r_1_2;
        second[3]<=r_2_2;
        end
      7:begin
        third[3]<=r_1_2;
        fourth[3]<=r_2_2;
        end
	  endcase
    end
end

always @(en)
begin
    case(en[2:1])
	 0:begin
	   evenr[0]=first[0];
		evenr[1]=second[0];
		oddr[0]=third[0];
		oddr[1]=fourth[0];
	   end
	 1:begin
	   evenr[0]=first[1];
		evenr[1]=second[1];
		oddr[0]=third[1];
		oddr[1]=fourth[1];
	   end
	 2:begin
	   evenr[0]=first[2];
		evenr[1]=second[2];
		oddr[0]=third[2];
		oddr[1]=fourth[2];
	   end
	 3:begin
	   evenr[0]=first[3];
		evenr[1]=second[3];
		oddr[0]=third[3];
		oddr[1]=fourth[3];
	   end
	 endcase
end



endmodule


/* 8pt dft */



module dft8p(
    input clkmain,
	 input hfclk,
	 input rst2,
	 input [2:0]en,
	 input [3:0]ana,
	 input [1:0]j,
	 
	 output signed [15:0]r_1_8,r_2_8,r_3_8,r_4_8,r_5_8,r_6_8,r_7_8,r_8_8,
	 output signed [15:0]i_1_8,i_2_8,i_3_8,i_4_8,i_5_8,i_6_8,i_7_8,i_8_8
    );
wire signed [7:0]wreal[3:0];
wire signed [7:0]wimag[3:0];

wire signed [15:0]r_1_4,r_2_4,r_3_4,r_4_4;
wire signed [15:0]i_1_4,i_2_4,i_3_4,i_4_4;

reg signed [15:0]zero4r[3:0];
reg signed [15:0]zero4i[3:0];
reg signed [15:0]one4r[3:0];
reg signed [15:0]one4i[3:0];
reg signed [15:0]two4r[3:0];
reg signed [15:0]two4i[3:0];
reg signed [15:0]three4r[3:0];
reg signed [15:0]three4i[3:0];

dft4p dft4p1(
    .clkmain(clkmain),
	 .hfclk(hfclk),
	 .rst2(rst2),
	 .en(en),
	 .ana(ana),
	 .j(j),
	 .r_1_4(r_1_4),
	 .r_2_4(r_2_4),
	 .r_3_4(r_3_4),
	 .r_4_4(r_4_4),
	 .i_1_4(i_1_4),
	 .i_2_4(i_2_4),
	 .i_3_4(i_3_4),
	 .i_4_4(i_4_4)
);

reg signed [15:0]evenr[3:0];
reg signed [15:0]eveni[3:0];
reg signed [15:0]oddr[3:0];
reg signed [15:0]oddi[3:0];

assign wreal[0]=1;
assign wimag[0]=0;
assign wreal[1]=7;
assign wimag[1]=-7;
assign wreal[2]=0;
assign wimag[2]=-1;
assign wreal[3]=-7;
assign wimag[3]=-7;

assign r_1_8=(evenr[0]+(wreal[0]*oddr[0]-wimag[0]*oddi[0]))/2;
assign i_1_8=(eveni[0]+(wreal[0]*oddi[0]+wimag[0]*oddr[0]))/2;
assign r_2_8=(evenr[1]+(wreal[1]*oddr[1]-wimag[1]*oddi[1]))/2;
assign i_2_8=(eveni[1]+(wreal[1]*oddi[1]+wimag[1]*oddr[1]))/2;
assign r_3_8=(evenr[2]+(wreal[2]*oddr[2]-wimag[2]*oddi[2]))/2;
assign i_3_8=(eveni[2]+(wreal[2]*oddi[2]+wimag[2]*oddr[2]))/2;
assign r_4_8=(evenr[3]+(wreal[3]*oddr[3]-wimag[3]*oddi[3]))/2;
assign i_4_8=(eveni[3]+(wreal[3]*oddi[3]+wimag[3]*oddr[3]))/2;

assign r_5_8=(evenr[0]-(wreal[0]*oddr[0]-wimag[0]*oddi[0]))/2;
assign i_5_8=(eveni[0]-(wreal[0]*oddi[0]+wimag[0]*oddr[0]))/2;
assign r_6_8=(evenr[1]-(wreal[1]*oddr[1]-wimag[1]*oddi[1]))/2;
assign i_6_8=(eveni[1]-(wreal[1]*oddi[1]+wimag[1]*oddr[1]))/2;
assign r_7_8=(evenr[2]-(wreal[2]*oddr[2]-wimag[2]*oddi[2]))/2;
assign i_7_8=(eveni[2]-(wreal[2]*oddi[2]+wimag[2]*oddr[2]))/2;
assign r_8_8=(evenr[3]-(wreal[3]*oddr[3]-wimag[3]*oddi[3]))/2;
assign i_8_8=(eveni[3]-(wreal[3]*oddi[3]+wimag[3]*oddr[3]))/2;



always @(en)
begin
	    case(en[2])
		 0:begin
		   evenr[0]=zero4r[0];
			evenr[1]=zero4r[1];
			evenr[2]=zero4r[2];
			evenr[3]=zero4r[3];
			eveni[0]=zero4i[0];
			eveni[1]=zero4i[1];
			eveni[2]=zero4i[2];
			eveni[3]=zero4i[3];
		   oddr[0]=one4r[0];
			oddr[1]=one4r[1];
			oddr[2]=one4r[2];
			oddr[3]=one4r[3];
			oddi[0]=one4i[0];
			oddi[1]=one4i[1];
			oddi[2]=one4i[2];
			oddi[3]=one4i[3];
		   end
		 1:begin
		   evenr[0]=two4r[0];
			evenr[1]=two4r[1];
			evenr[2]=two4r[2];
			evenr[3]=two4r[3];
			eveni[0]=two4i[0];
			eveni[1]=two4i[1];
			eveni[2]=two4i[2];
			eveni[3]=two4i[3];
		   oddr[0]=three4r[0];
			oddr[1]=three4r[1];
			oddr[2]=three4r[2];
			oddr[3]=three4r[3];
			oddi[0]=three4i[0];
			oddi[1]=three4i[1];
			oddi[2]=three4i[2];
			oddi[3]=three4i[3];
			end
		 endcase
end


always @(negedge clkmain)
begin
   if ((hfclk==1'd1)&&(j==2'd2)) begin
	    case(en[2:1])
		 0:begin
		   zero4r[0]<=r_1_4;
			zero4r[1]<=r_2_4;
			zero4r[2]<=r_3_4;
			zero4r[3]<=r_4_4;
			zero4i[0]<=i_1_4;
			zero4i[1]<=i_2_4;
			zero4i[2]<=i_3_4;
			zero4i[3]<=i_4_4;
		   end
		 1:begin
		   one4r[0]<=r_1_4;
			one4r[1]<=r_2_4;
			one4r[2]<=r_3_4;
			one4r[3]<=r_4_4;
			one4i[0]<=i_1_4;
			one4i[1]<=i_2_4;
			one4i[2]<=i_3_4;
			one4i[3]<=i_4_4;
		   end
		 2:begin
		   two4r[0]<=r_1_4;
			two4r[1]<=r_2_4;
			two4r[2]<=r_3_4;
			two4r[3]<=r_4_4;
			two4i[0]<=i_1_4;
			two4i[1]<=i_2_4;
			two4i[2]<=i_3_4;
			two4i[3]<=i_4_4;
		   end
		 3:begin
		   three4r[0]<=r_1_4;
			three4r[1]<=r_2_4;
			three4r[2]<=r_3_4;
			three4r[3]<=r_4_4;
			three4i[0]<=i_1_4;
			three4i[1]<=i_2_4;
			three4i[2]<=i_3_4;
			three4i[3]<=i_4_4;
		   end
		 endcase
	end
end



endmodule



/* dft 16 pt*/


module dft16p(
    input clkmain,
	 input hfclk,
	 input rst2,
	 input [2:0]en,
	 input [3:0]ana,
	 input [1:0]j,
	 
	 output signed [15:0]r_1_16,r_2_16,r_3_16,r_4_16,r_5_16,r_6_16,r_7_16,r_8_16,r_9_16,r_10_16,r_11_16,r_12_16,r_13_16,r_14_16,r_15_16,r_16_16,
	 output signed [15:0]i_1_16,i_2_16,i_3_16,i_4_16,i_5_16,i_6_16,i_7_16,i_8_16,i_9_16,i_10_16,i_11_16,i_12_16,i_13_16,i_14_16,i_15_16,i_16_16
    );

wire signed [15:0]r_1_8,r_2_8,r_3_8,r_4_8,r_5_8,r_6_8,r_7_8,r_8_8;
wire signed [15:0]i_1_8,i_2_8,i_3_8,i_4_8,i_5_8,i_6_8,i_7_8,i_8_8;

wire signed [7:0]wreal[7:0];
wire signed [7:0]wimag[7:0];

reg signed [15:0]evenr[7:0];
reg signed [15:0]eveni[7:0];
reg signed [15:0]oddr[7:0];
reg signed [15:0]oddi[7:0];

dft8p dft8p1(
    .clkmain(clkmain),
	 .hfclk(hfclk),
	 .rst2(rst2),
	 .en(en),
	 .ana(ana),
	 .j(j),
	 
	 .r_1_8(r_1_8),
	 .r_2_8(r_2_8),
	 .r_3_8(r_3_8),
	 .r_4_8(r_4_8),
	 .r_5_8(r_5_8),
	 .r_6_8(r_6_8),
	 .r_7_8(r_7_8),
	 .r_8_8(r_8_8),
	 .i_1_8(i_1_8),
	 .i_2_8(i_2_8),
	 .i_3_8(i_3_8),
	 .i_4_8(i_4_8),
	 .i_5_8(i_5_8),
	 .i_6_8(i_6_8),
	 .i_7_8(i_7_8),
	 .i_8_8(i_8_8)
);

assign wreal[0]=1;
assign wimag[0]=0;
assign wreal[1]=9;
assign wimag[1]=-3;
assign wreal[2]=7;
assign wimag[2]=-7;
assign wreal[3]=3;
assign wimag[3]=-9;
assign wreal[4]=0;
assign wimag[4]=-1;
assign wreal[5]=-3;
assign wimag[5]=-9;
assign wreal[6]=-7;
assign wimag[6]=-7;
assign wreal[7]=-9;
assign wimag[7]=-3;

assign r_1_16=evenr[0]+(wreal[0]*oddr[0]-wimag[0]*oddi[0]);
assign i_1_16=eveni[0]+(wreal[0]*oddi[0]+wimag[0]*oddr[0]);
assign r_2_16=evenr[1]+(wreal[1]*oddr[1]-wimag[1]*oddi[1]);
assign i_2_16=eveni[1]+(wreal[1]*oddi[1]+wimag[1]*oddr[1]);
assign r_3_16=evenr[2]+(wreal[2]*oddr[2]-wimag[2]*oddi[2]);
assign i_3_16=eveni[2]+(wreal[2]*oddi[2]+wimag[2]*oddr[2]);
assign r_4_16=evenr[3]+(wreal[3]*oddr[3]-wimag[3]*oddi[3]);
assign i_4_16=eveni[3]+(wreal[3]*oddi[3]+wimag[3]*oddr[3]);
assign r_5_16=evenr[4]+(wreal[4]*oddr[4]-wimag[4]*oddi[4]);
assign i_5_16=eveni[4]+(wreal[4]*oddi[4]+wimag[4]*oddr[4]);
assign r_6_16=evenr[5]+(wreal[5]*oddr[5]-wimag[5]*oddi[5]);
assign i_6_16=eveni[5]+(wreal[5]*oddi[5]+wimag[5]*oddr[5]);
assign r_7_16=evenr[6]+(wreal[6]*oddr[6]-wimag[6]*oddi[6]);
assign i_7_16=eveni[6]+(wreal[6]*oddi[6]+wimag[6]*oddr[6]);
assign r_8_16=evenr[7]+(wreal[7]*oddr[7]-wimag[7]*oddi[7]);
assign i_8_16=eveni[7]+(wreal[7]*oddi[7]+wimag[7]*oddr[7]);

assign r_9_16=evenr[0]-(wreal[0]*oddr[0]-wimag[0]*oddi[0]);
assign i_9_16=eveni[0]-(wreal[0]*oddi[0]+wimag[0]*oddr[0]);
assign r_10_16=evenr[1]-(wreal[1]*oddr[1]-wimag[1]*oddi[1]);
assign i_10_16=eveni[1]-(wreal[1]*oddi[1]+wimag[1]*oddr[1]);
assign r_11_16=evenr[2]-(wreal[2]*oddr[2]-wimag[2]*oddi[2]);
assign i_11_16=eveni[2]-(wreal[2]*oddi[2]+wimag[2]*oddr[2]);
assign r_12_16=evenr[3]-(wreal[3]*oddr[3]-wimag[3]*oddi[3]);
assign i_12_16=eveni[3]-(wreal[3]*oddi[3]+wimag[3]*oddr[3]);
assign r_13_16=evenr[4]-(wreal[4]*oddr[4]-wimag[4]*oddi[4]);
assign i_13_16=eveni[4]-(wreal[4]*oddi[4]+wimag[4]*oddr[4]);
assign r_14_16=evenr[5]-(wreal[5]*oddr[5]-wimag[5]*oddi[5]);
assign i_14_16=eveni[5]-(wreal[5]*oddi[5]+wimag[5]*oddr[5]);
assign r_15_16=evenr[6]-(wreal[6]*oddr[6]-wimag[6]*oddi[6]);
assign i_15_16=eveni[6]-(wreal[6]*oddi[6]+wimag[6]*oddr[6]);
assign r_16_16=evenr[7]-(wreal[7]*oddr[7]-wimag[7]*oddi[7]);
assign i_16_16=eveni[7]-(wreal[7]*oddi[7]+wimag[7]*oddr[7]);

always @(negedge clkmain)
begin
   if ((hfclk==1'd1)&&(j==2'd0)) begin
	    case(en[2])
		 0:begin
		   evenr[0]<=r_1_8;
			evenr[1]<=r_2_8;
			evenr[2]<=r_3_8;
			evenr[3]<=r_4_8;
			evenr[4]<=r_5_8;
			evenr[5]<=r_6_8;
			evenr[6]<=r_7_8;
			evenr[7]<=r_8_8;
			eveni[0]<=i_1_8;
			eveni[1]<=i_2_8;
			eveni[2]<=i_3_8;
			eveni[3]<=i_4_8;
			eveni[4]<=i_5_8;
			eveni[5]<=i_6_8;
			eveni[6]<=i_7_8;
			eveni[7]<=i_8_8;
		   end
		 1:begin
		   oddr[0]<=r_1_8;
			oddr[1]<=r_2_8;
			oddr[2]<=r_3_8;
			oddr[3]<=r_4_8;
			oddr[4]<=r_5_8;
			oddr[5]<=r_6_8;
			oddr[6]<=r_7_8;
			oddr[7]<=r_8_8;
			oddi[0]<=i_1_8;
			oddi[1]<=i_2_8;
			oddi[2]<=i_3_8;
			oddi[3]<=i_4_8;
			oddi[4]<=i_5_8;
			oddi[5]<=i_6_8;
			oddi[6]<=i_7_8;
			oddi[7]<=i_8_8;
		   end
		 endcase
	end
end

endmodule


