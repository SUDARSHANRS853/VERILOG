1.
```
module ha(input a,b,
          output s,c);
  function ha_sum(input x,y);
    begin
     ha_sum=x^y;
    end
  endfunction
  function ha_carry(input x,y);
    begin
      ha_carry=x&y;
    end
  endfunction
  assign s=ha_sum(a,b);
  assign c=ha_carry(a,b);
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
2
```
module ha(input a,b,
          output s,c);
  wire [1:0]temp;
  function [1:0]ha1(input x,y);
    begin
      ha1[0]=x^y;
      ha1[1]=x&y;
    end
  endfunction
  assign temp=ha1(a,b);
  assign s=temp[0];
  assign c=temp[1];
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
