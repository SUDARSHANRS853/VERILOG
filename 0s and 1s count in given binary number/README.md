```
module count(a,count_1,count_0);
  input [7:0]a;
  output reg [2:0]count_1,count_0;
  function [5:0]countt(input [7:0]in);
    int i=0;
    int count_1=0;
    int count_0=0;
    begin
      for(i=0;i<8;i=i+1)
        begin
          if(in[i]==1'b1)
            count_1=count_1+1;
          else 
            count_0=count_0+1;
        end
      countt[5:3]=count_1;
      countt[2:0]=count_0;
      countt={countt[5:3],countt[2:0]};
    end
  endfunction
  reg [5:0]cou;
  
  always@(*)begin
    cou=countt(a);
    count_1=cou[5:3];
    count_0=cou[2:0];
  end
endmodule
   ```
```
module test;
  reg [7:0]a;
  wire [2:0]count_1,count_0;
  count dut(a,count_1,count_0);
  initial begin
    a=8'd127;#2;
    
  end
  initial begin
    $monitor("simtime=%0t,a=%b,count_1=%d,count_0=%d",$time,a,count_1,count_0);
  end
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars(0,test);
  end
endmodule
```
Output
```
Compiler version U-2023.03-SP2_Full64; Runtime version U-2023.03-SP2_Full64;  Sep 16 06:36 2025
simtime=0,a=01111111,count_1=7,count_0=1
           V C S   S i m u l a t i o n   R e p o r t
```
    
      

    
