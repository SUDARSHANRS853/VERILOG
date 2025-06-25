## SERIAL IN SERIAL OUT
```
module siso(clk, rst, si, so);
  input clk, rst, si;
  output reg so;
  reg [3:0] shift_reg;

  always @(posedge clk or posedge rst) begin
    if (rst)
      shift_reg <= 4'b0000;
    else
      shift_reg <= {shift_reg[2:0], si};
      
    so <= shift_reg[3];
  end
endmodule
module test;
  reg clk, rst, si;
  wire so;

  siso dut(clk, rst, si, so);

  initial begin
    clk = 0;
    forever #5 clk = ~clk;
  end

  initial begin
    rst = 1;
    #12 rst = 0;
   si=1;#2
     si=1;#2
     si=1;#2
    si=1;#2
    si=0;#2
     si=1;#2
     si=1;#2
    si=1;
  #10000 $finish;
  end

  initial begin
    $monitor("time=%0t | si=%b  | rst=%b | so=%b", $time, si, rst, so);
    
  end
endmodule
```
Output
```
time=0 | si=x  | rst=1 | so=x
time=5 | si=x  | rst=1 | so=0
time=12 | si=1  | rst=0 | so=0
time=20 | si=0  | rst=0 | so=0
time=22 | si=1  | rst=0 | so=0
time=55 | si=1  | rst=0 | so=1
```
## SERIAL IN PARALLEL OUT
```
module sipo(clk,rst,si,po);
  input clk,rst,si;
  output reg [3:0]po;
  always@(posedge clk or posedge rst)begin
    if(rst)
      po<=4'b0000;
    else 
      po={po[2:0],si};
  end
endmodule
module test;
  reg clk, rst, si;
  wire [3:0] po;

  sipo dut(clk, rst, si, po);

  // Clock generation: 10 time units period
  initial begin
    clk = 0;
    forever #5 clk = ~clk;
  end

  // Stimulus
  initial begin
    rst = 1;
    si = 0;
    #12 rst = 0;

    // Apply bits at every rising clock edge
    si = 1; #10;
    si = 1; #10;
    si = 1; #10;
    si = 1; #10
   

    $finish;
  end

  // Monitor outputs
  initial begin
    $monitor("time=%0t | rst=%b | si=%b | po=%b", $time, rst, si, po);
  end
endmodule
```
Output
```
time=0 | rst=1 | si=0 | po=0000
time=12 | rst=0 | si=1 | po=0000
time=15 | rst=0 | si=1 | po=0001
time=25 | rst=0 | si=1 | po=0011
time=35 | rst=0 | si=1 | po=0111
time=45 | rst=0 | si=1 | po=1111
```
## PARALLEL IN PARALLEL OUT
```
module pipo(clk,rst,pi,po);
  input clk,rst;
  input [3:0]pi;
  output reg[3:0]po;
  always@(posedge clk or posedge rst)begin
    if(rst)
      po<=4'b0000;
    else
      po<=pi;
  end
endmodule
module test;
  reg clk,rst;
  reg [3:0]pi;
  wire [3:0]po;
  pipo dut(clk,rst,pi,po);
  initial begin
    clk=0;
    forever #5 clk=~clk;
  end
  initial begin
    rst=1;
    pi=4'b0000;
    #12 rst=0;
    pi=4'b0001;
    #10 pi=4'b0010;
    #10 pi=4'b0010;
    #10 pi=4'b0010;
    #10 $finish;
  end
initial begin
  $monitor("simtime=%0t|rst=%b|pi=%b|po=%b",$time,rst,pi,po);
end
endmodule
```
Output
```
simtime=0|rst=1|pi=0000|po=0000
simtime=12|rst=0|pi=0001|po=0000
simtime=15|rst=0|pi=0001|po=0001
simtime=22|rst=0|pi=0010|po=0001
simtime=25|rst=0|pi=0010|po=0010
```
## PARALLEL IN SERIAL OUT
```
// //   module siso(clk, rst, si, so);
// //   input clk, rst, si;
// //   output reg so;
// //   reg [3:0] shift_reg;

// //   always @(posedge clk or posedge rst) begin
// //     if (rst)
// //       shift_reg <= 4'b0000;
// //     else
// //       shift_reg <= {shift_reg[2:0], si};
      
// //     so <= shift_reg[3];
// //   end
// // endmodule
// // module test;
// //   reg clk, rst, si;
// //   wire so;

// //   siso dut(clk, rst, si, so);

// //   initial begin
// //     clk = 0;
// //     forever #5 clk = ~clk;
// //   end

// //   initial begin
// //     rst = 1;
// //     #12 rst = 0;
// //    si=1;#2
// //      si=1;#2
// //      si=1;#2
// //     si=1;#2
// //     si=0;#2
// //      si=1;#2
// //      si=1;#2
// //     si=1;
// //   #10000 $finish;
// //   end

// //   initial begin
// //     $monitor("time=%0t | si=%b  | rst=%b | so=%b", $time, si, rst, so);
    
// //   end
// // endmodule

// module sipo(clk,rst,si,po);
//   input clk,rst,si;
//   output reg [3:0]po;
//   always@(posedge clk or posedge rst)begin
//     if(rst)
//       po<=4'b0000;
//     else 
//       po={po[2:0],si};
//   end
// endmodule
// // module test;
// //   reg clk,rst,si;
// //   wire [3:0]po;
// //   sipo dut(clk,rst,si,po);
// //   initial begin
// //     clk=0;
// //     forever #5 clk=~clk;
// //   end
// //   initial begin
// //     rst=1;
// //     si=0;
// //     #12 rst=0;
// //     si=1;
// //     #2 si=1;
// //     #2 si=0;
// //     #2 si=0;
// //     #2 si=1;
// //     #2 si=1;
// //     #2 $finish;
// //   end
// //   initial begin
// //     $monitor("simtime=%0t|rst=%b|si=%b|po=%b",$time,rst,si,po);
// //   end
// // endmodule
    
//   module test;
//   reg clk, rst, si;
//   wire [3:0] po;

//   sipo dut(clk, rst, si, po);

//   // Clock generation: 10 time units period
//   initial begin
//     clk = 0;
//     forever #5 clk = ~clk;
//   end

//   // Stimulus
//   initial begin
//     rst = 1;
//     si = 0;
//     #12 rst = 0;

//     // Apply bits at every rising clock edge
//     si = 1; #10;
//     si = 1; #10;
//     si = 1; #10;
//     si = 1; #10
   

//     $finish;
//   end

//   // Monitor outputs
//   initial begin
//     $monitor("time=%0t | rst=%b | si=%b | po=%b", $time, rst, si, po);
//   end
// endmodule

// module pipo(clk,rst,pi,po);
//   input clk,rst;
//   input [3:0]pi;
//   output reg[3:0]po;
//   always@(posedge clk or posedge rst)begin
//     if(rst)
//       po<=4'b0000;
//     else
//       po<=pi;
//   end
// endmodule
// module test;
//   reg clk,rst;
//   reg [3:0]pi;
//   wire [3:0]po;
//   pipo dut(clk,rst,pi,po);
//   initial begin
//     clk=0;
//     forever #5 clk=~clk;
//   end
//   initial begin
//     rst=1;
//     pi=4'b0000;
//     #12 rst=0;
//     pi=4'b0001;
//     #10 pi=4'b0010;
//     #10 pi=4'b0010;
//     #10 pi=4'b0010;
//     #10 $finish;
//   end
// initial begin
//   $monitor("simtime=%0t|rst=%b|pi=%b|po=%b",$time,rst,pi,po);
// end
// endmodule
    
    
module piso(clk, rst, control, pi, so);
  input clk, rst;
  input control;
  input [3:0] pi;
  output reg so;

  reg [3:0] shift_reg;

  always @(posedge clk or posedge rst) begin
    if (rst) begin
      shift_reg <= 4'b0000;
      so <= 0;
    end else begin
      if (control)
        shift_reg <= pi; // Load parallel input
      else begin
        so <= shift_reg[3]; // Output MSB
        shift_reg <= {shift_reg[2:0], 1'b0}; // Shift left
      end
    end
  end
endmodule

module test;
  reg clk, rst, control;
  reg [3:0] pi;
  wire so;

  piso dut(clk, rst, control, pi, so);

  initial begin
    clk = 0;
    forever #5 clk = ~clk;
  end

  initial begin
    rst = 1;
    control = 0;
    pi = 4'b0000;
    #12 rst = 0;

    // Load data into PISO
    control = 1; pi = 4'b1010; #10;

    // Start shifting
    control = 0;
    repeat(4) #10;

    // Load new data
    control = 1; pi = 4'b1100; #10;

    control = 0;
    repeat(4) #10;

    $finish;
  end

  initial begin
    $monitor("time=%0t | rst=%b | control=%b | pi=%b | so=%b", 
              $time,  rst, control, pi, so);
  end
endmodule
```
Output
```
time=0 | rst=1 | control=0 | pi=0000 | so=0
time=12 | rst=0 | control=1 | pi=1010 | so=0
time=22 | rst=0 | control=0 | pi=1010 | so=0
time=25 | rst=0 | control=0 | pi=1010 | so=1
time=35 | rst=0 | control=0 | pi=1010 | so=0
time=45 | rst=0 | control=0 | pi=1010 | so=1
time=55 | rst=0 | control=0 | pi=1010 | so=0
time=62 | rst=0 | control=1 | pi=1100 | so=0
time=72 | rst=0 | control=0 | pi=1100 | so=0
time=75 | rst=0 | control=0 | pi=1100 | so=1
time=95 | rst=0 | control=0 | pi=1100 | so=0
```
