# Comparator16bit

The logical expression of a 16 bit unsigned comparator is given by:

```

A > B = A15'.B15 
        + A15.B15'.x1
        + A15.B15'.x2.x1
        + A15.B15'.x3.x2.x1
        + A15.B15'.x4.x3.x2.x1
        + A15.B15'.x5.x4.x3.x2.x1
        + A15.B15'.x6.x5.x4.x3.x2.x1
        + A15.B15'.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x8.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15.B15'.x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
  
A < B = A15.B15'
        + A15'.B15.x1
        + A15'.B15.x2.x1
        + A15'.B15.x3.x2.x1
        + A15'.B15.x4.x3.x2.x1
        + A15'.B15.x5.x4.x3.x2.x1
        + A15'.B15.x6.x5.x4.x3.x2.x1
        + A15'.B15.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x8.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1
        + A15'.B15.x15.x14.x13.x12.x11.x10.x9.x8.x7.x6.x5.x4.x3.x2.x1

(A = B) = x1.x2.x3.x4.x5.x6.x7.x8.x9.x10.x11.x12.x13.x14.x15

where 

xi = Ai.Bi + Ai.Bi' = Ai XOR Bi

```

Combining the above expression:

ans = 0.(A=B) + 1.(A>B) + 2.(A<B)

Verilog code:

```verilog

module signed_comparator(
    input [15:0] A,
    input [15:0] B,
    output [1:0] ans
);

wire [15:0] x;

xor16 x1(A[0], B[0], x[0]);
xor16 x2(A[1], B[1], x[1]);
xor16 x3(A[2], B[2], x[2]);
xor16 x4(A[3], B[3], x[3]);
xor16 x5(A[4], B[4], x[4]);
xor16 x6(A[5], B[5], x[5]);
xor16 x7(A[6], B[6], x[6]);
xor16 x8(A[7], B[7], x[7]);
xor16 x9(A[8], B[8], x[8]);
xor16 x10(A[9], B[9], x[9]);
xor16 x11(A[10], B[10], x[10]);
xor16 x12(A[11], B[11], x[11]);
xor16 x13(A[12], B[12], x[12]);
xor16 x14(A[13], B[13], x[13]);
xor16 x15(A[14], B[14], x[14]);
xor16 x16(A[15], B[15], x[15]);

wire [1:0] ans;

wire exp_less[15:0];
wire and_buffer[16:0];

and and0(and_buffer[0], A[15], B[15]);
and and1(and_buffer[1], x[15], and_buffer[0]);
and and2(and_buffer[2], x[14], and_buffer[1]);
and and3(and_buffer[3], x[13], and_buffer[2]);
and and4(and_buffer[4], x[12], and_buffer[3]);
and and5(and_buffer[5], x[11], and_buffer[4]);
and and6(and_buffer[6], x[10], and_buffer[5]);
and and7(and_buffer[7], x[9], and_buffer[6]);
and and8(and_buffer[8], x[8], and_buffer[7]);
and and9(and_buffer[9], x[7], and_buffer[8]);
and and10(and_buffer[10], x[6], and_buffer[9]);
and and11(and_buffer[11], x[5], and_buffer[10]);
and and12(and_buffer[12], x[4], and_buffer[11]);
and and13(and_buffer[13], x[3], and_buffer[12]);
and and14(and_buffer[14], x[2], and_buffer[13]);
and and15(and_buffer[15], x[1], and_buffer[14]);
and and16(and_buffer[16], x[0], and_buffer[15]);

wire or_buffer[16:0];

or or0(or_buffer[0], and_buffer[0], and_buffer[1]);
or or1(or_buffer[1], or_buffer[0], and_buffer[2]);
or or2(or_buffer[2], or_buffer[1], and_buffer[3]);
or or3(or_buffer[3], or_buffer[2], and_buffer[4]);
or or4(or_buffer[4], or_buffer[3], and_buffer[5]);
or or5(or_buffer[5], or_buffer[4], and_buffer[6]);
or or6(or_buffer[6], or_buffer[5], and_buffer[7]);
or or7(or_buffer[7], or_buffer[6], and_buffer[8]);
or or8(or_buffer[8], or_buffer[7], and_buffer[9]);
or or9(or_buffer[9], or_buffer[8], and_buffer[10]);
or or10(or_buffer[10], or_buffer[9], and_buffer[11]);
or or11(or_buffer[11], or_buffer[10], and_buffer[12]);
or or12(or_buffer[12], or_buffer[11], and_buffer[13]);
or or13(or_buffer[13], or_buffer[12], and_buffer[14]);
or or14(or_buffer[14], or_buffer[13], and_buffer[15]);
or or15(or_buffer[15], or_buffer[14], and_buffer[16]);

assign ans[0] = or_buffer[15];

and and17(and_buffer[17], A[15], B[15]);
and and18(and_buffer[18], x[0], and_buffer[17]);
and and19(and_buffer[19], x[1], and_buffer[18]);
and and20(and_buffer[20], x[2], and_buffer[19]);
and and21(and_buffer[21], x[3], and_buffer[20]);
and and22(and_buffer[22], x[4], and_buffer[21]);
and and23(and_buffer[23], x[5], and_buffer[22]);
and and24(and_buffer[24], x[6], and_buffer[23]);
and and25(and_buffer[25], x[7], and_buffer[24]);
and and26(and_buffer[26], x[8], and_buffer[25]);
and and27(and_buffer[27], x[9], and_buffer[26]);
and and28(and_buffer[28], x[10], and_buffer[27]);
and and29(and_buffer[29], x[11], and_buffer[28]);
and and30(and_buffer[30], x[12], and_buffer[29]);
and and31(and_buffer[31], x[13], and_buffer[30]);
and and32(and_buffer[32], x[14], and_buffer[31]);
and and33(and_buffer[33], x[15], and_buffer[32]);

or or16(or_buffer[16], and_buffer[16], and_buffer[17]);
or or17(or_buffer[17], or_buffer[16], and_buffer[18]);
or or18(or_buffer[18], or_buffer[17], and_buffer[19]);
or or19(or_buffer[19], or_buffer[18], and_buffer[20]);
or or20(or_buffer[20], or_buffer[19], and_buffer[21]);
or or21(or_buffer[21], or_buffer[20], and_buffer[22]);
or or22(or_buffer[22], or_buffer[21], and_buffer[23]);
or or23(or_buffer[23], or_buffer[22], and_buffer[24]);
or or24(or_buffer[24], or_buffer[23], and_buffer[25]);
or or25(or_buffer[25], or_buffer[24], and_buffer[26]);
or or26(or_buffer[26], or_buffer[25], and_buffer[27]);
or or27(or_buffer[27], or_buffer[26], and_buffer[28]);
or or28(or_buffer[28], or_buffer[27], and_buffer[29]);
or or29(or_buffer[29], or_buffer[28], and_buffer[30]);
or or30(or_buffer[30], or_buffer[29], and_buffer[31]);
or or31(or_buffer[31], or_buffer[30], and_buffer[32]);
or or32(or_buffer[32], or_buffer[31], and_buffer[33]);

assign ans[1] = or_buffer[32];

endmodule

```

