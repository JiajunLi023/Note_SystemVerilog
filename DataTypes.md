### Datatypes of SystemVerilog
>1. Hardware  
>>1. Procedural Assignment -> reg  （It's a format of variable）
>>2. Continuous Assignment -> wire  
**reg is used in begin...end and wire use for assign**  
**reg表示存储结构，net表示导线结构，当不知道使用哪个时可以使用 logic type. However,logic只适用于单驱动端口,多驱动时，like inout， we use wire.**  
>2. Variable
>>1. Fixed Variable -> 2 states(0,1) and 4 states(0,1,x,z)
>>>2 states
>>>>1. Signed
>>>>> 8bit -> byte  
>>>>> 16bit -> shortint  
>>>>> 32bit -> int  
>>>>> 64bit ->longint  
>>>>2. Unsigned
>>>>> 8bit -> bit[7:0]  
>>>>> 16bit -> bit[15:0]  
>>>>> 32bit -> bit[31:0]
>>2. Floating Variable -> real
>3. Simulation
>>1.Fixed point -> time -> use `$time();` to return the time  
>>2.Floating point -> realtime -> use `$realtime();` to return the time
