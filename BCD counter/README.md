```
module bcd(clk,rst,c);
  input clk,rst;
  output reg [3:0]c;
  reg [3:0]temp;
  always@(posedge clk or posedge rst)
  begin
    if(rst)begin
      temp<=4'b0000;
    end
    else begin
      if(clk)begin
        temp<=temp+1;
        if(temp==4'd9)
          begin
            temp<=4'b0000;
          end
      end
    end
    c=temp;
    end
endmodule
```
```
module test;
  reg clk,rst;
  wire [3:0]c;
  bcd dut(clk,rst,c);
  initial begin
    clk=0;
    rst=1;
    #10 rst=0;
    
    
    #200 $finish;
  end
  always #5clk=~clk; 
  initial begin
    $monitor("simtime=%0t,rst=%b,c=%d",$time,rst,c);
  end
  initial begin
    $dumpfile("dumpfile.vcd");
    $dumpvars(0,test);
  end
endmodule
```
Output
```
Compiler version U-2023.03-SP2_Full64; Runtime version U-2023.03-SP2_Full64;  Sep 14 06:38 2025
simtime=0,rst=1,c= x
simtime=5,rst=1,c= 0
simtime=10,rst=0,c= 0
simtime=25,rst=0,c= 1
simtime=35,rst=0,c= 2
simtime=45,rst=0,c= 3
simtime=55,rst=0,c= 4
simtime=65,rst=0,c= 5
simtime=75,rst=0,c= 6
simtime=85,rst=0,c= 7
simtime=95,rst=0,c= 8
simtime=105,rst=0,c= 9
simtime=115,rst=0,c= 0
simtime=125,rst=0,c= 1
simtime=135,rst=0,c= 2
simtime=145,rst=0,c= 3
simtime=155,rst=0,c= 4
simtime=165,rst=0,c= 5
simtime=175,rst=0,c= 6
simtime=185,rst=0,c= 7
simtime=195,rst=0,c= 8
simtime=205,rst=0,c= 9
$finish called from file "testbench.sv", line 11.
$finish at simulation time                  210
           V C S   S i m u l a t i o n   R e p o r
```
