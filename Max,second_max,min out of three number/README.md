```
module max1(a,b,c,max,second_max,min);
  input [3:0]a,b,c;
  output reg [3:0]max,second_max,min;
  always@(a,b,c)begin
    if(a>b && a>c)begin
      if(b>c)begin
        max=a;
        second_max=b;
        min=c;
      end
      else begin
        max=a;
        second_max=c;
        min=b;
      end
    end
    else if(b>a && b>c)begin
      if(c>a)begin
        max=b;
        second_max=c;
        min=a;
      end
      else begin
        max=b;
        second_max=a;
        min=c;
      end
    end
    else if(c>a&&c>b)begin
      if(a>b)begin
        max=c;
        second_max=a;
        min=b;
      end
      else begin
        max=c;
        second_max=b;
        min=a;end end
      
    else begin
      max=1'bx;
        second_max=1'bx;
        min=1'bx;
    end
    end
 
endmodule
 ```
```
module test;
  reg [3:0]a,b,c;
  wire [3:0]max,second_max,min;
  max1 dut(a,b,c,max,second_max,min);
  initial begin
    repeat(10)begin
      {a,b,c}=$random;
      #2;
    end
//     a=4'd13;b=4'd14;c=4'd12;#10;
//     a=4'd11;b=4'd10;c=4'd14;#10;
//     a=4'd1;b=4'd3;c=4'd2;#10;
//      a=4'd1;b=4'd3;c=4'd2;#10;
//      a=4'd1;b=4'd4;c=4'd8;#10;
//     #100 $finish;
  end
  initial begin
    $monitor("simtime=%0t,a=%d,b=%d,c=%d,max=%d,second_max=%d,min=%d",$time,a,b,c,max,second_max,min);
  end
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars(0,test);
  end
endmodule
```
Output
```
Compiler version U-2023.03-SP2_Full64; Runtime version U-2023.03-SP2_Full64;  Sep 16 06:27 2025
simtime=0,a= 5,b= 2,c= 4,max= 5,second_max= 4,min= 2
simtime=2,a=14,b= 8,c= 1,max=14,second_max= 8,min= 1
simtime=4,a= 6,b= 0,c= 9,max= 9,second_max= 6,min= 0
simtime=6,a= 6,b= 6,c= 3,max= X,second_max= X,min= X
simtime=8,a=11,b= 0,c=13,max=13,second_max=11,min= 0
simtime=10,a= 9,b= 8,c=13,max=13,second_max= 9,min= 8
simtime=12,a= 4,b= 6,c= 5,max= 6,second_max= 5,min= 4
simtime=14,a= 2,b= 1,c= 2,max= X,second_max= X,min= X
simtime=16,a= 3,b= 0,c= 1,max= 3,second_max= 1,min= 0
simtime=18,a=13,b= 0,c=13,max= X,second_max= X,min= X
           V C S   S i m u l a t i o n   R e p o r t
```
