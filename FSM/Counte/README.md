# 1  2   4  7  SKIPPING COUNTER USNG FSM
```
module mealy_1247_fsm (
  input clk,
  input rst,
  output reg [2:0] out
);
  
  reg [1:0]ps,ns;
   parameter  S0 = 2'b00;
    parameter S1 = 2'b01;
    parameter S2 = 2'b10;
    parameter S3 = 2'b11;


  
  always @(posedge clk or negedge rst) begin
    if (!rst)
      ps <= S0;
    else
      ps <= ns;
  end


  always @(*) begin
    case (ps)
      S0: begin
        ns = S1;
        out = 3'b001;  
      end
      S1: begin
        ns = S2;
        out = 3'b010;  // 2
      end
      S2: begin
        ns = S3;
        out = 3'b100;  // 4
      end
      S3: begin
        ns = S0;
        out = 3'b111;  // 7
      end
      default: begin
        ns = S0;
        out = 3'b001;
      end
    endcase
  end

endmodule


module mealy_1247_fsm_tb;
  reg clk, rst;
  wire [2:0] out;
  mealy_1247_fsm uut (.clk(clk),.rst(rst),.out(out));
  initial begin
    clk = 0;
    forever #5 clk = ~clk;
  end 
  initial begin
    $display("Time\tRST\tOUT");
    $monitor("%0t\t%b\t%b", $time, rst, out);
    rst = 0;
    #10;
    rst = 1;
    #100;

    $finish;
  end

endmodule
```
