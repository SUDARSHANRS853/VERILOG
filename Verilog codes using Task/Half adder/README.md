```
module ha(input a,b,
          output reg s,c);
  task ha_1;
    input x,y;
    output sum,carry;
    begin
      sum=x^y;
      carry=x&y;
    end
  endtask
  always@(*)
    ha_1(a,b,s,c);
      
endmodule
```
Output
```
module test;
  reg a,b;
  wire s,c;
  ha dut(a,b,s,c);
  initial begin
    a=1'b0;b=1'b0;#10;
    a=1'b0;b=1'b1;#10;
    a=1'b1;b=1'b0;#10;
    a=1'b1;b=1'b1;
  end
  initial begin
    $monitor("simtime=%0t,a=%b,b=%b,s=%b,c=%b",$time,a,b,s,c);
  end
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars(0,test);
  end
endmodule
```
Output
```
simtime=0,a=0,b=0,s=0,c=0
simtime=10,a=0,b=1,s=1,c=0
simtime=20,a=1,b=0,s=1,c=0
simtime=30,a=1,b=1,s=0,c=1
           V C S   S i m u l a t i o n   R e p o r t
```
