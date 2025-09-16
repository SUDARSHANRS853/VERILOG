```
module multiplication_using_addition(a,b,out);
  input [3:0]a,b;
  output reg [7:0]out;
  function [7:0] da(input [3:0]x,input [3:0]y);
    begin
    int z=0;
    int temp=0;
      int i;
      for(z=0;z<y;z=z+1)begin
        temp+=x;
      end
//     while(z<y)begin
//       temp=temp+x;
//       z=z+1;
//     end
    da=temp;
    end
  endfunction
  always@(*)begin
    out=da(a,b);
    
  end
    
endmodule
```
```
module test;
  reg [3:0]a,b;
  wire [7:0]out;
  multiplication_using_addition dut(a,b,out);
  initial begin
    a=4'b1000;b=4'b1010;#10;
     a=4'b1100;b=4'b1010;#10;
     
    
   #30 $finish;
  end
  initial begin
    $monitor("simtime=%0t,a=%d,b=%d,out=%d",$time,a,b,out);
  end
endmodule
```
Output
```
Compiler version U-2023.03-SP2_Full64; Runtime version U-2023.03-SP2_Full64;  Sep 16 06:39 2025
simtime=0,a= 8,b=10,out= 80
$finish called from file "testbench.sv", line 10.
$finish at simulation time                   40
```
