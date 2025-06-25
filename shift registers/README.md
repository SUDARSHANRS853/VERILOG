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
