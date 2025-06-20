## 3 bit UPDOWN COUNTER
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
## 3bit updown counter
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
## 3. Priority encoder
```
// Code your testbench here
// or browse Examples
module priority_encoder_8to3(d,i,valid);
  input [3:0]d;
  output reg[1:0]i;
  output reg valid;
  always@(d)begin
    valid=1'b1;
    casex(d)
      
      4'b0000:i=2'b00;
      4'b001x:i=2'b01;
      4'b01xx:i=2'b10;
      4'b1xxx:i=2'b11;
  default:begin 
    i=2'b00;
    valid=1'b0;
  end
    

    endcase
  end
endmodule

//Test bench
module pe42d_test;
  reg [3:0]d;
  wire [1:0]i;

  
  priority_encoder_8to3 dut(d,i,valid);
  initial begin 
    repeat(10)begin
      {d}=$random;
      #10;
    
//     d=4'b0000;
//     #5 d=4'b0010;
//     #5 d=4'b0100;
//     #5 d=4'b1000;
//     #5 d=4'b0011;   
  end 
  end
  
    initial begin
      $monitor("simtime=%0t,d=%b,i=%b",$time,d,i);
    end
    initial begin
      $dumpfile("dump.vcd");
      $dumpvars(0, pe42d_test);
    end
  endmodule
```
Output
```
simtime=0,d=0100,i=10
simtime=10,d=0001,i=00
simtime=20,d=1001,i=11
simtime=30,d=0011,i=01
simtime=40,d=1101,i=11
simtime=60,d=0101,i=10
simtime=70,d=0010,i=01
simtime=80,d=0001,i=00
```
## 4. Maximum value of three numbers using function
```
module max_val (
    input signed [31:0] a, b, c,
    output integer max_val
);

    // Function declaration inside the module
    function integer maximum;
        input signed [31:0] a, b, c;
        begin
            if (a > b && a > c)
                maximum = a;
            else if (b > c)
                maximum = b;
            else
                maximum = c;
        end
    endfunction

    // Use the function to assign output
    assign max_val = maximum(a, b, c);

endmodule

//testbench
module tb_max_val;

    // Declare testbench variables
    reg signed [31:0] a, b, c;
    wire integer max_val;

    // Instantiate the module under test
    max_val uut (
        .a(a),
        .b(b),
        .c(c),
        .max_val(max_val)
    );

    initial begin
        // Monitor outputs
      $monitor("Time=%0t | \ta=%0d,\t b=%0d,\t c=%0d => \tmax_val=%0d", $time, a, b, c, max_val);

        // Test cases
        a = 10; b = 20; c = 30; #5;
        a = -5; b = 15; c = 0;  #5;
        a = -20; b = -10; c = -30; #5;
        a = 50; b = 50; c = 10; #5;
        a = 100; b = 200; c = 150; #5;

        $finish;
    end

endmodule
```
0utput
```
Time=0 | 	a=10,	 b=20,	 c=30 => 	max_val=30
Time=5 | 	a=-5,	 b=15,	 c=0 => 	max_val=15
Time=10 | 	a=-20,	 b=-10,	 c=-30 => 	max_val=-10
Time=15 | 	a=50,	 b=50,	 c=10 => 	max_val=50
Time=20 | 	a=100,	 b=200,	 c=150 => 	max_val=200
```
## 5. Parity Checker
```
module parity_gen(input [7:0] din, output dout);

    // Function to compute parity (odd parity)
    function parity(input [7:0] din);
        integer i;
        reg temp;
        begin
            temp = 0;
            for (i = 0; i <= 7; i = i + 1)
                temp = temp ^ din[i]; // XOR reduction
            parity = temp;
        end
    endfunction

    assign dout = parity(din); // Use the function to assign parity output

endmodule

//testbench
module tb;
    reg [7:0] din;
    wire dout;

    // Instantiate the DUT (Device Under Test)
    parity_gen dut(din, dout);

    initial begin
        repeat (5) begin
            din = $random;
            #2;
        end
    end

    initial begin
        $monitor("Din = %b, Parity = %b", din, dout);
    end
endmodule
```
Output
```
Din = 00100100, Parity = 0
Din = 10000001, Parity = 0
Din = 00001001, Parity = 0
Din = 01100011, Parity = 0
Din = 00001101, Parity = 1
```
## 6. Fibonacci series
```module fibonacci;

    integer ft = 0; // First term
    integer st = 1; // Second term
    integer nt;     // Next term
    integer n_t = 10; // Number of terms to generate

    // Fibonacci function
    function void fib(input [31:0] n);
        integer i;
        begin
            $write("%0d\t", ft);
            $write("%0d\t", st);
            for (i = 3; i <= n; i = i + 1) begin
                nt = ft + st;
                ft = st;
                st = nt;
                $write("%0d\t", nt);
            end
        end
    endfunction

    // Invoke the function
    initial begin
        $display("Fibonacci sequence:");
        fib(n_t);
        $display(); // for newline
    end

endmodule
```
Output
```
 0          1          1          2          3          5          8         13         21         34
```
## 7.Prime number detection
```
module prime_det;
  integer n=21;
   reg prime;
  
  function prime_detect;
    input [31:0]a;
    integer i;
    begin
      if(a==0||a==1)
      prime_detect=0;
    else begin
      prime_detect=1;
      for(i=2;i<=a/2;i=i+1)
        if(a%i==0)begin
          prime_detect=0;
      break;
      end
    end
    end
  endfunction
  initial begin
    prime=prime_detect(n);
    if(prime)
      $display("%d is the prime number",n);
    else 
      $display("%d is not prime number",n);
  end
endmodule
```
Output
```
21 is not prime number
```
## 8. palindrome number
```
module palindrome(input [31:0] a, output reg p);
  reg [31:0] org_num;

  // Palindrome detection function
  function automatic bit pal(input [31:0] n);
    integer rem;
    integer rev_num = 0;
    begin
      while (n != 0) begin
        rem = n % 10;
        rev_num = rev_num * 10 + rem;
        n = n / 10;
      end

      if (rev_num == a)
        pal = 1; // number is palindrome
      else
        pal = 0; // number is not palindrome
    end
  endfunction

  // Use always block to update output
  always @(*) begin
    org_num = a;
    p = pal(org_num);
  end
endmodule


    module palindrome_tb;
  reg [31:0] a;       // This will be the input number we test
  wire p;             // This will show if the number is a palindrome or not

  // Connect testbench signals to the palindrome module
  palindrome uut (
    .a(a),
    .p(p)
  );

  initial begin
    // Show what's happening
    $display("Time\tInput\tPalindrome?");
    $monitor("%0t\t%d\t%b", $time, a, p);

    // Test Case 1: Palindrome number
    a = 121;  // 121 is a palindrome
    #10;

    // Test Case 2: Not a palindrome
    a = 123;  // 123 is not a palindrome
    #10;

    // Test Case 3: Another palindrome
    a = 1221; // 1221 is a palindrome
    #10;

    // Test Case 4: Single-digit (always palindrome)
    a = 7;    // 7 is a palindrome
    #10;

    // Test Case 5: Zero (edge case, considered palindrome)
    a = 0;    
    #10;

    // Test Case 6: Large number not a palindrome
    a = 123456789;
    #10;

    // End simulation
    $finish;
  end
endmodule
```
Output
```
Time	Input	Palindrome?
0	       121	1
10	       123	0
20	      1221	1
30	         7	1
40	         0	1
50	 123456789	0
```
## 9. priority Encoder using if else block
```
module priority_encoder_4to2 (
  input [3:0] d,
  output reg [1:0] i
  
);
  always @(*) begin
    if (d == 4'b0000) begin
      i = 2'b00;
     
    end else if (d[3]) begin
      i = 2'b11;
      
    end else if (d[2]) begin
      i = 2'b10;
      
    end else if (d[1]) begin
      i = 2'b01;
    end else begin // d[0] is set
      i = 2'b00;
      
    end
  end
endmodule
module pe42d_test;
  reg [3:0] d;
  wire [1:0] i;
 

  priority_encoder_4to2 dut(d, i );

  initial begin 
    // Apply random inputs
    repeat(10) begin
      d = $random;
      #10;
    end
    
    // Also test edge cases explicitly
    d = 4'b0000; #10;
    d = 4'b0001; #10;
    d = 4'b0010; #10;
    d = 4'b0100; #10;
    d = 4'b1000; #10;
    d = 4'b1010; #10;
    d = 4'b1111; #10;
    $stop;
  end

  initial begin
    $monitor("Time=%0t | d=%b | i=%b ", $time, d, i);
  end

  initial begin
    $dumpfile("dump.vcd");
    $dumpvars(0, pe42d_test);
  end
endmodule
```
Output
```
Time=0 | d=0100 | i=10 
Time=10 | d=0001 | i=00 
Time=20 | d=1001 | i=11 
Time=30 | d=0011 | i=01 
Time=40 | d=1101 | i=11
```
## Factorial of number using functions
```
module factorial (input [31:0] N, output [63:0] 
facto);
 assign facto = fact(N);
 function automatic[63:0]  fact (input [31:0] N);
 begin
 if(N>1)
 fact = fact(N-1) *N;
 else
 fact = 1;
 end
 endfunction
 endmodule

module fact_tb;
 reg [31:0] N;
 wire [63:0] facto;
 factorial dut(N,facto);
 initial begin 
N= 3;
 #2 N=2;
 #3 N=4;
 end
 initial begin
 $monitor(N,facto);
 end 
endmodule
```
Output
```
# 3       6
# 2       2
# 4       24
```
## MUX 4*1 using 2*1
```
module mux21(input [1:0] i, input s, output y);
  assign y = (s) ? i[1] : i[0];
endmodule

module mux41(input [3:0] i, input [1:0] sel, output y);
  wire w0, w1;

  // First level of muxes
  mux21 m1(.i({i[1], i[0]}), .s(sel[0]), .y(w0));
  mux21 m2(.i({i[3], i[2]}), .s(sel[0]), .y(w1));

  // Second level mux
  mux21 m3(.i({w1, w0}), .s(sel[1]), .y(y));
endmodule

module tb_mux41;

  reg [3:0] i;       // 4 data inputs
  reg [1:0] sel;     // 2-bit select line
  wire y;            // output from mux41

  // Instantiate the mux41
  mux41 uut (
    .i(i),
    .sel(sel),
    .y(y)
  );

  initial begin
    $display("Time | sel | inputs     | y");
    $display("-----------------------------");

    // Apply test vectors
    i = 4'b1010; // i[3]=1, i[2]=0, i[1]=1, i[0]=0

    sel = 2'b00; #10; $display("%4t | %b   | %b | %b", $time, sel, i, y);
    sel = 2'b01; #10; $display("%4t | %b   | %b | %b", $time, sel, i, y);
    sel = 2'b10; #10; $display("%4t | %b   | %b | %b", $time, sel, i, y);
    sel = 2'b11; #10; $display("%4t | %b   | %b | %b", $time, sel, i, y);

    $finish;
  end
endmodule
```
Output
```
Time | sel | inputs     | y
-----------------------------
  10 | 00   | 1010 | 0
  20 | 01   | 1010 | 1
  30 | 10   | 1010 | 0
  40 | 11   | 1010 | 1
```

