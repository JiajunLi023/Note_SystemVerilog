### How many kind of signals there?
>1. **Global signal** : clk, rst  
>2. **Data signal** : rdata, wdata  
>3. **Control signal** : WR, OE, raddr, waddr

### What is initial block?
>1. Initial block start execution at 0 sec.  
>2. Initial block can have one statement and multiple statements.  
```
initial a = 1;   //one statement

initial begin    //multiple statements
  a = 1;
  #10;
  a = 0
end
```
### Usage of Initial Block
>1.**Initialized Global signals**  
>2.**Random Signal for data / control**  
>3.**System Task at the start of simulation**  
>4.**Analyzing values of variable on Console**  
>5.**Stop simulation by forcefully calling**  
```
reg clk;
reg rst;
reg[3:0] temp;   //define the variables
//1. Initialized Global signals
initial begin 
  clk = 0;
  rst = 0;
end 

//2. Random Signal for data / control
initial begin
  temp = 4'b0000;
  #10;
  temp = 4'b0110;
  #10;
  temp = 4'b1100;
end

//3.System Task at the start of simulation
initial begin
  $dumpfile("dump.vcd");  //open a VCD database to record
  $dumpvars;  //record all signals
end

//4. Analyzing values of variable on Console
initial begin
  $monitor("temp : %0d at time : %0t", temp, $time);
end

//5.Stop simulation by forcefully calling 
initial begin
  #200;
  $finish();
end
```

### The format of always block
```
always @() //in () is the sensitive variable
```
> 1.always_comb
all inputs in ()
```
always @(a,b) // y = a & b
```
> 2.always_ff
```
always @(posedge clk)
```
> 3.always_latch  
> **However, we always don't use sensitive list always block in testbench because we just generate the stimulus**

### The Usage of always block
> Generate CLK
```
`timescale 1ns / 1ps

module tb();
  reg clk100;
  reg clk50;
  reg clk25;
  reg rst;
  
  initial begin
    clk100 = 0;
    clk50 = 0;
    clk25 = 0;
    rst = 0;
  end
  
  always #20 clk25 = ~clk25;
  always #10 clk50 = ~clk50;
  always #5 clk100 = ~clk100;
  
  initial begin 
    $dumpfile("dump.vcd");
    $dumpvars;
  end
  
  initial begin
    #200;
    $finish();
  end
endmodule
```

### How to make the clks posedge at the same time?
```
always 5# clk100 = ~clk100;
always begin
  5#;
  clk50 = ~clk50;
  #10;
  clk50 = ~clk50;
  #5;
end
always begin
  5#;
  clk25 = ~clk25;
  #20;
  clk25 = ~clk25;
  #15;
end
```
### What is timesacle?
```
`timescale time unit / time precision;
```
> For example: ``timescale 1ns / 1ps`  
>precision = 1ns / 1ps = 10^3  
>so #10.21 = #10.21, #10.231 = #10.231, #10.2356 = #10.236  

### The Parameter of Generating Clock
>1.frequence  
>2.duty cycle = ton / (ton + toff)  
>3.phase
```
`timescale 1ns / 1ps

module tb();
  reg clk;
  reg clk50;
  
  initial begin
    clk = 0;
    clk50 = 0;
  end
  
  always #5 clk = ~clk;
  
  real phase;
  real ton;
  real toff;
  
  task ParameterGen(input real phasein, period, dutyCycle, output real phase, ton, toff); //define a  function to calculate the phase, ton and toff
    phase = phasein;
    ton = period * dutyCycle;
    toff = period - ton;
  endtask
  
  task ClkGen(input real phase, ton, toff);   //define a function to generate the clk
    @(posedge clk);
    #phase;
    while(1) begin
      clk50 = 1;
      #ton;
      clk50 = 0;
      #toff;
    end
  endtask
  
  initial begin
    ParameterGen(0, 40, 0.4, phase, ton, toff);  // use the functions
    ClkGen(phase, ton, toff);
  end
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
  
  initial begin 
    #200;
    $finish();
  end
endmodule
```
