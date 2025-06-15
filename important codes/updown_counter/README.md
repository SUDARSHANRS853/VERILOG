# 3 bit UPDOWN COUNTER
0 1 2 3 4 5 6 7 7 6 5 4 3 2 1 0
```
module counter(clk,rst,up_down,count);
  input clk,rst,up_down;
  output reg [2:0]count;
  always@(posedge clk or posedge rst)begin
    if(rst)
      count<=3'b000;
    else begin
          if(up_down)begin
            if(count<3'b111)
              count=count+1;
            else count<=count;end
          else begin
            if(count>3'b000)
              count<=count-1;
            else count<=count;
        end
    end
  end
endmodule

module test;
  reg clk,rst,up_down;
  wire [2:0]count;
  counter dut(clk,rst,up_down,count);
  initial begin
    clk=0;
    forever #5 clk=~clk;
  end
  initial begin
    rst=1;
    up_down=0;
    #10;
    rst=0;
    up_down=1;
    #80;
    up_down=0;
    #100;
    $stop;
    
  end
  initial begin
    $monitor("simtime=%0t,rst=%b,up_down=%b,count=%d",$time,rst,up_down,count);
  end
endmodule
```
Output
```
simtime=0,rst=1,up_down=0,count=0
simtime=10,rst=0,up_down=1,count=0
simtime=15,rst=0,up_down=1,count=1
simtime=25,rst=0,up_down=1,count=2
simtime=35,rst=0,up_down=1,count=3
simtime=45,rst=0,up_down=1,count=4
simtime=55,rst=0,up_down=1,count=5
simtime=65,rst=0,up_down=1,count=6
simtime=75,rst=0,up_down=1,count=7
simtime=90,rst=0,up_down=0,count=7
simtime=95,rst=0,up_down=0,count=6
simtime=105,rst=0,up_down=0,count=5
simtime=115,rst=0,up_down=0,count=4
simtime=125,rst=0,up_down=0,count=3
simtime=135,rst=0,up_down=0,count=2
simtime=145,rst=0,up_down=0,count=1
simtime=155,rst=0,up_down=0,count=0
```
# 3bit updown counter
0 1 2 3 4 5 6 7 6 5 4 3 2 1 0
```
module counter (
  input clk,
  input rst,
  output reg [2:0] count,
  output reg up_down
);
  always @(posedge clk or posedge rst) begin
    if (rst) begin
      count <= 3'b000;
      up_down <= 1'b1; // start by counting up
    end else begin
      if (up_down) begin
        count <= count + 1;
        if (count == 3'b110) // next will be 7
          up_down <= 1'b0;   // switch to down after reaching 7
      end else begin
        count <= count - 1;
      end
    end
  end
endmodule
module test;
  reg clk, rst;
  wire [2:0] count;
  wire up_down;

  counter dut(clk, rst, count, up_down);

  initial begin
    clk = 0;
    forever #5 clk = ~clk; // 10ns clock
  end

  initial begin
    rst = 1; #10;
    rst = 0;
    #150; // run long enough to capture full sequence
    $stop;
  end

  initial begin
    $monitor("Time=%0t | rst=%b | up_down=%b | count=%0d", $time, rst, up_down, count);
  end
endmodule
```
Output
```
Time=0 | rst=1 | up_down=1 | count=0
Time=10 | rst=0 | up_down=1 | count=0
Time=15 | rst=0 | up_down=1 | count=1
Time=25 | rst=0 | up_down=1 | count=2
Time=35 | rst=0 | up_down=1 | count=3
Time=45 | rst=0 | up_down=1 | count=4
Time=55 | rst=0 | up_down=1 | count=5
Time=65 | rst=0 | up_down=1 | count=6
Time=75 | rst=0 | up_down=0 | count=7
Time=85 | rst=0 | up_down=0 | count=6
Time=95 | rst=0 | up_down=0 | count=5
Time=105 | rst=0 | up_down=0 | count=4
Time=115 | rst=0 | up_down=0 | count=3
Time=125 | rst=0 | up_down=0 | count=2
Time=135 | rst=0 | up_down=0 | count=1
Time=145 | rst=0 | up_down=0 | count=0
Time=155 | rst=0 | up_down=0 | count=7
```
