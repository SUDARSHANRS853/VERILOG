```
module fa(a,b,cin,s,co);
  input a,b,cin;
  output reg s,co;
  always@(*)begin
    {co,s}=a+b+cin;
  end
endmodule
module carry_look_ahead_adder(a,b,cin,s,co);
  input [3:0]a,b;
  output reg [3:0]s;
  input cin;
  output reg co;
  reg  [3:0]p;
  reg [3:0]g;
  reg [4:1]c;
  
  always@(*)begin
    p[0]=(a[0])^(b[0]);
    p[1]=(a[1])^(b[1]);
    p[2]=(a[2])^(b[2]);
    p[3]=(a[3])^(b[3]);
  end
  always@(*)begin
    g[0]=(a[0])&(b[0]);
    g[1]=(a[1])&(b[1]);
    g[2]=(a[2])&(b[2]);
    g[3]=(a[3])&(b[3]);
  end
  always@(*)begin
    c[1]=(g[0])|(p[0]&cin);
    c[2]=(g[1])|(p[1]&c[1]);
    c[3]=(g[2])|(p[2]&c[2]);
    c[4]=(g[3])|(p[3]&c[3]);
    co=c[4];
  end
  fa o1(a[0],b[0],cin,s[0]);
  fa o2(a[1],b[1],c[1],s[1]);
  fa o3(a[2],b[2],c[2],s[2]);
  fa o4(a[3],b[3],c[3],s[3]);
  
endmodule
  
```
```

module test;
  reg [3:0]a,b;
  wire [3:0]s;
  wire co;
  reg cin;
  integer i;
  carry_look_ahead_adder dut(a,b,cin,s,co);
//   initial begin
//     a=4'b0100;b=4'b0000;cin=1;#2;
//      a=4'b0100;b=4'b0000;cin=0;#2;
//      a=4'b0100;b=4'b0000;cin=1;#2;
//      a=4'b1100;b=4'b1000;cin=0;
//     #100 $finish;
//   end
  initial begin
  for(i=0;i<512;i++)
    begin
      {a,b,cin}=i;
      #2;
    end
    #1000 $finish;
  end
  initial begin
    $monitor("simtime=%0t,a=%b,b=%b,cin=%b,s=%b,co=%b",$time,a,b,cin,s,co);
  end
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars(0,test);
  end
endmodule
  
 ```
Output
```
Compiler version U-2023.03-SP2_Full64; Runtime version U-2023.03-SP2_Full64;  Sep 13 22:47 2025
simtime=0,a=0000,b=0000,cin=0,s=0000,co=0
simtime=10,a=0000,b=0000,cin=1,s=0001,co=0
simtime=20,a=0000,b=0001,cin=0,s=0001,co=0
simtime=30,a=0000,b=0001,cin=1,s=0010,co=0
simtime=40,a=0000,b=0010,cin=0,s=0010,co=0
simtime=50,a=0000,b=0010,cin=1,s=0011,co=0
simtime=60,a=0000,b=0011,cin=0,s=0011,co=0
simtime=70,a=0000,b=0011,cin=1,s=0100,co=0
simtime=80,a=0000,b=0100,cin=0,s=0100,co=0
simtime=90,a=0000,b=0100,cin=1,s=0101,co=0
simtime=100,a=0000,b=0101,cin=0,s=0101,co=0
simtime=110,a=0000,b=0101,cin=1,s=0110,co=0
simtime=120,a=0000,b=0110,cin=0,s=0110,co=0
simtime=130,a=0000,b=0110,cin=1,s=0111,co=0
simtime=140,a=0000,b=0111,cin=0,s=0111,co=0
simtime=150,a=0000,b=0111,cin=1,s=1000,co=0
simtime=160,a=0000,b=1000,cin=0,s=1000,co=0
simtime=170,a=0000,b=1000,cin=1,s=1001,co=0
simtime=180,a=0000,b=1001,cin=0,s=1001,co=0
simtime=190,a=0000,b=1001,cin=1,s=1010,co=0
simtime=200,a=0000,b=1010,cin=0,s=1010,co=0
simtime=210,a=0000,b=1010,cin=1,s=1011,co=0
````
    
    
    
