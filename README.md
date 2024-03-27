# Comparator16bit

The logical expression of a 16 bit unsigned comparator is given by:

```

A > B =   A[15]'.[B15]
        + A[14].B[14]' .x15
        + A[13].B[13]' .x15.x14
        + A[13].B[13]' .x15.x14.x13
        + A[11].B[11]' .x15.x14.x13.x12
        + A[10].B[10]' .x15.x14.x13.x12.x11
        + A[ 9].B[ 9]' .x15.x14.x13.x12.x11.x10
        + A[ 8].B[ 8]' .x15.x14.x13.x12.x11.x10.x9
        + A[ 7].B[ 7]' .x15.x14.x13.x12.x11.x10.x9.x8
        + A[ 6].B[ 6]' .x15.x14.x13.x12.x11.x10.x9.x8.x7
        + A[ 5].B[ 5]' .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6
        + A[ 4].B[ 4]' .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5
        + A[ 3].B[ 3]' .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4
        + A[ 2].B[ 2]' .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3
        + A[ 1].B[ 1]' .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2
        + A[ 0].B[ 0]' .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
  
A < B =   A[15].[B15]' 
        + A[14]'.B[14] .x15
        + A[13]'.B[13] .x15.x14
        + A[13]'.B[13] .x15.x14.x13
        + A[11]'.B[11] .x15.x14.x13.x12
        + A[10]'.B[10] .x15.x14.x13.x12.x11
        + A[ 9]'.B[ 9] .x15.x14.x13.x12.x11.x10
        + A[ 8]'.B[ 8] .x15.x14.x13.x12.x11.x10.x9
        + A[ 7]'.B[ 7] .x15.x14.x13.x12.x11.x10.x9.x8
        + A[ 6]'.B[ 6] .x15.x14.x13.x12.x11.x10.x9.x8.x7
        + A[ 5]'.B[ 5] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6
        + A[ 4]'.B[ 4] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5
        + A[ 3]'.B[ 3] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4
        + A[ 2]'.B[ 2] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3
        + A[ 1]'.B[ 1] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2
        + A[ 0]'.B[ 0] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1

(A = B) = x1.x2.x3.x4.x5.x6.x7.x8.x9.x10.x11.x12.x13.x14.x15

where 

xi = Ai.Bi + Ai'.Bi' = Ai XNOR Bi

```

Combining the above expression:

ans = 0.(A=B) + 1.(A>B) + 2.(A<B)

Verilog code:

```verilog



module signed_comparator(
    input [15:0] A,
    input [15:0] B,
    output reg g ,
    output reg l ,
    output reg e
);

wire [15:0] x;


// Wires for all the xor gates:
xor  x1(A[ 0], B[ 0], x[ 0]); // x1
xor  x2(A[ 1], B[ 1], x[ 1]); // x2
xor  x3(A[ 2], B[ 2], x[ 2]); // x3
xor  x4(A[ 3], B[ 3], x[ 3]); // x4
xor  x5(A[ 4], B[ 4], x[ 4]); // x5
xor  x6(A[ 5], B[ 5], x[ 5]); // x6
xor  x7(A[ 6], B[ 6], x[ 6]); // x7
xor  x8(A[ 7], B[ 7], x[ 7]); // x8
xor  x9(A[ 8], B[ 8], x[ 8]); // x9
xor x10(A[ 9], B[ 9], x[ 9]); // x10
xor x11(A[10], B[10], x[10]); // x11
xor x12(A[11], B[11], x[11]); // x12
xor x13(A[12], B[12], x[12]); // x13
xor x14(A[13], B[13], x[13]); // x14
xor x15(A[14], B[14], x[14]); // x15

wire exp_less[15:0];
wire and_buffer[15:0];

// wires to store each statement to be used in the A > B statement :

wire notB[15:0];
not  notB0( notB[ 0], B[ 0] ); // notB0
not  notB1( notB[ 1], B[ 1] ); // notB1
not  notB2( notB[ 2], B[ 2] ); // notB2 
not  notB3( notB[ 3], B[ 3] ); // notB3
not  notB4( notB[ 4], B[ 4] ); // notB4
not  notB5( notB[ 5], B[ 5] ); // notB5
not  notB6( notB[ 6], B[ 6] ); // notB6
not  notB7( notB[ 7], B[ 7] ); // notB7
not  notB8( notB[ 8], B[ 8] ); // notB8
not  notB9( notB[ 9], B[ 9] ); // notB9
not notB10( notB[10], B[10] ); // notB10
not notB11( notB[11], B[11] ); // notB11
not notB12( notB[12], B[12] ); // notB12
not notB13( notB[13], B[13] ); // notB13
not notB14( notB[14], B[14] ); // notB14
not notB15( notB[15], B[15] ); // notB15

wire notA[15:0];
not  notA0 ( notA[ 0] , A[ 0] ); // notA0
not  notA1 ( notA[ 1] , A[ 1] ); // notA1
not  notA2 ( notA[ 2] , A[ 2] ); // notA2
not  notA3 ( notA[ 3] , A[ 3] ); // notA3
not  notA4 ( notA[ 4] , A[ 4] ); // notA4
not  notA5 ( notA[ 5] , A[ 5] ); // notA5
not  notA6 ( notA[ 6] , A[ 6] ); // notA6
not  notA7 ( notA[ 7] , A[ 7] ); // notA7
not  notA8 ( notA[ 8] , A[ 8] ); // notA8
not  notA9 ( notA[ 9] , A[ 9] ); // notA9
not notA10 ( notA[10] , A[10] ); // notA10
not notA11 ( notA[11] , A[11] ); // notA11
not notA12 ( notA[12] , A[12] ); // notA12
not notA13 ( notA[13] , A[13] ); // notA13
not notA14 ( notA[14] , A[14] ); // notA14
not notA15 ( notA[15] , A[15] ); // notA15



and and1 (and_buffer[ 1], x[15], 1);              // x15
and and2 (and_buffer[ 2], x[14], and_buffer[ 1]); // x15.x14
and and3 (and_buffer[ 3], x[13], and_buffer[ 2]); // x15.x14.x13
and and4 (and_buffer[ 4], x[12], and_buffer[ 3]); // x15.x14.x13.x12
and and5 (and_buffer[ 5], x[11], and_buffer[ 4]); // x15.x14.x13.x12.x11
and and6 (and_buffer[ 6], x[10], and_buffer[ 5]); // x15.x14.x13.x12.x11.x10
and and7 (and_buffer[ 7], x[ 9], and_buffer[ 6]); // .
and and8 (and_buffer[ 8], x[ 8], and_buffer[ 7]); // .
and and9 (and_buffer[ 9], x[ 7], and_buffer[ 8]); // .
and and10(and_buffer[10], x[ 6], and_buffer[ 9]);
and and11(and_buffer[11], x[ 5], and_buffer[10]);
and and12(and_buffer[12], x[ 4], and_buffer[11]);
and and13(and_buffer[13], x[ 3], and_buffer[12]); // .
and and14(and_buffer[14], x[ 2], and_buffer[13]); // . 
and and15(and_buffer[15], x[ 1], and_buffer[14]); // .
and and16(and_buffer[16], x[ 0], and_buffer[15]); // x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1

wire or_buffer1[16:0];
wire buffer1[16:0];
wire exp1[16:0];

// All the statments are being or'd : 
and and0(exp1[0], A[15], notB[15]); // A[15].B[15]'

and and17(buffer1[0], notA[14], B[14]); // A[14]'.B[14]
and and18(exp1[1], x[15], buffer1[0]); // A[14]'.B[14] .x15

and and19(buffer1[1], notA[13], B[13]); // A[13]'.B[13]
and and20(exp1[2], and_buffer[2], buffer1[1]); // A[13]'.B[13] .x15.x14

and and21(buffer1[2], notA[12], B[12]); // A[13]'.B[13]
and and22(exp1[3], and_buffer[3], buffer1[2]); // A[13]'.B[13] .x15.x14.x13

and and23(buffer1[3], notA[11], B[11]); // A[11]'.B[11]
and and24(exp1[4], and_buffer[4], buffer1[3]); // A[11]'.B[11] .x15.x14.x13.x12

and and25(buffer1[4], notA[10], B[10]); // A[10]'.B[10]
and and26(exp1[5], and_buffer[5], buffer1[4]); // A[10]'.B[10] .x15.x14.x13.x12.x11

and and27(buffer1[5], notA[9], B[9]); // A[9]'.B[9]
and and28(exp1[6], and_buffer[6], buffer1[5]); // A[9]'.B[9] .x15.x14.x13.x12.x11.x10

and and29(buffer1[6], notA[8], B[8]); // A[8]'.B[8]
and and30(exp1[7], and_buffer[7], buffer1[6]); // A[8]'.B[8] .x15.x14.x13.x12.x11.x10.x9

and and31(buffer1[7], notA[7], B[7]); // A[7]'.B[7]
and and32(exp1[8], and_buffer[8], buffer1[7]); // A[7]'.B[7] .x15.x14.x13.x12.x11.x10.x9.x8

and and33(buffer1[8], notA[6], B[6]); // A[6]'.B[6]
and and34(exp1[9], and_buffer[9], buffer1[8]); // A[6]'.B[6] .x15.x14.x13.x12.x11.x10.x9.x8.x7

and and35(buffer1[9], notA[5], B[5]); // A[5]'.B[5]
and and36(exp1[10], and_buffer[10], buffer1[9]); // A[5]'.B[5] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6

and and37(buffer1[10], notA[4], B[4]); // A[4]'.B[4]
and and38(exp1[11], and_buffer[11], buffer1[10]); // A[4]'.B[4] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5

and and39(buffer1[11], notA[3], B[3]); // A[3]'.B[3]
and and40(exp1[12], and_buffer[12], buffer1[11]); // A[3]'.B[3] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4

and and41(buffer1[12], notA[2], B[2]); // A[2]'.B[2]
and and42(exp1[13], and_buffer[13], buffer1[12]); // A[2]'.B[2] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3

and and43(buffer1[13], notA[1], B[1]); // A[1]'.B[1]
and and44(exp1[14], and_buffer[14], buffer1[13]); // A[1]'.B[1] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2

and and45(buffer1[14], notA[0], B[0]); // A[ 0]'.B[ 0]
and and46(exp1[15], and_buffer[15], buffer1[14]); // A[ 0]'.B[ 0] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1

or or_buffer1_16(or_buffer1[16], exp1[15], 1'b0); // exp1[15]
or or_buffer1_15(or_buffer1[15], exp1[14], or_buffer1[16]); // exp1[15] + exp1[14]
or or_buffer1_14(or_buffer1[14], exp1[13], or_buffer1[15]); // exp1[15] + exp1[14] + exp1[13]
or or_buffer1_13(or_buffer1[13], exp1[12], or_buffer1[14]); // exp1[15] + exp1[14] + exp1[13] + exp1[12]
or or_buffer1_12(or_buffer1[12], exp1[11], or_buffer1[13]);
or or_buffer1_11(or_buffer1[11], exp1[10], or_buffer1[12]);
or or_buffer1_10(or_buffer1[10], exp1[ 9], or_buffer1[11]);
or or_buffer1_9(or_buffer1[ 9], exp1[ 8], or_buffer1[10]);
or or_buffer1_8(or_buffer1[ 8], exp1[ 7], or_buffer1[ 9]);
or or_buffer1_7(or_buffer1[ 7], exp1[ 6], or_buffer1[ 8]);
or or_buffer1_6(or_buffer1[ 6], exp1[ 5], or_buffer1[ 7]);
or or_buffer1_5(or_buffer1[ 5], exp1[ 4], or_buffer1[ 6]);
or or_buffer1_4(or_buffer1[ 4], exp1[ 3], or_buffer1[ 5]);
or or_buffer1_3(or_buffer1[ 3], exp1[ 2], or_buffer1[ 4]);
or or_buffer1_2(or_buffer1[ 2], exp1[ 1], or_buffer1[ 3]);
or or_buffer1_1(or_buffer1[ 1], exp1[ 0], or_buffer1[ 2]); // 


wire or_buffer2[16:0];
wire buffer2[16:0];
wire exp2[16:0];

// All the statments are being or'd :

and and0(exp2[0], notA[15], B[15]); // A[15]'.B[15]

and andd17(buffer2[0], A[14], notB[14]); // A[14].B[14]'
and andd18(exp2[1], x[15], buffer2[0]); // A[14].B[14] .x15

and andd19(buffer2[1], A[13], notB[13]); // A[13].B[13]'
and andd20(exp2[2], and_buffer[2], buffer2[1]); // A[13].B[13] .x15.x14

and andd21(buffer2[2], A[12], notB[12]); // A[13].B[13]'
and andd22(exp2[3], and_buffer[3], buffer2[2]); // A[13].B[13] .x15.x14.x13

and andd23(buffer2[3], A[11], notB[11]); // A[11].B[11]'
and andd24(exp2[4], and_buffer[4], buffer2[3]); // A[11].B[11] .x15.x14.x13.x12

and andd25(buffer2[4], A[10], notB[10]); // A[10].B[10]'
and andd26(exp2[5], and_buffer[5], buffer2[4]); // A[10].B[10] .x15.x14.x13.x12.x11

and andd27(buffer2[5], A[ 9], notB[ 9]); // A[ 9].B[ 9]'
and andd28(exp2[6], and_buffer[6], buffer2[5]); // A[ 9].B[ 9] .x15.x14.x13.x12.x11.x10

and andd29(buffer2[6], A[ 8], notB[ 8]); // A[ 8].B[ 8]'
and andd30(exp2[7], and_buffer[7], buffer2[6]); // A[ 8].B[ 8] .x15.x14.x13.x12.x11.x10.x9

and andd31(buffer2[7], A[ 7], notB[ 7]); // A[ 7].B[ 7]'
and andd32(exp2[8], and_buffer[8], buffer2[7]); // A[ 7].B[ 7] .x15.x14.x13.x12.x11.x10.x9.x8

and andd33(buffer2[8], A[ 6], notB[ 6]); // A[ 6].B[ 6]'
and andd34(exp2[9], and_buffer[9], buffer2[8]); // A[ 6].B[ 6] .x15.x14.x13.x12.x11.x10.x9.x8.x7

and andd35(buffer2[9], A[ 5], notB[ 5]); // A[ 5].B[ 5]'
and andd36(exp2[10], and_buffer[10], buffer2[9]); // A[ 5].B[ 5] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6

and andd37(buffer2[10], A[ 4], notB[ 4]); // A[ 4].B[ 4]'
and andd38(exp2[11], and_buffer[11], buffer2[10]); // A[ 4].B[ 4] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5

and andd39(buffer2[11], A[ 3], notB[ 3]); // A[ 3].B[ 3]'
and andd40(exp2[12], and_buffer[12], buffer2[11]); // A[ 3].B[ 3] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4

and andd41(buffer2[12], A[ 2], notB[ 2]); // A[ 2].B[ 2]'
and andd42(exp2[13], and_buffer[13], buffer2[12]); // A[ 2].B[ 2] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3

and andd43(buffer2[13], A[ 1], notB[ 1]); // A[ 1].B[ 1]'
and andd44(exp2[14], and_buffer[14], buffer2[13]); // A[ 1].B[ 1] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2

and andd45(buffer2[14], A[ 0], notB[ 0]); // A[ 0].B[ 0]'
and andd46(exp2[15], and_buffer[15], buffer2[14]); // A[ 0].B[ 0] .x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1

or or_buffer2_16(or_buffer2[16], exp2[15], 1'b0); // exp2[15]
or or_buffer2_15(or_buffer2[15], exp2[14], or_buffer2[16]); // exp2[15] + exp2[14]
or or_buffer2_14(or_buffer2[14], exp2[13], or_buffer2[15]); // exp2[15] + exp2[14] + exp2[13]
or or_buffer2_13(or_buffer2[13], exp2[12], or_buffer2[14]); // exp2[15] + exp2[14] + exp2[13] + exp2[12]
or or_buffer2_12(or_buffer2[12], exp2[11], or_buffer2[13]);
or or_buffer2_11(or_buffer2[11], exp2[10], or_buffer2[12]);
or or_buffer2_10(or_buffer2[10], exp2[ 9], or_buffer2[11]);
or or_buffer2_9(or_buffer2[ 9], exp2[ 8], or_buffer2[10]);
or or_buffer2_8(or_buffer2[ 8], exp2[ 7], or_buffer2[ 9]);
or or_buffer2_7(or_buffer2[ 7], exp2[ 6], or_buffer2[ 8]);
or or_buffer2_6(or_buffer2[ 6], exp2[ 5], or_buffer2[ 7]);
or or_buffer2_5(or_buffer2[ 5], exp2[ 4], or_buffer2[ 6]);
or or_buffer2_4(or_buffer2[ 4], exp2[ 3], or_buffer2[ 5]);
or or_buffer2_3(or_buffer2[ 3], exp2[ 2], or_buffer2[ 4]);
or or_buffer2_2(or_buffer2[ 2], exp2[ 1], or_buffer2[ 3]);
or or_buffer2_1(or_buffer2[ 1], exp2[ 0], or_buffer2[ 2]);

assign l = or_buffer1[1];

assign g = or_buffer2[1];

assign e = and_buffer[16];

endmodule




```






