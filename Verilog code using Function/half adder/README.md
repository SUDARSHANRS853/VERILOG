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
