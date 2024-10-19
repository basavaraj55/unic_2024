module blowfish(s,key,fa_pt);
  input key;
  input [3:0]s;
  output fa_pt;
  wire [63:0]sk,pt2,ct,pt;
  wire [31:0]ky1,ky2,ky3,ky4,ky5,ky6,ky7,ky8,ky9,ky10,ky11,ky12,ky13,ky14,ky15,ky16,ky17,ky18,
             a1,a2,p1,p2,p3,p4,p5,p6,p7,p8,p9,p10,p11,p12,p13,p14,p15,p16,p17,p18;
  
 bf_demux b1(s,pt);
 bf_keygen b2(key,sk,p1,p2,p3,p4,p5,p6,p7,p8,p9,p10,p11,p12,p13,p14,p15,p16,p17,p18);
 bf_bitsplit64 b3(sk,a2,a1);
 xor32bits xb1(p1,a1,ky1),xb2(p2,a2,ky2), xb3(p3,a1,ky3), xb4(p4,a2,ky4), 
           xb5(p5,a1,ky5), xb6(p6,a2,ky6), xb7(p7,a1,ky7), xb8(p8,a2,ky8), xb9(p9,a1,ky9), 
           xb10(p10,a2,ky10),xb11(p11,a1,ky11), xb12(p12,a2,ky12), xb13(p13,a1,ky13), xb14(p14,a2,ky14), 
           xb15(p15,a1,ky15), xb16(p16,a2,ky16), xb17(p17,a1,ky17), xb18(p18,a2,ky18);
bf_encryption e1(pt,ky1,ky2,ky3,ky4,ky5,ky6,ky7,ky8,ky9,ky10,ky11,ky12,ky13,ky14,ky15,ky16,ky17,ky18,ct);
bf_decryption e2(ct,ky1,ky2,ky3,ky4,ky5,ky6,ky7,ky8,ky9,ky10,ky11,ky12,ky13,ky14,ky15,ky16,ky17,ky18,pt2);
bf_fault f1(pt,pt2,fa_pt);
endmodule


module bf_demux(s,y);
  input [3:0]s;
  output reg [63:0]y;
  always@(s)
  begin
    if (s == 4'b0000) begin y = 64'b0001001000110100010101101010101111001101000100110010010100110110; end
    else if (s == 4'b0001) begin y = 64'b0100010000100001101010111111101000110111010001011101111011001010; end
    else if (s == 4'b0010) begin y = 64'b0101010000110010100010011100110110111010111111110110011100110010; end
    else if (s == 4'b0011) begin y = 64'b1010101100010001101111000010001000110100110111010101011010101111; end
    else if (s == 4'b0100) begin y = 64'b0001001000110100001100111101110101000100111111111001100001010001; end 
    else if (s == 4'b0101) begin y = 64'b1110111111101111111110101111101010111100101111001101101011011011; end
    else if (s == 4'b0110) begin y = 64'b1011101000010100111110100110010100100011010000010110100001010111; end
    else if (s == 4'b0111) begin y = 64'b1101110001111000101110100110010100010010111011110011010001000011; end 
    else if (s == 4'b1000) begin y = 64'b1001100011001101010000100001001111001010101111011111111010101011; end 
    else if (s == 4'b1001) begin y = 64'b0001010110001001101011001111111000110111010000101101101101100100; end
    else if (s == 4'b1010) begin y = 64'b0100010000100001101010111111101000110111010001011101111011001010; end
    else if (s == 4'b1011) begin y = 64'b0110011110001001111111101101110001000011001000011010101101010100; end
    else if (s == 4'b1100) begin y = 64'b0001000100100010001100110100010010101010101110111110111011011101; end 
    else if (s == 4'b1101) begin y = 64'b0001000100010001001000100010001011001100110011001010101111001101; end
    else if (s == 4'b1110) begin y = 64'b0001001000100011101010111011110001000101010101101100110111001101; end
    else if (s == 4'b1111) begin y = 64'b1111111101000100000100100011010011011101001100110100001100100001; end
  end
  endmodule

module bf_keygen(s,y,p1,p2,p3,p4,p5,p6,p7,p8,p9,p10,p11,p12,p13,p14,p15,p16,p17,p18);
  input s;
  output reg [63:0]y;
  output reg [31:0]p1,p2,p3,p4,p5,p6,p7,p8,p9,p10,p11,p12,p13,p14,p15,p16,p17,p18;
  always@(s)
  begin
    if (s == 1'b1) 
    begin 
    y = 64'b1010101010111011000010010001100000100111001101101100110011011101; 
    p1=32'b00100100001111110110101010001000;
    p2=32'b10000101101000110000100011010011;
    p3=32'b00010011000110011000101000101110;
    p4=32'b00000011011100000111001101000100;
    p5=32'b10100100000010010011100000100010;
    p6=32'b00101001100111110011000111010000;
    p7=32'b00001000001011101111101010011000;
    p8=32'b11101100010011100110110010001001;
    p9=32'b01000101001010000010000111100110;
    p10=32'b00111000110100000001001101110111;
    p11=32'b10111110010101000110011011001111;
    p12=32'b00110100111010010000110001101100;
    p13=32'b11000000101011000010100110110111;
    p14=32'b11001001011111000101000011011101;
    p15=32'b00111111100001001101010110110101;
    p16=32'b10110101010001110000100100010111;
    p17=32'b10010010000101101101010111011001;
    p18=32'b10001001011110011111101100011011;
    end
  end
endmodule


module bf_bitsplit64(plain,d1,d2);
input [63:0]plain;
output [31:0]d1,d2;
begin
assign d1 = plain[31:0];     
assign d2 = plain[63:32];
end 
endmodule

module bf_xor_1 (a,b,y);
  input [31:0]a,b;
  output [31:0]y;
  begin
    assign y = a ^ b;
  end
endmodule

module bf_encryption(pt,k1,k2,k3,k4,k5,k6,k7,k8,k9,k10,k11,k12,k13,k14,k15,k16,k17,k18,ct);
  input [63:0]pt;
  input [31:0]k1,k2,k3,k4,k5,k6,k7,k8,k9,k10,k11,k12,k13,k14,k15,k16,k17,k18;
  wire [31:0]a1,a2,a3,a4,a5,a6,a7,a8,a9,a10,
             a11,a12,a13,a14,a15,a16,a17,a18,a19,a20,
             a21,a22,a23,a24,a25,a26,a27,a28,a29,a30,
             a31,a32,a33,a34,a35,a36,a37,a38,a39,a40,
             a41,a42,a43,a44,a45,a46,a47,a48,a49,a50,a51,a52;
  output [63:0] ct;
  

bf_bitsplit64 be1(pt,a1,a2);

bf_xor_1 be2(a2,k1,a3); 
bf_function_r1 be3(a3,a4);
bf_xor_1 be4(a1,a4,a5);

bf_xor_1 be5(a5,k2,a6); 
bf_function_r1 be6(a6,a7);
bf_xor_1 be7(a3,a7,a8);

bf_xor_1 be8(a8,k3,a9); 
bf_function_r1 be9(a9,a10);
bf_xor_1 be10(a6,a10,a11);

bf_xor_1 be11(a11,k4,a12); 
bf_function_r1 be12(a12,a13);
bf_xor_1 be13(a9,a13,a14);

bf_xor_1 be14(a14,k5,a15); 
bf_function_r1 be15(a15,a16);
bf_xor_1 be16(a12,a16,a17);

bf_xor_1 be17(a17,k6,a18); 
bf_function_r1 be18(a18,a19);
bf_xor_1 be19(a15,a19,a20);

bf_xor_1 be20(a20,k7,a21); 
bf_function_r1 be21(a21,a22);
bf_xor_1 be22(a18,a22,a23);

bf_xor_1 be23(a23,k8,a24); 
bf_function_r1 be24(a24,a25);
bf_xor_1 be25(a21,a25,a26);

bf_xor_1 be26(a26,k9,a27); 
bf_function_r1 be27(a27,a28);
bf_xor_1 be28(a24,a28,a29);

bf_xor_1 be29(a29,k10,a30); 
bf_function_r1 be30(a30,a31);
bf_xor_1 be31(a27,a31,a32);

bf_xor_1 be32(a32,k11,a33); 
bf_function_r1 be33(a33,a34);
bf_xor_1 be34(a30,a34,a35);

bf_xor_1 be35(a35,k12,a36); 
bf_function_r1 be36(a36,a37);
bf_xor_1 be37(a33,a37,a38);

bf_xor_1 be38(a38,k13,a39); 
bf_function_r1 be39(a39,a40);
bf_xor_1 be40(a36,a40,a41);

bf_xor_1 be41(a41,k14,a42); 
bf_function_r1 be42(a42,a43);
bf_xor_1 be43(a39,a43,a44);

bf_xor_1 be44(a44,k15,a45); 
bf_function_r1 be45(a45,a46);
bf_xor_1 be46(a42,a46,a47);

bf_xor_1 be47(a47,k16,a48); 
bf_function_r1 be48(a48,a49);
bf_xor_1 be49(a45,a49,a50);

bf_xor_1 be50(a50,k17,a51);
bf_xor_1 be51(a48,k18,a52);

cipher65 be52(a51,a52,ct);

endmodule


module bf_bitsplit64(plain,d1,d2);
input [63:0]plain;
output [31:0]d1,d2;
begin
assign d1 = plain[31:0];     
assign d2 = plain[63:32];
end 
endmodule


module bf_function_r1(fbs,fout);
  input [31:0]fbs;
  output [31:0] fout;
  wire  [31:0]s1,s2,s3,s4;
  wire [7:0]t1,t2,t3,t4;
  wire [31:0]j5,j6;
  bf_bitsplit_sbox b1(fbs,t4,t3,t2,t1); 
   bf_loopup_s1box b2(t1,s1);
   bf_loopup_s2box_updated b3(t2,s2);
   bf_loopup_s3box b4(t3,s3);
   bf_loopup_s4box b5(t4,s4);
   bf_add_32 a1(s1,s2,j5);
   bf_xor_1 x1(j5,s3,j6);
   bf_add_32 a2(j6,s4,fout);
endmodule

module bf_bitsplit_sbox(subkey,s1,s2,s3,s4);
  input [31:0]subkey;
  output [7:0]s1,s2,s3,s4;
  begin
    assign s1=subkey[7:0];
    assign s2=subkey[15:8];
    assign s3=subkey[23:16];
    assign s4=subkey[31:24];
  end
endmodule

module bf_loopup_s1box(x,y);
input [7:0]x;
output reg [31:0]y;
always @(x)
begin
if (x==8'b00000000)
begin
  y=32'hd1310ba6;
end
else if(x==8'b00000001)
begin
  y=32'h98dfb5ac;
end
else if(x==8'b00000010)
  begin
    y=32'h2ffd72db;
    end
else if(x==8'b00000011)
  begin
    y=32'hd01adfb7;
    end
    else if(x==8'b00000100)
  begin
    y=32'hb8e1afed;
    end
    else if(x==8'b00000101)
  begin
    y=32'h6a267e96;
    end
    else if(x==8'b00000110)
  begin
    y=32'hba7c9045;
    end
    else if(x==8'b00000111)
  begin
    y=32'hf12c7f99;
    end
    else if(x==8'b00001000)
  begin
    y=32'h24a19947;
    end
    else if(x==8'b00001001)
  begin
    y=32'hb3916cf7;
    end
    else if(x==8'b00001010)
  begin
    y=32'h0801f2e2;
    end
    else if(x==8'b00001011)
  begin
    y=32'h858efc16;
    end
    else if(x==8'b00001100)
  begin
    y=32'h636920d8;
    end
    else if(x==8'b00001101)
  begin
    y=32'h71574e69;
    end
    else if(x==8'b00001110)
  begin
    y=32'ha458fea3;
    end
    else if(x==8'b00001111)
  begin
    y=32'hf4933d7e;
    end
 else if (x==8'b00010000)
begin
  y=32'h0d95748f;
end
else if(x==8'b00010001)
begin
  y=32'h728eb658;
end
else if(x==8'b00010010)
  begin
    y=32'h718bcd58;
    end
else if(x==8'b00010011)
  begin
    y=32'h82154aee;
    end
    else if(x==8'b00010100)
  begin
    y=32'h7b54a41d;
    end
    else if(x==8'b00010101)
  begin
    y=32'hc25a59b5;
    end
    else if(x==8'b00010110)
  begin
    y=32'h9c30d539;
    end
    else if(x==8'b00010111)
  begin
    y=32'h2af26013;
    end
    else if(x==8'b00011000)
begin
  y=32'hc5d1b023;
end
else if(x==8'b00011001)
begin
  y=32'h286085f0;
end
else if(x==8'b00011010)
begin
  y=32'hca417918;
end
else if(x==8'b00011011)
begin
  y=32'hb8db38ef;
end
else if(x==8'b00011100)
begin
  y=32'h8e79dcb0;
end
else if(x==8'b00011101)
begin
  y=32'h603a180e;
end
else if(x==8'b00011110)
begin
  y=32'h6c9e0e8b;
end
else if(x==8'b00011111)
begin
    y=32'hb01e8a3e;
  end
  else if (x==8'b00100000)
begin
  y=32'hd71577c1;
end
else if(x==8'b00100001)
begin
  y=32'hbd314b27;
end
else if(x==8'b00100010)
  begin
    y=32'h78af2fda;
    end
else if(x==8'b00100011)
  begin
    y=32'h55605c60;
    end
    else if(x==8'b00100100)
  begin
    y=32'he65525f3;
    end
    else if(x==8'b00100101)
  begin
    y=32'haa55ab94;
    end
    else if(x==8'b00100110)
  begin
    y=32'h57489862;
    end
    else if(x==8'b00100111)
  begin
    y=32'h63e81440;
    end
    else if(x==8'b00101000)
begin
  y=32'h55ca396a;
end
else if(x==8'b00101001)
begin
  y=32'h2aab10b6;
end
else if(x==8'b00101010)
begin
  y=32'hb4cc5c34;
end
else if(x==8'b00101011)
begin
  y=32'h1141e8ce;
end
else if(x==8'b00101100)
begin
  y=32'ha15486af;
end
else if(x==8'b00101101)
begin
  y=32'h7c72e993;
end
else if(x==8'b00101110)
begin
  y=32'hb3ee1411;
end
else if(x==8'b00101111)
begin
    y=32'h636fbc2a;
  end
  else if (x==8'b00110000)
begin
  y=32'h2ba9c55d;
end
else if(x==8'b00110001)
begin
  y=32'h741831f6;
end
else if(x==8'b00110010)
  begin
    y=32'hce5c3e16;
    end
else if(x==8'b00110011)
  begin
    y=32'h9b87931e;
    end
    else if(x==8'b00110100)
  begin
    y=32'hafd6da33;
    end
    else if(x==8'b00110101)
  begin
    y=32'h6c24cf5c;
    end
    else if(x==8'b00110110)
  begin
    y=32'h7a325381;
    end
    else if(x==8'b00110111)
  begin
    y=32'h28958677;
    end
    else if(x==8'b00111000)
begin
  y=32'h3b8f4898;
end
else if(x==8'b00111001)
begin
  y=32'h6b4bb9af;
end
else if(x==8'b00111010)
begin
  y=32'hc4bfe81b;
end
else if(x==8'b00111011)
begin
  y=32'h66282193;
end
else if(x==8'b00111100)
begin
  y=32'h61d809cc;
end
else if(x==8'b00111101)
begin
  y=32'hfb21a991;
end
else if(x==8'b00111110)
begin
  y=32'h487cac60;
end
else if(x==8'b00111111)
begin
    y=32'h5dec8032;
  end
else if (x==8'b01000000)
begin
  y=32'hef845d5d;
end
else if(x==8'b01000001)
begin
  y=32'he98575b1;
end
else if(x==8'b01000010)
  begin
    y=32'hdc262302;
    end
else if(x==8'b01000011)
  begin
    y=32'heb651b88;
    end
    else if(x==8'b01000100)
  begin
    y=32'h23893e81;
    end
    else if(x==8'b01000101)
  begin
    y=32'hd396acc5;
    end
    else if(x==8'b01000110)
  begin
    y=32'h0f6d6ff3;
    end
    else if(x==8'b01000111)
  begin
    y=32'h83f44239;
    end
    else if(x==8'b01001000)
  begin
    y=32'h2e0b4482;
    end
    else if(x==8'b01001001)
  begin
    y=32'ha4842004;
    end
    else if(x==8'b01001010)
  begin
    y=32'h69c8f04a;
    end
    else if(x==8'b01001011)
  begin
    y=32'h9e1f9b5e;
    end
    else if(x==8'b01001100)
  begin
    y=32'h21c66842;
    end
    else if(x==8'b01001101)
  begin
    y=32'hf6e96c9a;
    end
    else if(x==8'b01001110)
  begin
    y=32'h670c9c61;
    end
    else if(x==8'b01001111)
  begin
    y=32'habd388f0;
    end
    else if (x==8'b01010000)
begin
  y=32'h6a51a0d2;
end
else if(x==8'b01010001)
begin
  y=32'hd8542f68;
end
else if(x==8'b01010010)
  begin
    y=32'h960fa728;
    end
else if(x==8'b01010011)
  begin
    y=32'hab5133a3;
    end
    else if(x==8'b01010100)
  begin
    y=32'h6eef0b6c;
    end
    else if(x==8'b01010101)
  begin
    y=32'h137a3be4;
    end
    else if(x==8'b01010110)
  begin
    y=32'hba3bf050;
    end
    else if(x==8'b01010111)
  begin
    y=32'h7efb2a98;
    end
    else if(x==8'b01011000)
  begin
    y=32'ha1f1651d;
    end
    else if(x==8'b01011001)
  begin
    y=32'h39af0176;
    end
    else if(x==8'b01011010)
  begin
    y=32'h66ca593e;
    end
    else if(x==8'b01011011)
  begin
    y=32'h82430e88;
    end
    else if(x==8'b01011100)
  begin
    y=32'h8cee8619;
    end
    else if(x==8'b01011101)
  begin
    y=32'h456f9fb4;
    end
    else if(x==8'b01011110)
  begin
    y=32'h7d84a5c3;
    end
    else if(x==8'b01011111)
  begin
    y=32'h3b8b5ebe;
    end
    else if (x==8'b01100000)
begin
  y=32'he06f75d8;
end
else if(x==8'b01100001)
begin
  y=32'h85c12073;
end
else if(x==8'b01100010)
  begin
    y=32'h401a449f;
    end
else if(x==8'b01100011)
  begin
    y=32'h56c16aa6;
    end
    else if(x==8'b01100100)
  begin
    y=32'h4ed3aa62;
    end
    else if(x==8'b01100101)
  begin
    y=32'h363f7706;
    end
    else if(x==8'b01100110)
  begin
    y=32'h1bfedf72;
    end
    else if(x==8'b01100111)
  begin
    y=32'h429b023d;
    end
    else if(x==8'b01101000)
  begin
    y=32'h37d0d724;
    end
    else if(x==8'b01101001)
  begin
    y=32'hd00a1248;
    end
    else if(x==8'b01101010)
  begin
    y=32'hdb0fead3;
    end
    else if(x==8'b01101011)
  begin
    y=32'h4941c09b;
    end
    else if(x==8'b01101100)
  begin
    y=32'h075372c9;
    end
    else if(x==8'b01101101)
  begin
    y=32'h80991b7b;
    end
    else if(x==8'b01101110)
  begin
    y=32'h25d479d8;
    end
    else if(x==8'b01101111)
  begin
    y=32'hf6e8def7;
    end
   else if (x==8'b01110000)
begin
  y=32'he3fe501a;
end
else if(x==8'b01110001)
begin
  y=32'hb6794c3b;
end
else if(x==8'b01110010)
  begin
    y=32'h976ce0bd;
    end
else if(x==8'b01110011)
  begin
    y=32'h04c006ba;
    end
    else if(x==8'b01110100)
  begin
    y=32'hc1a94fb6;
    end
    else if(x==8'b01110101)
  begin
    y=32'h409f60c4;
    end
    else if(x==8'b01110110)
  begin
    y=32'h5e5c9ec2;
    end
    else if(x==8'b01110111)
  begin
    y=32'h196a2463;
    end
    else if(x==8'b01111000)
  begin
    y=32'h68fb6faf;
    end
    else if(x==8'b01111001)
  begin
    y=32'h3e6c53b5;
    end
    else if(x==8'b01111010)
  begin
    y=32'h1339b2eb;
    end
    else if(x==8'b01111011)
  begin
    y=32'h3b52ec6f;
    end
    else if(x==8'b01111100)
  begin
    y=32'h6dfc511f;
    end
    else if(x==8'b01111101)
  begin
    y=32'h9b30952c;
    end
    else if(x==8'b01111110)
  begin
    y=32'hcc814544;
    end
    else if(x==8'b01111111)
  begin
    y=32'haf5ebd09;
    end 
else if (x==8'b10000000)
begin
  y=32'hbee3d004;
end
else if(x==8'b10000001)
begin
  y=32'hde334afd;
end
else if(x==8'b10000010)
  begin
    y=32'h660f2807;
    end
else if(x==8'b10000011)
  begin
    y=32'h192e4bb3;
    end
    else if(x==8'b10000100)
  begin
    y=32'hc0cba857;
    end
    else if(x==8'b10000101)
  begin
    y=32'h45c8740f;
    end
    else if(x==8'b10000110)
  begin
    y=32'hd20b5f39;
    end
    else if(x==8'b10000111)
  begin
    y=32'hb9d3fbdb;
    end
    else if(x==8'b10001000)
  begin
    y=32'h5579c0bd;
    end
    else if(x==8'b10001001)
  begin
    y=32'h1a60320a;
    end
    else if(x==8'b10001010)
  begin
    y=32'hd6a100c6;
    end
    else if(x==8'b10001011)
  begin
    y=32'h402c7279;
    end
    else if(x==8'b10001100)
  begin
    y=32'h679f25fe;
    end
    else if(x==8'b10001101)
  begin
    y=32'hfb1fa3cc;
    end
    else if(x==8'b10001110)
  begin
    y=32'h8ea5e9f8;
    end
    else if(x==8'b10001111)
  begin
    y=32'hdb3222f8;
    end
    else if (x==8'b10010000)
begin
  y=32'h3c7516df;
end
else if(x==8'b10010001)
begin
  y=32'hfd616b15;
end
else if(x==8'b10010010)
  begin
    y=32'h2f501ec8;
    end
else if(x==8'b10010011)
  begin
    y=32'had0552ab;
    end
    else if(x==8'b10010100)
  begin
    y=32'h323db5fa;
    end
    else if(x==8'b10010101)
  begin
    y=32'hfd238760;
    end
    else if(x==8'b10010110)
  begin
    y=32'h53317b48;
    end
    else if(x==8'b10010111)
  begin
    y=32'h3e00df82;
    end
    else if(x==8'b10011000)
  begin
    y=32'h9e5c57bb;
    end
    else if(x==8'b10011001)
  begin
    y=32'hca6f8ca0;
    end
    else if(x==8'b10011010)
  begin
    y=32'h1a87562e;
    end
    else if(x==8'b10011011)
  begin
    y=32'hdf1769db;
    end
    else if(x==8'b10011100)
  begin
    y=32'hd542a8f6;
    end
    else if(x==8'b10011101)
  begin
    y=32'h287effc3;
    end
    else if(x==8'b10011110)
  begin
    y=32'hac6732c6;
    end
    else if(x==8'b10011111)
  begin
    y=32'h8c4f5573;
    end
    else if (x==8'b10100000)
begin
  y=32'h695b27b0;
end
else if(x==8'b10100001)
begin
  y=32'hbbca58c8;
end
else if(x==8'b10100010)
  begin
    y=32'he1ffa35d;
    end
else if(x==8'b10100011)
  begin
    y=32'hb8f011a0;
    end
    else if(x==8'b10100100)
  begin
    y=32'h10fa3d98;
    end
    else if(x==8'b10100101)
  begin
    y=32'hfd2183b8;
    end
    else if(x==8'b10100110)
  begin
    y=32'h4afcb56c;
    end
    else if(x==8'b10100111)
  begin
    y=32'h2dd1d35b;
    end
    else if(x==8'b10101000)
  begin
    y=32'h9a53e479;
    end
    else if(x==8'b10101001)
  begin
    y=32'hb6f84565;
    end
    else if(x==8'b10101010)
  begin
    y=32'hd28e49bc;
    end
    else if(x==8'b10101011)
  begin
    y=32'h4bfb9790;
    end
    else if(x==8'b10101100)
  begin
    y=32'he1ddf2da;
    end
    else if(x==8'b10101101)
  begin
    y=32'ha4cb7e33;
    end
    else if(x==8'b10101110)
  begin
    y=32'h62fb1341;
    end
    else if(x==8'b10101111)
  begin
    y=32'hcee4c6e8;
    end
   else if (x==8'b10110000)
begin
  y=32'hef20cada;
end
else if(x==8'b10110001)
begin
  y=32'h36774c01;
end
else if(x==8'b10110010)
  begin
    y=32'hd07e9efe;
    end
else if(x==8'b10110011)
  begin
    y=32'h2bf11fb4;
    end
    else if(x==8'b10110100)
  begin
    y=32'h95dbda4d;
    end
    else if(x==8'b10110101)
  begin
    y=32'hae909198;
    end
    else if(x==8'b10110110)
  begin
    y=32'heaad8e71;
    end
    else if(x==8'b10110111)
  begin
    y=32'h6b93d5a0;
    end
    else if(x==8'b10111000)
  begin
    y=32'hd08ed1d0;
    end
    else if(x==8'b10111001)
  begin
    y=32'hafc725e0;
    end
    else if(x==8'b10111010)
  begin
    y=32'h8e3c5b2f;
    end
    else if(x==8'b10111011)
  begin
    y=32'h8e7594b7;
    end
    else if(x==8'b10111100)
  begin
    y=32'h8ff6e2fb;
    end
    else if(x==8'b10111101)
  begin
    y=32'hf2122b64;
    end
    else if(x==8'b10111110)
  begin
    y=32'h8888b812;
    end
    else if(x==8'b10111111)
  begin
    y=32'h900df01c;
    end
    else if (x==8'b11000000)
begin
  y=32'h4fad5ea0;
end
else if(x==8'b11000001)
begin
  y=32'h688fc31c;
end
else if(x==8'b11000010)
  begin
    y=32'hd1cff191;
    end
else if(x==8'b11000011)
  begin
    y=32'hb3a8c1ad;
    end
    else if(x==8'b11000100)
  begin
    y=32'h2f2f2218;
    end
    else if(x==8'b11000101)
  begin
    y=32'hbe0e1777;
    end
    else if(x==8'b11000110)
  begin
    y=32'hea752dfe;
    end
    else if(x==8'b11000111)
  begin
    y=32'h8b021fa1;
    end
    else if(x==8'b11001000)
  begin
    y=32'he5a0cc0f;
    end
    else if(x==8'b11001001)
  begin
    y=32'hb56f74e8;
    end
    else if(x==8'b11001010)
  begin
    y=32'h18acf3d6;
    end
    else if(x==8'b11001011)
  begin
    y=32'hce89e299;
    end
    else if(x==8'b11001100)
  begin
    y=32'hb4a84fe0;
    end
    else if(x==8'b11001101)
  begin
    y=32'hfd13e0b7;
    end
    else if(x==8'b11001110)
  begin
    y=32'h7cc43b81;
    end
    else if(x==8'b11001111)
  begin
    y=32'hd2ada8d9;
    end
   else if (x==8'b11010000)
begin
  y=32'h165fa266;
end
else if(x==8'b11010001)
begin
  y=32'h80957705;
end
else if(x==8'b11010010)
  begin
    y=32'h93cc7314;
    end
else if(x==8'b11010011)
  begin
    y=32'h211a1477;
    end
    else if(x==8'b11010100)
  begin
    y=32'he6ad2065;
    end
    else if(x==8'b11010101)
  begin
    y=32'h77b5fa86;
    end
    else if(x==8'b11010110)
  begin
    y=32'hc75442f5;
    end
    else if(x==8'b11010111)
  begin
    y=32'hfb9d35cf;
    end
    else if(x==8'b11011000)
  begin
    y=32'hebcdaf0c;
    end
    else if(x==8'b11011001)
  begin
    y=32'h7b3e89a0;
    end
    else if(x==8'b11011010)
  begin
    y=32'hd6411bd3;
    end
    else if(x==8'b11011011)
  begin
    y=32'hae1e7e49;
    end
    else if(x==8'b11011100)
  begin
    y=32'h00250e2d;
    end
    else if(x==8'b11011101)
  begin
    y=32'h2071b35e;
    end
    else if(x==8'b11011110)
  begin
    y=32'h226800bb;
    end
    else if(x==8'b11011111)
  begin
    y=32'h57b8e0af;
    end
    else if (x==8'b11100000)
begin
  y=32'h2464369b;
end
else if(x==8'b11100001)
begin
  y=32'hf009b91e;
end
else if(x==8'b11100010)
  begin
    y=32'h5563911d;
    end
else if(x==8'b11100011)
  begin
    y=32'h59dfa6aa;
    end
    else if(x==8'b11100100)
  begin
    y=32'h78c14389;
    end
    else if(x==8'b11100101)
  begin
    y=32'hd95a537f;
    end
    else if(x==8'b11100110)
  begin
    y=32'h207d5ba2;
    end
    else if(x==8'b11100111)
  begin
    y=32'h02e5b9c5;
    end
    else if(x==8'b11101000)
  begin
    y=32'h83260376;
    end
    else if(x==8'b11101001)
  begin
    y=32'h6295cfa9;
    end
    else if(x==8'b11101010)
  begin
    y=32'h11c81968;
    end
    else if(x==8'b11101011)
  begin
    y=32'h4e734a41;
    end
    else if(x==8'b11101100)
  begin
    y=32'hb3472dca;
    end
    else if(x==8'b11101101)
  begin
    y=32'h7b14a94a;
    end
    else if(x==8'b11101110)
  begin
    y=32'h1b510052;
    end
    else if(x==8'b11101111)
  begin
    y=32'h9a532915;
    end
    else if (x==8'b11110000)
begin
  y=32'hd60f573f;
end
else if(x==8'b11110001)
begin
  y=32'hbc9bc6e4;
end
else if(x==8'b11110010)
  begin
    y=32'h2b60a476;
    end
else if(x==8'b11110011)
  begin
    y=32'h81e67400;
    end
    else if(x==8'b11110100)
  begin
    y=32'h08ba6fb5;
    end
    else if(x==8'b11110101)
  begin
    y=32'h571be91f;
    end
    else if(x==8'b11110110)
  begin
    y=32'hf296ec6b;
    end
    else if(x==8'b11110111)
  begin
    y=32'h2a0dd915;
    end
    else if(x==8'b11111000)
  begin
    y=32'hb6636521;
    end
    else if(x==8'b11111001)
  begin
    y=32'he7b9f9b6;
    end
    else if(x==8'b11111010)
  begin
    y=32'hff34052e;
    end
    else if(x==8'b11111011)
  begin
    y=32'hc5855664;
    end
    else if(x==8'b11111100)
  begin
    y=32'h53b02d5d;
    end
    else if(x==8'b11111101)
  begin
    y=32'ha99f8fa1;
    end
    else if(x==8'b11111110)
  begin
    y=32'h08ba4799;
    end
    else if(x==8'b11111111)
  begin
    y=32'h6e85076a;
    end
     end
endmodule

module bf_loopup_s2box_updated(x,y);
input [7:0]x;
output reg [31:0]y;
always @(x)
begin
if (x==8'b00000000)
begin
  y=32'h4b7a70e9;
end
else if(x==8'b00000001)
begin
  y=32'hb5b32944;
end
else if(x==8'b00000010)
  begin
    y=32'hdb75092e;
    end
else if(x==8'b00000011)
  begin
    y=32'hc4192623;
    end
    else if(x==8'b00000100)
  begin
    y=32'had6ea6b0;
    end
    else if(x==8'b00000101)
  begin
    y=32'h49a7df7d;
    end
    else if(x==8'b00000110)
  begin
    y=32'h9cee60b8;
    end
    else if(x==8'b00000111)
  begin
    y=32'h8fedb266;
    end
    else if(x==8'b00001000)
  begin
    y=32'hecaa8c71;
    end
    else if(x==8'b00001001)
  begin
    y=32'h699a17ff;
    end
    else if(x==8'b00001010)
  begin
    y=32'h5664526c;
    end
    else if(x==8'b00001011)
  begin
    y=32'hc2b19ee1;
    end
    else if(x==8'b00001100)
  begin
    y=32'h193602a5;
    end
    else if(x==8'b00001101)
  begin
    y=32'h75094c29;
    end
    else if(x==8'b00001110)
  begin
    y=32'ha0591340;
    end
    else if(x==8'b00001111)
  begin
    y=32'he4183a3e;
    end
 else if (x==8'b00010000)
begin
  y=32'h3f54989a;
end
else if(x==8'b00010001)
begin
  y=32'h5b429d65;
end
else if(x==8'b00010010)
  begin
    y=32'h6b8fe4d6;
    end
else if(x==8'b00010011)
  begin
    y=32'h99f73fd6;
    end
    else if(x==8'b00010100)
  begin
    y=32'ha1d29c07;
    end
    else if(x==8'b00010101)
  begin
    y=32'hefe830f5;
    end
    else if(x==8'b00010110)
  begin
    y=32'h4d2d38e6;
    end
    else if(x==8'b00010111)
  begin
    y=32'hf0255dc1;
    end
    else if(x==8'b00011000)
begin
  y=32'h4cdd2086;
end
else if(x==8'b00011001)
begin
  y=32'h8470eb26;
end
else if(x==8'b00011010)
begin
  y=32'h6382e9c6;
end
else if(x==8'b00011011)
begin
  y=32'h021ecc5e;
end
else if(x==8'b00011100)
begin
  y=32'h09686b3f;
end
else if(x==8'b00011101)
begin
  y=32'h3ebaefc9;
end
else if(x==8'b00011110)
begin
  y=32'h3c971814;
end
else if(x==8'b00011111)
begin
    y=32'h6b6a70a1;
  end
  else if (x==8'b00100000)
begin
  y=32'h687f3584;
end
else if(x==8'b00100001)
begin
  y=32'h52a0e286;
end
else if(x==8'b00100010)
  begin
    y=32'hb79c5305;
    end
else if(x==8'b00100011)
  begin
    y=32'haa500737;
    end
    else if(x==8'b00100100)
  begin
    y=32'h3e07841c;
    end
    else if(x==8'b00100101)
  begin
    y=32'h7fdeae5c;
    end
    else if(x==8'b00100110)
  begin
    y=32'h8e7d44ec;
    end
    else if(x==8'b00100111)
  begin
    y=32'h5716f2b8;
    end
    else if(x==8'b00101000)
begin
  y=32'hb03ada37;
end
else if(x==8'b00101001)
begin
  y=32'hf0500c0d;
end
else if(x==8'b00101010)
begin
  y=32'hf01c1f04;
end
else if(x==8'b00101011)
begin
  y=32'h0200b3ff;
end
else if(x==8'b00101100)
begin
  y=32'hae0cf51a;
end
else if(x==8'b00101101)
begin
  y=32'h3cb574b2;
end
else if(x==8'b00101110)
begin
  y=32'h25837a58;
end
else if(x==8'b00101111)
begin
    y=32'hdc0921bd;
  end
  else if (x==8'b00110000)
begin
  y=32'hd19113f9;
end
else if(x==8'b00110001)
begin
  y=32'h7ca92ff6;
end
else if(x==8'b00110010)
  begin
    y=32'h94324773;
    end
else if(x==8'b00110011)
  begin
    y=32'h22f54701;
    end
    else if(x==8'b00110100)
  begin
    y=32'h3ae5e581;
    end
    else if(x==8'b00110101)
  begin
    y=32'h37c2dadc;
    end
    else if(x==8'b00110110)
  begin
    y=32'hc8b57634;
    end
    else if(x==8'b00110111)
  begin
    y=32'h9af3dda7;
    end
    else if(x==8'b00111000)
begin
  y=32'ha9446146;
end
else if(x==8'b00111001)
begin
  y=32'h0fd0030e;
end
else if(x==8'b00111010)
begin
  y=32'hecc8c73e;
end
else if(x==8'b00111011)
begin
  y=32'ha4751e41;
end
else if(x==8'b00111100)
begin
  y=32'he238cd99;
end
else if(x==8'b00111101)
begin
  y=32'h3bea0e2f;
end
else if(x==8'b00111110)
begin
  y=32'h3280bba1;
end
else if(x==8'b00111111)
begin
    y=32'h183eb331;
  end
else if (x==8'b01000000)
begin
  y=32'h4e548b38;
end
else if(x==8'b01000001)
begin
  y=32'h4f6db908;
end
else if(x==8'b01000010)
  begin
    y=32'h6f420d03;
    end
else if(x==8'b01000011)
  begin
    y=32'hf60a04bf;
    end
    else if(x==8'b01000100)
  begin
    y=32'h2cb81290;
    end
    else if(x==8'b01000101)
  begin
    y=32'h24977c79;
    end
    else if(x==8'b01000110)
  begin
    y=32'h5679b072;
    end
    else if(x==8'b01000111)
  begin
    y=32'hbcaf89af;
    end
    else if(x==8'b01001000)
  begin
    y=32'hde9a771f;
    end
    else if(x==8'b01001001)
  begin
    y=32'hd9930810;
    end
    else if(x==8'b01001010)
  begin
    y=32'hb38bae12;
    end
    else if(x==8'b01001011)
  begin
    y=32'hdccf3f2e;
    end
    else if(x==8'b01001100)
  begin
    y=32'h5512721f;
    end
    else if(x==8'b01001101)
  begin
    y=32'h2e6b7124;
    end
    else if(x==8'b01001110)
  begin
    y=32'h501adde6;
    end
    else if(x==8'b01001111)
  begin
    y=32'h9f84cd87;
    end
    else if (x==8'b01010000)
begin
  y=32'h7a584718;
end
else if(x==8'b01010001)
begin
  y=32'h7408da17;
end
else if(x==8'b01010010)
  begin
    y=32'hbc9f9abc;
    end
else if(x==8'b01010011)
  begin
    y=32'he94b7d8c;
    end
    else if(x==8'b01010100)
  begin
    y=32'hec7aec3a;
    end
    else if(x==8'b01010101)
  begin
    y=32'hdb851dfa;
    end
    else if(x==8'b01010110)
  begin
    y=32'h63094366;
    end
    else if(x==8'b01010111)
  begin
    y=32'hc464c3d2;
    end
    else if(x==8'b01011000)
  begin
    y=32'hef1c1847;
    end
    else if(x==8'b01011001)
  begin
    y=32'h3215d908;
    end
    else if(x==8'b01011010)
  begin
    y=32'hdd433b37;
    end
    else if(x==8'b01011011)
  begin
    y=32'h24c2ba16;
    end
    else if(x==8'b01011100)
  begin
    y=32'h12a14d43;
    end
    else if(x==8'b01011101)
  begin
    y=32'h2a65c451;
    end
    else if(x==8'b01011110)
  begin
    y=32'h50940002;
    end
    else if(x==8'b01011111)
  begin
    y=32'h133ae4dd;
    end
    else if (x==8'b01100000)
begin
  y=32'h71dff89e;
end
else if(x==8'b01100001)
begin
  y=32'h10314e55;
end
else if(x==8'b01100010)
  begin
    y=32'h81ac77d6;
    end
else if(x==8'b01100011)
  begin
    y=32'h5f11199b;
    end
    else if(x==8'b01100100)
  begin
    y=32'h043556f1;
    end
    else if(x==8'b01100101)
  begin
    y=32'hd7a3c76b;
    end
    else if(x==8'b01100110)
  begin
    y=32'h3c11183b;
    end
    else if(x==8'b01100111)
  begin
    y=32'h5924a509;
    end
    else if(x==8'b01101000)
  begin
    y=32'hf28fe6ed;
    end
    else if(x==8'b01101001)
  begin
    y=32'h97f1fbfa;
    end
    else if(x==8'b01101010)
  begin
    y=32'h9ebabf2c;
    end
    else if(x==8'b01101011)
  begin
    y=32'h1e153c6e;
    end
    else if(x==8'b01101100)
  begin
    y=32'h86e34570;
    end
    else if(x==8'b01101101)
  begin
    y=32'heae96fb1;
    end
    else if(x==8'b01101110)
  begin
    y=32'h860e5e0a;
    end
    else if(x==8'b01101111)
  begin
    y=32'h5a3e2ab3;
    end
else if (x==8'b01110000)
begin
  y=32'h771fe71c;
end
else if(x==8'b01110001)
begin
  y=32'h4e3d06fa;
end
else if(x==8'b01110010)
  begin
    y=32'h2965dcb9;
    end
else if(x==8'b01110011)
  begin
    y=32'h99e71d0f;
    end
    else if(x==8'b01110100)
  begin
    y=32'h803e89d6;
    end
    else if(x==8'b01110101)
  begin
    y=32'h5266c825;
    end
    else if(x==8'b01110110)
  begin
    y=32'h2e4cc978;
    end
    else if(x==8'b01110111)
  begin
    y=32'h9c10b36a;
    end
    else if(x==8'b01111000)
  begin
    y=32'hc6150eba;
    end
    else if(x==8'b01111001)
  begin
    y=32'h94e2ea78;
    end
    else if(x==8'b01111010)
  begin
    y=32'ha5fc3c53;
    end
    else if(x==8'b01111011)
  begin
    y=32'h1e0a2df4;
    end
    else if(x==8'b01111100)
  begin
    y=32'hf2f74ea7;
    end
    else if(x==8'b01111101)
  begin
    y=32'h361d2b3d;
    end
    else if(x==8'b01111110)
  begin
    y=32'h1939260f;
    end
    else if(x==8'b01111111)
  begin
    y=32'h19c27960;
    end
else if (x==8'b10000000)
begin
  y=32'h5223a708;
end
else if(x==8'b10000001)
begin
  y=32'hf71312b6;
end
else if(x==8'b10000010)
  begin
    y=32'hebadfe6e;
    end
else if(x==8'b10000011)
  begin
    y=32'heac31f66;
    end
    else if(x==8'b10000100)
  begin
    y=32'he3bc4595;
    end
    else if(x==8'b10000101)
  begin
    y=32'ha67bc883;
    end
    else if(x==8'b10000110)
  begin
    y=32'hb17f37d1;
    end
    else if(x==8'b10000111)
  begin
    y=32'h018cff28;
    end
    else if(x==8'b10001000)
  begin
    y=32'hc332ddef;
    end
    else if(x==8'b10001001)
  begin
    y=32'hbe6c5aa5;
    end
    else if(x==8'b10001010)
  begin
    y=32'h65582185;
    end
    else if(x==8'b10001011)
  begin
    y=32'h68ab9802;
    end
    else if(x==8'b10001100)
  begin
    y=32'heecea50f;
    end
    else if(x==8'b10001101)
  begin
    y=32'hdb2f953b;
    end
    else if(x==8'b10001110)
  begin
    y=32'h2aef7dad;
    end
    else if(x==8'b10001111)
  begin
    y=32'h5b6e2f84;
    end
    else if (x==8'b10010000)
begin
  y=32'h1521b628;
end
else if(x==8'b10010001)
begin
  y=32'h29076170;
end
else if(x==8'b10010010)
  begin
    y=32'hecdd4775;
    end
else if(x==8'b10010011)
  begin
    y=32'h619f1510;
    end
    else if(x==8'b10010100)
  begin
    y=32'h13cca830;
    end
    else if(x==8'b10010101)
  begin
    y=32'heb61bd96;
    end
    else if(x==8'b10010110)
  begin
    y=32'h0334fe1e;
    end
    else if(x==8'b10010111)
  begin
    y=32'haa0363cf;
    end
    else if(x==8'b10011000)
  begin
    y=32'hb5735c90;
    end
    else if(x==8'b10011001)
  begin
    y=32'h4c70a239;
    end
    else if(x==8'b10011010)
  begin
    y=32'hd59e9e0b;
    end
    else if(x==8'b10011011)
  begin
    y=32'hcbaade14;
    end
    else if(x==8'b10011100)
  begin
    y=32'heecc86bc;
    end
    else if(x==8'b10011101)
  begin
    y=32'h60622ca7;
    end
    else if(x==8'b10011110)
  begin
    y=32'h9cab5cab;
    end
    else if(x==8'b10011111)
  begin
    y=32'hb2f3846e;
    end
    else if (x==8'b10100000)
begin
  y=32'h648b1eaf;
end
else if(x==8'b10100001)
begin
  y=32'h19bdf0ca;
end
else if(x==8'b10100010)
  begin
    y=32'ha02369b9;
    end
else if(x==8'b10100011)
  begin
    y=32'h655abb50;
    end
    else if(x==8'b10100100)
  begin
    y=32'h40685a32;
    end
    else if(x==8'b10100101)
  begin
    y=32'h3c2ab4b3;
    end
    else if(x==8'b10100110)
  begin
    y=32'h319ee9d5;
    end
    else if(x==8'b10100111)
  begin
    y=32'hc021b8f7;
    end
    else if(x==8'b10101000)
  begin
    y=32'h9b540b19;
    end
    else if(x==8'b10101001)
  begin
    y=32'h875fa099;
    end
    else if(x==8'b10101010)
  begin
    y=32'h95f7997e;
    end
    else if(x==8'b10101011)
  begin
    y=32'h623d7da8;
    end
    else if(x==8'b10101100)
  begin
    y=32'hf837889a;
    end
    else if(x==8'b10101101)
  begin
    y=32'h97e32d77;
    end
    else if(x==8'b10101110)
  begin
    y=32'h11ed935f;
    end
    else if(x==8'b10101111)
  begin
    y=32'h16681281;
    end
   else if (x==8'b10110000)
begin
  y=32'h0e358829;
end
else if(x==8'b10110001)
begin
  y=32'hc7e61fd6;
end
else if(x==8'b10110010)
  begin
    y=32'h96dedfa1;
    end
else if(x==8'b10110011)
  begin
    y=32'h7858ba99;
    end
    else if(x==8'b10110100)
  begin
    y=32'h57f584a5;
    end
    else if(x==8'b10110101)
  begin
    y=32'h1b227263;
    end
    else if(x==8'b10110110)
  begin
    y=32'h9b83c3ff;
    end
    else if(x==8'b10110111)
  begin
    y=32'h1ac24696;
    end
    else if(x==8'b10111000)
  begin
    y=32'hcdb30aeb;
    end
    else if(x==8'b10111001)
  begin
    y=32'h532e3054;
    end
    else if(x==8'b10111010)
  begin
    y=32'h8fd948e4;
    end
    else if(x==8'b10111011)
  begin
    y=32'h6dbc3128;
    end
    else if(x==8'b10111100)
  begin
    y=32'h58ebf2ef;
    end
    else if(x==8'b10111101)
  begin
    y=32'h34c6ffea;
    end
    else if(x==8'b10111110)
  begin
    y=32'hfe28ed61;
    end
    else if(x==8'b10111111)
  begin
    y=32'hee7c3c73;
    end
    else if (x==8'b11000000)
begin
  y=32'h5d4a14d9;
end
else if(x==8'b11000001)
begin
  y=32'he864b7e3;
end
else if(x==8'b11000010)
  begin
    y=32'h42105d14;
    end
else if(x==8'b11000011)
  begin
    y=32'h203e13e0;
    end
    else if(x==8'b11000100)
  begin
    y=32'h45eee2b6;
    end
    else if(x==8'b11000101)
  begin
    y=32'ha3aaabea;
    end
    else if(x==8'b11000110)
  begin
    y=32'hdb6c4f15;
    end
    else if(x==8'b11000111)
  begin
    y=32'hfacb4fd0;
    end
    else if(x==8'b11001000)
  begin
    y=32'hc742f442;
    end
    else if(x==8'b11001001)
  begin
    y=32'hef6abbb5;
    end
    else if(x==8'b11001010)
  begin
    y=32'h654f3b1d;
    end
    else if(x==8'b11001011)
  begin
    y=32'h41cd2105;
    end
    else if(x==8'b11001100)
  begin
    y=32'hd81e799e;
    end
    else if(x==8'b11001101)
  begin
    y=32'h86854dc7;
    end
    else if(x==8'b11001110)
  begin
    y=32'he44b476a;
    end
    else if(x==8'b11001111)
  begin
    y=32'h3d816250;
    end
   else if (x==8'b11010000)
begin
  y=32'hcf62a1f2;
end
else if(x==8'b11010001)
begin
  y=32'h5b8d2646;
end
else if(x==8'b11010010)
  begin
    y=32'hfc8883a0;
    end
else if(x==8'b11010011)
  begin
    y=32'hc1c7b6a3;
    end
    else if(x==8'b11010100)
  begin
    y=32'h7f1524c3;
    end
    else if(x==8'b11010101)
  begin
    y=32'h69cb7492;
    end
    else if(x==8'b11010110)
  begin
    y=32'h47848a0b;
    end
    else if(x==8'b11010111)
  begin
    y=32'h5692b285;
    end
    else if(x==8'b11011000)
  begin
    y=32'h095bbf00;
    end
    else if(x==8'b11011001)
  begin
    y=32'had19489d;
    end
    else if(x==8'b11011010)
  begin
    y=32'h1462b174;
    end
    else if(x==8'b11011011)
  begin
    y=32'h23820e00;
    end
    else if(x==8'b11011100)
  begin
    y=32'h58428d2a;
    end
    else if(x==8'b11011101)
  begin
    y=32'h0c55f5ea;
    end
    else if(x==8'b11011110)
  begin
    y=32'h1dadf43e;
    end
    else if(x==8'b11011111)
  begin
    y=32'h233f7061;
    end
    else if (x==8'b11100000)
begin
  y=32'h3372f092;
end
else if(x==8'b11100001)
begin
  y=32'h8d937e41;
end
else if(x==8'b11100010)
  begin
    y=32'hd65fecf1;
    end
else if(x==8'b11100011)
  begin
    y=32'h6c223bdb;
    end
    else if(x==8'b11100100)
  begin
    y=32'h7cde3759;
    end
    else if(x==8'b11100101)
  begin
    y=32'hcbee7460;
    end
    else if(x==8'b11100110)
  begin
    y=32'h4085f2a7;
    end
    else if(x==8'b11100111)
  begin
    y=32'hce77326e;
    end
    else if(x==8'b11101000)
  begin
    y=32'ha6078084;
    end
    else if(x==8'b11101001)
  begin
    y=32'h19f8509e;
    end
    else if(x==8'b11101010)
  begin
    y=32'he8efd855;
    end
    else if(x==8'b11101011)
  begin
    y=32'h61d99735;
    end
    else if(x==8'b11101100)
  begin
    y=32'ha969a7aa;
    end
    else if(x==8'b11101101)
  begin
    y=32'hc50c06c2;
    end
    else if(x==8'b11101110)
  begin
    y=32'h5a04abfc;
    end
    else if(x==8'b11101111)
  begin
    y=32'h800bcadc;
    end
    else if (x==8'b11110000)
begin
  y=32'h9e447a2e;
end
else if(x==8'b11110001)
begin
  y=32'hc3453484;
end
else if(x==8'b11110010)
  begin
    y=32'hfdd56705;
    end
else if(x==8'b11110011)
  begin
    y=32'h0e1e9ec9;
    end
    else if(x==8'b11110100)
  begin
    y=32'hdb73dbd3;
    end
    else if(x==8'b11110101)
  begin
    y=32'h105588cd;
    end
    else if(x==8'b11110110)
  begin
    y=32'h675fda79;
    end
    else if(x==8'b11110111)
  begin
    y=32'he3674340;
    end
    else if(x==8'b11111000)
  begin
    y=32'hc5c43465;
    end
    else if(x==8'b11111001)
  begin
    y=32'h713e38d8;
    end
    else if(x==8'b11111010)
  begin
    y=32'h3d28f89e;
    end
    else if(x==8'b11111011)
  begin
    y=32'hf16dff20;
    end
    else if(x==8'b11111100)
  begin
    y=32'h153e21e7;
    end
    else if(x==8'b11111101)
  begin
    y=32'h8fb03d4a;
    end
    else if(x==8'b11111110)
  begin
    y=32'he6e39f2b;
    end
    else if(x==8'b11111111)
  begin
    y=32'hdb83adf7;
    end
     end
endmodule


module bf_loopup_s3box(x,y);
input [7:0]x;
output reg [31:0]y;
always @(x)
begin
if (x==8'b00000000)
begin
  y=32'he93d5a68;
end
else if(x==8'b00000001)
begin
  y=32'h948140f7;
end
else if(x==8'b00000010)
  begin
    y=32'hf64c261c;
    end
else if(x==8'b00000011)
  begin
    y=32'h94692934;
    end
    else if(x==8'b00000100)
  begin
    y=32'h411520f7;
    end
    else if(x==8'b00000101)
  begin
    y=32'h7602d4f7;
    end
    else if(x==8'b00000110)
  begin
    y=32'hbcf46b2e;
    end
    else if(x==8'b00000111)
  begin
    y=32'hd4a20068;
    end
    else if(x==8'b00001000)
  begin
    y=32'hd4082471;
    end
    else if(x==8'b00001001)
  begin
    y=32'h3320f46a;
    end
    else if(x==8'b00001010)
  begin
    y=32'h43b7d4b7;
    end
    else if(x==8'b00001011)
  begin
    y=32'h500061af;
    end
    else if(x==8'b00001100)
  begin
    y=32'h1e39f62e;
    end
    else if(x==8'b00001101)
  begin
    y=32'h97244546;
    end
    else if(x==8'b00001110)
  begin
    y=32'h14214f74;
    end
    else if(x==8'b00001111)
  begin
    y=32'hbf8b8840;
    end
 else if (x==8'b00010000)
begin
  y=32'h4d95fc1d;
end
else if(x==8'b00010001)
begin
  y=32'h96b591af;
end
else if(x==8'b00010010)
  begin
    y=32'h70f4ddd3;
    end
else if(x==8'b00010011)
  begin
    y=32'h66a02f45;
    end
    else if(x==8'b00010100)
  begin
    y=32'hbfbc09ec;
    end
    else if(x==8'b00010101)
  begin
    y=32'h03bd9785;
    end
    else if(x==8'b00010110)
  begin
    y=32'h7fac6dd0;
    end
    else if(x==8'b00010111)
  begin
    y=32'h31cb8504;
    end
    else if(x==8'b00011000)
begin
  y=32'h96eb27b3;
end
else if(x==8'b00011001)
begin
  y=32'h55fd3941;
end
else if(x==8'b00011010)
begin
  y=32'hda2547e6;
end
else if(x==8'b00011011)
begin
  y=32'habca0a9a;
end
else if(x==8'b00011100)
begin
  y=32'h28507825;
end
else if(x==8'b00011101)
begin
  y=32'h530429f4;
end
else if(x==8'b00011110)
begin
  y=32'h0a2c86da;
end
else if(x==8'b00011111)
begin
    y=32'he9b66dfb;
  end
  else if (x==8'b00100000)
begin
  y=32'h68dc1462;
end
else if(x==8'b00100001)
begin
  y=32'hd7486900;
end
else if(x==8'b00100010)
  begin
    y=32'h680ec0a4;
    end
else if(x==8'b00100011)
  begin
    y=32'h27a18dee;
    end
    else if(x==8'b00100100)
  begin
    y=32'h4f3ffea2;
    end
    else if(x==8'b00100101)
  begin
    y=32'he887ad8c;
    end
    else if(x==8'b00100110)
  begin
    y=32'hb58ce006;
    end
    else if(x==8'b00100111)
  begin
    y=32'h7af4d6b6;
    end
    else if(x==8'b00101000)
begin
  y=32'haace1e7c;
end
else if(x==8'b00101001)
begin
  y=32'hd3375fec;
end
else if(x==8'b00101010)
begin
  y=32'hce78a399;
end
else if(x==8'b00101011)
begin
  y=32'h406b2a42;
end
else if(x==8'b00101100)
begin
  y=32'h20fe9e35;
end
else if(x==8'b00101101)
begin
  y=32'hd9f385b9;
end
else if(x==8'b00101110)
begin
  y=32'hee39d7ab;
end
else if(x==8'b00101111)
begin
    y=32'h3b124e8b;
  end
  else if (x==8'b00110000)
begin
  y=32'h1dc9faf7;
end
else if(x==8'b00110001)
begin
  y=32'h4b6d1856;
end
else if(x==8'b00110010)
  begin
    y=32'h26a36631;
    end
else if(x==8'b00110011)
  begin
    y=32'heae397b2;
    end
    else if(x==8'b00110100)
  begin
    y=32'h3a6efa74;
    end
    else if(x==8'b00110101)
  begin
    y=32'hdd5b4332;
    end
    else if(x==8'b00110110)
  begin
    y=32'h6841e7f7;
    end
    else if(x==8'b00110111)
  begin
    y=32'hca7820fb;
    end
    else if(x==8'b00111000)
begin
  y=32'hfb0af54e;
end
else if(x==8'b00111001)
begin
  y=32'hd8feb397;
end
else if(x==8'b00111010)
begin
  y=32'h454056ac;
end
else if(x==8'b00111011)
begin
  y=32'hba489527;
end
else if(x==8'b00111100)
begin
  y=32'h55533a3a;
end
else if(x==8'b00111101)
begin
  y=32'h20838d87;
end
else if(x==8'b00111110)
begin
  y=32'hfe6ba9b7;
end
else if(x==8'b00111111)
begin
    y=32'hd096954b;
  end
else if (x==8'b01000000)
begin
  y=32'h55a867bc;
end
else if(x==8'b01000001)
begin
  y=32'ha1159a58;
end
else if(x==8'b01000010)
  begin
    y=32'hcca92963;
    end
else if(x==8'b01000011)
  begin
    y=32'h99e1db33;
    end
    else if(x==8'b01000100)
  begin
    y=32'ha62a4a56;
    end
    else if(x==8'b01000101)
  begin
    y=32'h3f3125f9;
    end
    else if(x==8'b01000110)
  begin
    y=32'h5ef47e1c;
    end
    else if(x==8'b01000111)
  begin
    y=32'h9029317c;
    end
    else if(x==8'b01001000)
  begin
    y=32'hfdf8e802;
    end
    else if(x==8'b01001001)
  begin
    y=32'h04272f70;
    end
    else if(x==8'b01001010)
  begin
    y=32'h80bb155c;
    end
    else if(x==8'b01001011)
  begin
    y=32'h05282ce3;
    end
    else if(x==8'b01001100)
  begin
    y=32'h95c11548;
    end
    else if(x==8'b01001101)
  begin
    y=32'he4c66d22;
    end
    else if(x==8'b01001110)
  begin
    y=32'h48c1133f;
    end
    else if(x==8'b01001111)
  begin
    y=32'hc70f86dc;
    end
    else if (x==8'b01010000)
begin
  y=32'h07f9c9ee;
end
else if(x==8'b01010001)
begin
  y=32'h41041f0f;
end
else if(x==8'b01010010)
  begin
    y=32'h404779a4;
    end
else if(x==8'b01010011)
  begin
    y=32'h5d886e17;
    end
    else if(x==8'b01010100)
  begin
    y=32'h325f51eb;
    end
    else if(x==8'b01010101)
  begin
    y=32'hd59bc0d1;
    end
    else if(x==8'b01010110)
  begin
    y=32'hf2bcc18f;
    end
    else if(x==8'b01010111)
  begin
    y=32'h41113564;
    end
    else if(x==8'b01011000)
  begin
    y=32'h257b7834;
    end
    else if(x==8'b01011001)
  begin
    y=32'h602a9c60;
    end
    else if(x==8'b01011010)
  begin
    y=32'hdff8e8a3;
    end
    else if(x==8'b01011011)
  begin
    y=32'h1f636c1b;
    end
    else if(x==8'b01011100)
  begin
    y=32'h0e12b4c2;
    end
    else if(x==8'b01011101)
  begin
    y=32'h02e1329e;
    end
    else if(x==8'b01011110)
  begin
    y=32'haf664fd1;
    end
    else if(x==8'b01011111)
  begin
    y=32'hcad18115;
    end
    else if (x==8'b01100000)
begin
  y=32'h6b2395e0;
end
else if(x==8'b01100001)
begin
  y=32'h333e92e1;
end
else if(x==8'b01100010)
  begin
    y=32'h3b240b62;
    end
else if(x==8'b01100011)
  begin
    y=32'heebeb922;
    end
    else if(x==8'b01100100)
  begin
    y=32'h85b2a20e;
    end
    else if(x==8'b01100101)
  begin
    y=32'he6ba0d99;
    end
    else if(x==8'b01100110)
  begin
    y=32'hde720c8c;
    end
    else if(x==8'b01100111)
  begin
    y=32'h2da2f728;
    end
    else if(x==8'b01101000)
  begin
    y=32'hd0127845;
    end
    else if(x==8'b01101001)
  begin
    y=32'h95b794fd;
    end
    else if(x==8'b01101010)
  begin
    y=32'h647d0862;
    end
    else if(x==8'b01101011)
  begin
    y=32'he7ccf5f0;
    end
    else if(x==8'b01101100)
  begin
    y=32'h5449a36f;
    end
    else if(x==8'b01101101)
  begin
    y=32'h877d48fa;
    end
    else if(x==8'b01101110)
  begin
    y=32'hc39dfd27;
    end
    else if(x==8'b01101111)
  begin
    y=32'hf33e8d1e;
    end
   else if (x==8'b01110000)
begin
  y=32'h0a476341;
end
else if(x==8'b01110001)
begin
  y=32'h992eff74;
end
else if(x==8'b01110010)
  begin
    y=32'h3a6f6eab;
    end
else if(x==8'b01110011)
  begin
    y=32'hf4f8fd37;
    end
    else if(x==8'b01110100)
  begin
    y=32'ha812dc60;
    end
    else if(x==8'b01110101)
  begin
    y=32'ha1ebddf8;
    end
    else if(x==8'b01110110)
  begin
    y=32'h991be14c;
    end
    else if(x==8'b01110111)
  begin
    y=32'hdb6e6b0d;
    end
    else if(x==8'b01111000)
  begin
    y=32'hc67b5510;
    end
    else if(x==8'b01111001)
  begin
    y=32'h6d672c37;
    end
    else if(x==8'b01111010)
  begin
    y=32'h2765d43b;
    end
    else if(x==8'b01111011)
  begin
    y=32'hdcd0e804;
    end
    else if(x==8'b01111100)
  begin
    y=32'hf1290dc7;
    end
    else if(x==8'b01111101)
  begin
    y=32'hcc00ffa3;
    end
    else if(x==8'b01111110)
  begin
    y=32'hb5390f92;
    end
    else if(x==8'b01111111)
  begin
    y=32'h690fed0b;
    end 
else if (x==8'b10000000)
begin
  y=32'h667b9ffb;
end
else if(x==8'b10000001)
begin
  y=32'hcedb7d9c;
end
else if(x==8'b10000010)
  begin
    y=32'ha091cf0b;
    end
else if(x==8'b10000011)
  begin
    y=32'hd9155ea3;
    end
    else if(x==8'b10000100)
  begin
    y=32'hbb132f88;
    end
    else if(x==8'b10000101)
  begin
    y=32'h515bad24;
    end
    else if(x==8'b10000110)
  begin
    y=32'h7b9479bf;
    end
    else if(x==8'b10000111)
  begin
    y=32'h763bd6eb;
    end
    else if(x==8'b10001000)
  begin
    y=32'h37392eb3;
    end
    else if(x==8'b10001001)
  begin
    y=32'hcc115979;
    end
    else if(x==8'b10001010)
  begin
    y=32'h8026e297;
    end
    else if(x==8'b10001011)
  begin
    y=32'hf42e312d;
    end
    else if(x==8'b10001100)
  begin
    y=32'h6842ada7;
    end
    else if(x==8'b10001101)
  begin
    y=32'hc66a2b3b;
    end
    else if(x==8'b10001110)
  begin
    y=32'h12754ccc;
    end
    else if(x==8'b10001111)
  begin
    y=32'h782ef11c;
    end
    else if (x==8'b10010000)
begin
  y=32'h6a124237;
end
else if(x==8'b10010001)
begin
  y=32'hb79251e7;
end
else if(x==8'b10010010)
  begin
    y=32'h06a1bbe6;
    end
else if(x==8'b10010011)
  begin
    y=32'h4bfb6350;
    end
    else if(x==8'b10010100)
  begin
    y=32'h1a6b1018;
    end
    else if(x==8'b10010101)
  begin
    y=32'h11caedfa;
    end
    else if(x==8'b10010110)
  begin
    y=32'h3d25bdd8;
    end
    else if(x==8'b10010111)
  begin
    y=32'he2e1c3c9;
    end
    else if(x==8'b10011000)
  begin
    y=32'h44421659;
    end
    else if(x==8'b10011001)
  begin
    y=32'h0a121386;
    end
    else if(x==8'b10011010)
  begin
    y=32'hd90cec6e;
    end
    else if(x==8'b10011011)
  begin
    y=32'hd5abea2a;
    end
    else if(x==8'b10011100)
  begin
    y=32'h64af674e;
    end
    else if(x==8'b10011101)
  begin
    y=32'hda86a85f;
    end
    else if(x==8'b10011110)
  begin
    y=32'hbebfe988;
    end
    else if(x==8'b10011111)
  begin
    y=32'h64e4c3fe;
    end
    else if (x==8'b10100000)
begin
  y=32'h9dbc8057;
end
else if(x==8'b10100001)
begin
  y=32'hf0f7c086;
end
else if(x==8'b10100010)
  begin
    y=32'h60787bf8;
    end
else if(x==8'b10100011)
  begin
    y=32'h6003604d;
    end
    else if(x==8'b10100100)
  begin
    y=32'hd1fd8346;
    end
    else if(x==8'b10100101)
  begin
    y=32'hf6381fb0;
    end
    else if(x==8'b10100110)
  begin
    y=32'h7745ae04;
    end
    else if(x==8'b10100111)
  begin
    y=32'hd736fccc;
    end
    else if(x==8'b10101000)
  begin
    y=32'h83426b33;
    end
    else if(x==8'b10101001)
  begin
    y=32'hf01eab71;
    end
    else if(x==8'b10101010)
  begin
    y=32'hb0804187;
    end
    else if(x==8'b10101011)
  begin
    y=32'h3c005e5f;
    end
    else if(x==8'b10101100)
  begin
    y=32'h77a057be;
    end
    else if(x==8'b10101101)
  begin
    y=32'hbde8ae24;
    end
    else if(x==8'b10101110)
  begin
    y=32'h55464299;
    end
    else if(x==8'b10101111)
  begin
    y=32'hbf582e61;
    end
   else if (x==8'b10110000)
begin
  y=32'h4e58f48f;
end
else if(x==8'b10110001)
begin
  y=32'hf2ddfda2;
end
else if(x==8'b10110010)
  begin
    y=32'hf474ef38;
    end
else if(x==8'b10110011)
  begin
    y=32'h8789bdc2;
    end
    else if(x==8'b10110100)
  begin
    y=32'h5366f9c3;
    end
    else if(x==8'b10110101)
  begin
    y=32'hc8b38e74;
    end
    else if(x==8'b10110110)
  begin
    y=32'hb475f255;
    end
    else if(x==8'b10110111)
  begin
    y=32'h46fcd9b9;
    end
    else if(x==8'b10111000)
  begin
    y=32'h7aeb2661;
    end
    else if(x==8'b10111001)
  begin
    y=32'h8b1ddf84;
    end
    else if(x==8'b10111010)
  begin
    y=32'h846a0e79;
    end
    else if(x==8'b10111011)
  begin
    y=32'h915f95e2;
    end
    else if(x==8'b10111100)
  begin
    y=32'h466e598e;
    end
    else if(x==8'b10111101)
  begin
    y=32'h20b45770;
    end
    else if(x==8'b10111110)
  begin
    y=32'h8cd55591;
    end
    else if(x==8'b10111111)
  begin
    y=32'hc902de4c;
    end
    else if (x==8'b11000000)
begin
  y=32'hb90bace1;
end
else if(x==8'b11000001)
begin
  y=32'hbb8205d0;
end
else if(x==8'b11000010)
  begin
    y=32'h11a86248;
    end
else if(x==8'b11000011)
  begin
    y=32'h7574a99e;
    end
    else if(x==8'b11000100)
  begin
    y=32'hb77f19b6;
    end
    else if(x==8'b11000101)
  begin
    y=32'he0a9dc09;
    end
    else if(x==8'b11000110)
  begin
    y=32'h662d09a1;
    end
    else if(x==8'b11000111)
  begin
    y=32'hc4324633;
    end
    else if(x==8'b11001000)
  begin
    y=32'he85a1f02;
    end
    else if(x==8'b11001001)
  begin
    y=32'h09f0be8c;
    end
    else if(x==8'b11001010)
  begin
    y=32'h4a99a025;
    end
    else if(x==8'b11001011)
  begin
    y=32'h1d6efe10;
    end
    else if(x==8'b11001100)
  begin
    y=32'h1ab93d1d;
    end
    else if(x==8'b11001101)
  begin
    y=32'h0ba5a4df;
    end
    else if(x==8'b11001110)
  begin
    y=32'ha186f20f;
    end
    else if(x==8'b11001111)
  begin
    y=32'h2868f169;
    end
   else if (x==8'b11010000)
begin
  y=32'hdcb7da83;
end
else if(x==8'b11010001)
begin
  y=32'h573906fe;
end
else if(x==8'b11010010)
  begin
    y=32'ha1e2ce9b;
    end
else if(x==8'b11010011)
  begin
    y=32'h4fcd7f52;
    end
    else if(x==8'b11010100)
  begin
    y=32'h50115e01;
    end
    else if(x==8'b11010101)
  begin
    y=32'ha70683fa;
    end
    else if(x==8'b11010110)
  begin
    y=32'ha002b5c4;
    end
    else if(x==8'b11010111)
  begin
    y=32'h0de6d027;
    end
    else if(x==8'b11011000)
  begin
    y=32'h9af88c27;
    end
    else if(x==8'b11011001)
  begin
    y=32'h773f8641;
    end
    else if(x==8'b11011010)
  begin
    y=32'hc3604c06;
    end
    else if(x==8'b11011011)
  begin
    y=32'h61a806b5;
    end
    else if(x==8'b11011100)
  begin
    y=32'hf0177a28;
    end
    else if(x==8'b11011101)
  begin
    y=32'hc0f586e0;
    end
    else if(x==8'b11011110)
  begin
    y=32'h006058aa;
    end
    else if(x==8'b11011111)
  begin
    y=32'h30dc7d62;
    end
    else if (x==8'b11100000)
begin
  y=32'h11e69ed7;
end
else if(x==8'b11100001)
begin
  y=32'h2338ea63;
end
else if(x==8'b11100010)
  begin
    y=32'h53c2dd94;
    end
else if(x==8'b11100011)
  begin
    y=32'hc2c21634;
    end
    else if(x==8'b11100100)
  begin
    y=32'hbbcbee56;
    end
    else if(x==8'b11100101)
  begin
    y=32'h90bcb6de;
    end
    else if(x==8'b11100110)
  begin
    y=32'h90bcb6de;
    end
    else if(x==8'b11100111)
  begin
    y=32'hce591d76;
    end
    else if(x==8'b11101000)
  begin
    y=32'h6f05e409;
    end
    else if(x==8'b11101001)
  begin
    y=32'h4b7c0188;
    end
    else if(x==8'b11101010)
  begin
    y=32'h39720a3d;
    end
    else if(x==8'b11101011)
  begin
    y=32'h7c927c24;
    end
    else if(x==8'b11101100)
  begin
    y=32'h86e3725f;
    end
    else if(x==8'b11101101)
  begin
    y=32'h724d9db9;
    end
    else if(x==8'b11101110)
  begin
    y=32'h1ac15bb4;
    end
    else if(x==8'b11101111)
  begin
    y=32'hd39eb8fc;
    end
    else if (x==8'b11110000)
begin
  y=32'hed545578;
end
else if(x==8'b11110001)
begin
  y=32'h08fca5b5;
end
else if(x==8'b11110010)
  begin
    y=32'hd83d7cd3;
    end
else if(x==8'b11110011)
  begin
    y=32'h4dad0fc4;
    end
    else if(x==8'b11110100)
  begin
    y=32'h1e50ef5e;
    end
    else if(x==8'b11110101)
  begin
    y=32'hb161e6f8;
    end
    else if(x==8'b11110110)
  begin
    y=32'ha28514d9;
    end
    else if(x==8'b11110111)
  begin
    y=32'h6c51133c;
    end
    else if(x==8'b11111000)
  begin
    y=32'h6fd5c7e7;
    end
    else if(x==8'b11111001)
  begin
    y=32'h56e14ec4;
    end
    else if(x==8'b11111010)
  begin
    y=32'h362abfce;
    end
    else if(x==8'b11111011)
  begin
    y=32'hddc6c837;
    end
    else if(x==8'b11111100)
  begin
    y=32'hd79a3234;
    end
    else if(x==8'b11111101)
  begin
    y=32'h92638212;
    end
    else if(x==8'b11111110)
  begin
    y=32'h670efa8e;
    end
    else if(x==8'b11111111)
  begin
    y=32'h406000e0;
    end
     end
endmodule

module bf_loopup_s4box(x,y);
input [7:0]x;
output reg [31:0]y;
always @(x)
begin
if (x==8'b00000000)
begin
  y=32'h3a39ce37;
end
else if(x==8'b00000001)
begin
  y=32'hd3faf5cf;
end
else if(x==8'b00000010)
  begin
    y=32'habc27737;
    end
else if(x==8'b00000011)
  begin
    y=32'h5ac52d1b;
    end
    else if(x==8'b00000100)
  begin
    y=32'h5cb0679e;
    end
    else if(x==8'b00000101)
  begin
    y=32'h4fa33742;
    end
    else if(x==8'b00000110)
  begin
    y=32'hd3822740;
    end
    else if(x==8'b00000111)
  begin
    y=32'h99bc9bbe;
    end
    else if(x==8'b00001000)
  begin
    y=32'hd5118e9d;
    end
    else if(x==8'b00001001)
  begin
    y=32'hbf0f7315;
    end
    else if(x==8'b00001010)
  begin
    y=32'hd62d1c7e;
    end
    else if(x==8'b00001011)
  begin
    y=32'hc700c47b;
    end
    else if(x==8'b00001100)
  begin
    y=32'hb78c1b6b;
    end
    else if(x==8'b00001101)
  begin
    y=32'h21a19045;
    end
    else if(x==8'b00001110)
  begin
    y=32'hb26eb1be;
    end
    else if(x==8'b00001111)
  begin
    y=32'h6a366eb4;
    end
 else if (x==8'b00010000)
begin
  y=32'h5748ab2f;
end
else if(x==8'b00010001)
begin
  y=32'hbc946e79;
end
else if(x==8'b00010010)
  begin
    y=32'hc6a376d2;
    end
else if(x==8'b00010011)
  begin
    y=32'h6549c2c8;
    end
    else if(x==8'b00010100)
  begin
    y=32'h530ff8ee;
    end
    else if(x==8'b00010101)
  begin
    y=32'h468dde7d;
    end
    else if(x==8'b00010110)
  begin
    y=32'hd5730a1d;
    end
    else if(x==8'b00010111)
  begin
    y=32'h4cd04dc6;
    end
    else if(x==8'b00011000)
begin
  y=32'h2939bbdb;
end
else if(x==8'b00011001)
begin
  y=32'ha9ba4650;
end
else if(x==8'b00011010)
begin
  y=32'hac9526e8;
end
else if(x==8'b00011011)
begin
  y=32'hbe5ee304;
end
else if(x==8'b00011100)
begin
  y=32'ha1fad5f0;
end
else if(x==8'b00011101)
begin
  y=32'h6a2d519a;
end
else if(x==8'b00011110)
begin
  y=32'h63ef8ce2;
end
else if(x==8'b00011111)
begin
    y=32'h9a86ee22;
  end
  else if (x==8'b00100000)
begin
  y=32'hc089c2b8;
end
else if(x==8'b00100001)
begin
  y=32'h43242ef6;
end
else if(x==8'b00100010)
  begin
    y=32'ha51e03aa;
    end
else if(x==8'b00100011)
  begin
    y=32'h9cf2d0a4;
    end
    else if(x==8'b00100100)
  begin
    y=32'h83c061ba;
    end
    else if(x==8'b00100101)
  begin
    y=32'h9be96a4d;
    end
    else if(x==8'b00100110)
  begin
    y=32'h8fe51550;
    end
    else if(x==8'b00100111)
  begin
    y=32'hba645bd6;
    end
    else if(x==8'b00101000)
begin
  y=32'h2826a2f9;
end
else if(x==8'b00101001)
begin
  y=32'ha73a3ae1;
end
else if(x==8'b00101010)
begin
  y=32'h4ba99586;
end
else if(x==8'b00101011)
begin
  y=32'hef5562e9;
end
else if(x==8'b00101100)
begin
  y=32'hc72fefd3;
end
else if(x==8'b00101101)
begin
  y=32'hf752f7da;
end
else if(x==8'b00101110)
begin
  y=32'h3f046f69;
end
else if(x==8'b00101111)
begin
    y=32'h77fa0a59;
  end
  else if (x==8'b00110000)
begin
  y=32'h80e4a915;
end
else if(x==8'b00110001)
begin
  y=32'h87b08601;
end
else if(x==8'b00110010)
  begin
    y=32'h9b09e6ad;
    end
else if(x==8'b00110011)
  begin
    y=32'h3b3ee593;
    end
    else if(x==8'b00110100)
  begin
    y=32'he990fd5a;
    end
    else if(x==8'b00110101)
  begin
    y=32'h9e34d797;
    end
    else if(x==8'b00110110)
  begin
    y=32'h2cf0b7d9;
    end
    else if(x==8'b00110111)
  begin
    y=32'h022b8b51;
    end
    else if(x==8'b00111000)
begin
  y=32'h96d5ac3a;
end
else if(x==8'b00111001)
begin
  y=32'h017da67d;
end
else if(x==8'b00111010)
begin
  y=32'hd1cf3ed6;
end
else if(x==8'b00111011)
begin
  y=32'h7c7d2d28;
end
else if(x==8'b00111100)
begin
  y=32'h1f9f25cf;
end
else if(x==8'b00111101)
begin
  y=32'hadf2b89b;
end
else if(x==8'b00111110)
begin
  y=32'h5ad6b472;
end
else if(x==8'b00111111)
begin
    y=32'h5a88f54c;
  end
else if (x==8'b01000000)
begin
  y=32'he029ac71;
end
else if(x==8'b01000001)
begin
  y=32'he019a5e6;
end
else if(x==8'b01000010)
  begin
    y=32'h47b0acfd;
    end
else if(x==8'b01000011)
  begin
    y=32'hed93fa9b;
    end
    else if(x==8'b01000100)
  begin
    y=32'he8d3c48d;
    end
    else if(x==8'b01000101)
  begin
    y=32'h283b57cc;
    end
    else if(x==8'b01000110)
  begin
    y=32'hf8d56629;
    end
    else if(x==8'b01000111)
  begin
    y=32'h79132e28;
    end
    else if(x==8'b01001000)
  begin
    y=32'h785f0191;
    end
    else if(x==8'b01001001)
  begin
    y=32'hed756055;
    end
    else if(x==8'b01001010)
  begin
    y=32'hf7960e44;
    end
    else if(x==8'b01001011)
  begin
    y=32'he3d35e8c;
    end
    else if(x==8'b01001100)
  begin
    y=32'h15056dd4;
    end
    else if(x==8'b01001101)
  begin
    y=32'h88f46dba;
    end
    else if(x==8'b01001110)
  begin
    y=32'h03a16125;
    end
    else if(x==8'b01001111)
  begin
    y=32'h0564f0bd;
    end
    else if (x==8'b01010000)
begin
  y=32'hc3eb9e15;
end
else if(x==8'b01010001)
begin
  y=32'h3c9057a2;
end
else if(x==8'b01010010)
  begin
    y=32'h97271aec;
    end
else if(x==8'b01010011)
  begin
    y=32'ha93a072a;
    end
    else if(x==8'b01010100)
  begin
    y=32'h1b3f6d9b;
    end
    else if(x==8'b01010101)
  begin
    y=32'h1e6321f5;
    end
    else if(x==8'b01010110)
  begin
    y=32'hf59c66fb;
    end
    else if(x==8'b01010111)
  begin
    y=32'h26dcf319;
    end
    else if(x==8'b01011000)
  begin
    y=32'h7533d928;
    end
    else if(x==8'b01011001)
  begin
    y=32'hb155fdf5;
    end
    else if(x==8'b01011010)
  begin
    y=32'h03563482;
    end
    else if(x==8'b01011011)
  begin
    y=32'h8aba3cbb;
    end
    else if(x==8'b01011100)
  begin
    y=32'h28517711;
    end
    else if(x==8'b01011101)
  begin
    y=32'hc20ad9f8;
    end
    else if(x==8'b01011110)
  begin
    y=32'habcc5167;
    end
    else if(x==8'b01011111)
  begin
    y=32'hccad925f;
    end
    else if (x==8'b01100000)
begin
  y=32'h4de81751;
end
else if(x==8'b01100001)
begin
  y=32'h3830dc8e;
end
else if(x==8'b01100010)
  begin
    y=32'h379d5862;
    end
else if(x==8'b01100011)
  begin
    y=32'h9320f991;
    end
    else if(x==8'b01100100)
  begin
    y=32'hea7a90c2;
    end
    else if(x==8'b01100101)
  begin
    y=32'hfb3e7bce;
    end
    else if(x==8'b01100110)
  begin
    y=32'h5121ce64;
    end
    else if(x==8'b01100111)
  begin
    y=32'h774fbe32;
    end
    else if(x==8'b01101000)
  begin
    y=32'ha8b6e37e;
    end
    else if(x==8'b01101001)
  begin
    y=32'hc3293d46;
    end
    else if(x==8'b01101010)
  begin
    y=32'h48de5369;
    end
    else if(x==8'b01101011)
  begin
    y=32'h6413e680;
    end
    else if(x==8'b01101100)
  begin
    y=32'ha2ae0810;
    end
    else if(x==8'b01101101)
  begin
    y=32'hdd6db224;
    end
    else if(x==8'b01101110)
  begin
    y=32'h69852dfd;
    end
    else if(x==8'b01101111)
  begin
    y=32'h09072166;
    end
   else if (x==8'b01110000)
begin
  y=32'hb39a460a;
end
else if(x==8'b01110001)
begin
  y=32'h6445c0dd;
end
else if(x==8'b01110010)
  begin
    y=32'h586cdecf;
    end
else if(x==8'b01110011)
  begin
    y=32'h1c20c8ae;
    end
    else if(x==8'b01110100)
  begin
    y=32'h5bbef7dd;
    end
    else if(x==8'b01110101)
  begin
    y=32'h1b588d40;
    end
    else if(x==8'b01110110)
  begin
    y=32'hccd2017f;
    end
    else if(x==8'b01110111)
  begin
    y=32'h6bb4e3bb;
    end
    else if(x==8'b01111000)
  begin
    y=32'hdda26a7e;
    end
    else if(x==8'b01111001)
  begin
    y=32'h3a59ff45;
    end
    else if(x==8'b01111010)
  begin
    y=32'h3e350a44;
    end
    else if(x==8'b01111011)
  begin
    y=32'hbcb4cdd5;
    end
    else if(x==8'b01111100)
  begin
    y=32'h72eacea8;
    end
    else if(x==8'b01111101)
  begin
    y=32'hfa6484bb;
    end
    else if(x==8'b01111110)
  begin
    y=32'h8d6612ae;
    end
    else if(x==8'b01111111)
  begin
    y=32'hbf3c6f47;
    end 
else if (x==8'b10000000)
begin
  y=32'hd29be463;
end
else if(x==8'b10000001)
begin
  y=32'h542f5d9e;
end
else if(x==8'b10000010)
  begin
    y=32'haec2771b;
    end
else if(x==8'b10000011)
  begin
    y=32'hf64e6370;
    end
    else if(x==8'b10000100)
  begin
    y=32'h740e0d8d;
    end
    else if(x==8'b10000101)
  begin
    y=32'he75b1357;
    end
    else if(x==8'b10000110)
  begin
    y=32'hf8721671;
    end
    else if(x==8'b10000111)
  begin
    y=32'haf537d5d;
    end
    else if(x==8'b10001000)
  begin
    y=32'h4040cb08;
    end
    else if(x==8'b10001001)
  begin
    y=32'h4eb4e2cc;
    end
    else if(x==8'b10001010)
  begin
    y=32'h34d2466a;
    end
    else if(x==8'b10001011)
  begin
    y=32'h0115af84;
    end
    else if(x==8'b10001100)
  begin
    y=32'he1b00428;
    end
    else if(x==8'b10001101)
  begin
    y=32'h95983a1d;
    end
    else if(x==8'b10001110)
  begin
    y=32'h06b89fb4;
    end
    else if(x==8'b10001111)
  begin
    y=32'hce6ea048;
    end
    else if (x==8'b10010000)
begin
  y=32'h6f3f3b82;
end
else if(x==8'b10010001)
begin
  y=32'h3520ab82;
end
else if(x==8'b10010010)
  begin
    y=32'h011a1d4b;
    end
else if(x==8'b10010011)
  begin
    y=32'h277227f8;
    end
    else if(x==8'b10010100)
  begin
    y=32'h611560b1;
    end
    else if(x==8'b10010101)
  begin
    y=32'he7933fdc;
    end
    else if(x==8'b10010110)
  begin
    y=32'hbb3a792b;
    end
    else if(x==8'b10010111)
  begin
    y=32'h344525bd;
    end
    else if(x==8'b10011000)
  begin
    y=32'ha08839e1;
    end
    else if(x==8'b10011001)
  begin
    y=32'h51ce794b;
    end
    else if(x==8'b10011010)
  begin
    y=32'h2f32c9b7;
    end
    else if(x==8'b10011011)
  begin
    y=32'ha01fbac9;
    end
    else if(x==8'b10011100)
  begin
    y=32'he01cc87e;
    end
    else if(x==8'b10011101)
  begin
    y=32'hbcc7d1f6;
    end
    else if(x==8'b10011110)
  begin
    y=32'hcf0111c3;
    end
    else if(x==8'b10011111)
  begin
    y=32'ha1e8aac7;
    end
    else if (x==8'b10100000)
begin
  y=32'h1a908749;
end
else if(x==8'b10100001)
begin
  y=32'hd44fbd9a;
end
else if(x==8'b10100010)
  begin
    y=32'hd0dadecb;
    end
else if(x==8'b10100011)
  begin
    y=32'hd50ada38;
    end
    else if(x==8'b10100100)
  begin
    y=32'h0339c32a;
    end
    else if(x==8'b10100101)
  begin
    y=32'hc6913667;
    end
    else if(x==8'b10100110)
  begin
    y=32'h8df9317c;
    end
    else if(x==8'b10100111)
  begin
    y=32'he0b12b4f;
    end
    else if(x==8'b10101000)
  begin
    y=32'hf79e59b7;
    end
    else if(x==8'b10101001)
  begin
    y=32'h43f5bb3a;
    end
    else if(x==8'b10101010)
  begin
    y=32'hf2d519ff;
    end
    else if(x==8'b10101011)
  begin
    y=32'h27d9459c;
    end
    else if(x==8'b10101100)
  begin
    y=32'hbf97222c;
    end
    else if(x==8'b10101101)
  begin
    y=32'h15e6fc2a;
    end
    else if(x==8'b10101110)
  begin
    y=32'h0f91fc71;
    end
    else if(x==8'b10101111)
  begin
    y=32'h9b941525;
    end
   else if (x==8'b10110000)
begin
  y=32'hfae59361;
end
else if(x==8'b10110001)
begin
  y=32'hceb69ceb;
end
else if(x==8'b10110010)
  begin
    y=32'hc2a86459;
    end
else if(x==8'b10110011)
  begin
    y=32'h12baa8d1;
    end
    else if(x==8'b10110100)
  begin
    y=32'hb6c1075e;
    end
    else if(x==8'b10110101)
  begin
    y=32'he3056a0c;
    end
    else if(x==8'b10110110)
  begin
    y=32'h10d25065;
    end
    else if(x==8'b10110111)
  begin
    y=32'hcb03a442;
    end
    else if(x==8'b10111000)
  begin
    y=32'he0ec6e0e;
    end
    else if(x==8'b10111001)
  begin
    y=32'h1698db3b;
    end
    else if(x==8'b10111010)
  begin
    y=32'h4c98a0be;
    end
    else if(x==8'b10111011)
  begin
    y=32'h3278e964;
    end
    else if(x==8'b10111100)
  begin
    y=32'h9f1f9532;
    end
    else if(x==8'b10111101)
  begin
    y=32'he0d392df;
    end
    else if(x==8'b10111110)
  begin
    y=32'hd3a0342b;
    end
    else if(x==8'b10111111)
  begin
    y=32'h8971f21e;
    end
    else if (x==8'b11000000)
begin
  y=32'h1b0a7441;
end
else if(x==8'b11000001)
begin
  y=32'h4ba3348c;
end
else if(x==8'b11000010)
  begin
    y=32'hc5be7120;
    end
else if(x==8'b11000011)
  begin
    y=32'hc37632d8;
    end
    else if(x==8'b11000100)
  begin
    y=32'hdf359f8d;
    end
    else if(x==8'b11000101)
  begin
    y=32'h9b992f2e;
    end
    else if(x==8'b11000110)
  begin
    y=32'he60b6f47;
    end
    else if(x==8'b11000111)
  begin
    y=32'h0fe3f11d;
    end
    else if(x==8'b11001000)
  begin
    y=32'he54cda54;
    end
    else if(x==8'b11001001)
  begin
    y=32'h1edad891;
    end
    else if(x==8'b11001010)
  begin
    y=32'hce6279cf;
    end
    else if(x==8'b11001011)
  begin
    y=32'hcd3e7e6f;
    end
    else if(x==8'b11001100)
  begin
    y=32'h1618b166;
    end
    else if(x==8'b11001101)
  begin
    y=32'hfd2c1d05;
    end
    else if(x==8'b11001110)
  begin
    y=32'h848fd2c5;
    end
    else if(x==8'b11001111)
  begin
    y=32'hf6fb2299;
    end
   else if (x==8'b11010000)
begin
  y=32'hf523f357;
end
else if(x==8'b11010001)
begin
  y=32'ha6327623;
end
else if(x==8'b11010010)
  begin
    y=32'h93a83531;
    end
else if(x==8'b11010011)
  begin
    y=32'h56cccd02;
    end
    else if(x==8'b11010100)
  begin
    y=32'hacf08162;
    end
    else if(x==8'b11010101)
  begin
    y=32'h5a75ebb5;
    end
    else if(x==8'b11010110)
  begin
    y=32'h6e163697;
    end
    else if(x==8'b11010111)
  begin
    y=32'h88d273cc;
    end
    else if(x==8'b11011000)
  begin
    y=32'hde966292;
    end
    else if(x==8'b11011001)
  begin
    y=32'h81b949d0;
    end
    else if(x==8'b11011010)
  begin
    y=32'h4c50901b;
    end
    else if(x==8'b11011011)
  begin
    y=32'h71c65614;
    end
    else if(x==8'b11011100)
  begin
    y=32'he6c6c7bd;
    end
    else if(x==8'b11011101)
  begin
    y=32'h327a140a;
    end
    else if(x==8'b11011110)
  begin
    y=32'h45e1d006;
    end
    else if(x==8'b11011111)
  begin
    y=32'hc3f27b9a;
    end
    else if (x==8'b11100000)
begin
  y=32'hc9aa53fd;
end
else if(x==8'b11100001)
begin
  y=32'h62a80f00;
end
else if(x==8'b11100010)
  begin
    y=32'hbb25bfe2;
    end
else if(x==8'b11100011)
  begin
    y=32'h35bdd2f6;
    end
    else if(x==8'b11100100)
  begin
    y=32'h71126905;
    end
    else if(x==8'b11100101)
  begin
    y=32'hb2040222;
    end
    else if(x==8'b11100110)
  begin
    y=32'hb6cbcf7c;
    end
    else if(x==8'b11100111)
  begin
    y=32'hcd769c2b;
    end
    else if(x==8'b11101000)
  begin
    y=32'h53113ec0;
    end
    else if(x==8'b11101001)
  begin
    y=32'h1640e3d3;
    end
    else if(x==8'b11101010)
  begin
    y=32'h38abbd60;
    end
    else if(x==8'b11101011)
  begin
    y=32'h2547adf0;
    end
    else if(x==8'b11101100)
  begin
    y=32'hba38209c;
    end
    else if(x==8'b11101101)
  begin
    y=32'hf746ce76;
    end
    else if(x==8'b11101110)
  begin
    y=32'h77afa1c5;
    end
    else if(x==8'b11101111)
  begin
    y=32'h20756060;
    end
    else if (x==8'b11110000)
begin
  y=32'h85cbfe4e;
end
else if(x==8'b11110001)
begin
  y=32'h8ae88dd8;
end
else if(x==8'b11110010)
  begin
    y=32'h7aaaf9b0;
    end
else if(x==8'b11110011)
  begin
    y=32'h4cf9aa7e;
    end
    else if(x==8'b11110100)
  begin
    y=32'h1948c25c;
    end
    else if(x==8'b11110101)
  begin
    y=32'h02fb8a8c;
    end
    else if(x==8'b11110110)
  begin
    y=32'h01c36ae4;
    end
    else if(x==8'b11110111)
  begin
    y=32'hd6ebe1f9;
    end
    else if(x==8'b11111000)
  begin
    y=32'h90d4f869;
    end
    else if(x==8'b11111001)
  begin
    y=32'ha65cdea0;
    end
    else if(x==8'b11111010)
  begin
    y=32'h3f09252d;
    end
    else if(x==8'b11111011)
  begin
    y=32'hc208e69f;
    end
    else if(x==8'b11111100)
  begin
    y=32'hb74e6132;
    end
    else if(x==8'b11111101)
  begin
    y=32'hce77e25b;
    end
    else if(x==8'b11111110)
  begin
    y=32'h578fdfe3;
    end
    else if(x==8'b11111111)
  begin
    y=32'h3ac372e6;
    end
     end
endmodule


module bf_add_32(p,q,z);
  input[31:0]p,q;
  output[31:0]z;
  begin
  assign z=p+q;
  end
endmodule

module cipher65(r17,r18,ct);
input [31:0] r17,r18;
output [63:0]ct;
begin
  assign ct = {r18,r17};
end
endmodule

module bf_decryption(ct,k1,k2,k3,k4,k5,k6,k7,k8,k9,k10,k11,k12,k13,k14,k15,k16,k17,k18,pt);
  input [63:0]ct;
  input [31:0]k1,k2,k3,k4,k5,k6,k7,k8,k9,k10,k11,k12,k13,k14,k15,k16,k17,k18;
  wire [31:0]a1,a2,a3,a4,a5,a6,a7,a8,a9,a10,
             a11,a12,a13,a14,a15,a16,a17,a18,a19,a20,
             a21,a22,a23,a24,a25,a26,a27,a28,a29,a30,
             a31,a32,a33,a34,a35,a36,a37,a38,a39,a40,
             a41,a42,a43,a44,a45,a46,a47,a48,a49,a50,a51,a52;
  output [63:0] pt;
  

bf_bitsplit64 be1(ct,a1,a2);

bf_xor_1 be2(a2,k18,a3); 
bf_function_r1 be3(a3,a4);
bf_xor_1 be4(a1,a4,a5);

bf_xor_1 be5(a5,k17,a6); 
bf_function_r1 be6(a6,a7);
bf_xor_1 be7(a3,a7,a8);

bf_xor_1 be8(a8,k16,a9); 
bf_function_r1 be9(a9,a10);
bf_xor_1 be10(a6,a10,a11);

bf_xor_1 be11(a11,k15,a12); 
bf_function_r1 be12(a12,a13);
bf_xor_1 be13(a9,a13,a14);

bf_xor_1 be14(a14,k14,a15); 
bf_function_r1 be15(a15,a16);
bf_xor_1 be16(a12,a16,a17);

bf_xor_1 be17(a17,k13,a18); 
bf_function_r1 be18(a18,a19);
bf_xor_1 be19(a15,a19,a20);

bf_xor_1 be20(a20,k12,a21); 
bf_function_r1 be21(a21,a22);
bf_xor_1 be22(a18,a22,a23);

bf_xor_1 be23(a23,k11,a24); 
bf_function_r1 be24(a24,a25);
bf_xor_1 be25(a21,a25,a26);

bf_xor_1 be26(a26,k10,a27); 
bf_function_r1 be27(a27,a28);
bf_xor_1 be28(a24,a28,a29);

bf_xor_1 be29(a29,k9,a30); 
bf_function_r1 be30(a30,a31);
bf_xor_1 be31(a27,a31,a32);

bf_xor_1 be32(a32,k8,a33); 
bf_function_r1 be33(a33,a34);
bf_xor_1 be34(a30,a34,a35);

bf_xor_1 be35(a35,k7,a36); 
bf_function_r1 be36(a36,a37);
bf_xor_1 be37(a33,a37,a38);

bf_xor_1 be38(a38,k6,a39); 
bf_function_r1 be39(a39,a40);
bf_xor_1 be40(a36,a40,a41);

bf_xor_1 be41(a41,k5,a42); 
bf_function_r1 be42(a42,a43);
bf_xor_1 be43(a39,a43,a44);

bf_xor_1 be44(a44,k4,a45); 
bf_function_r1 be45(a45,a46);
bf_xor_1 be46(a42,a46,a47);

bf_xor_1 be47(a47,k3,a48); 
bf_function_r1 be48(a48,a49);
bf_xor_1 be49(a45,a49,a50);

bf_xor_1 be50(a50,k2,a51);
bf_xor_1 be51(a48,k1,a52);

plaintext64 be52(a51,a52,pt);

endmodule


module plaintext64(r17,r18,pt);
input [31:0] r17,r18;
output [63:0]pt;
begin
  assign pt = {r18,r17};
end
endmodule

module bf_fault(a,b,y);
  input a,b;
  output reg y;
  always@(a,b)
  begin
  if(a == b) 
   begin y = 0; end else begin y = 1;end
 end
 endmodule  
