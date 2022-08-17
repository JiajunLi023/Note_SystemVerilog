### Datatypes of SystemVerilog
>1. Hardware  
>>1. Procedural Assignment -> reg  （It's a format of variable）
>>2. Continuous Assignment -> wire  
**reg is used in begin...end and wire use for assign**  
**reg表示存储结构，net表示导线结构，当不知道使用哪个时可以使用 logic type. However,logic只适用于单驱动端口,多驱动时，like inout， we use wire.**  
![微信图片_20220815201343](https://user-images.githubusercontent.com/96273504/185232215-08824020-d419-475c-99b1-ba1f9004b594.jpg)

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

### Initialization of Array
>1.Unique value  
>```int arr[3] = '{1,2,3};```  
>2.Repetition value  
>```int arr[10] = '{10{23}}; ```  
>3.Default value  
>```int arr[5] = '{default: 4};```  
>4.Uninitialization value  
>```int arr[8];```  
>5.Give value but no size  
>```
>int arr[] = {1,2,3,4};  // size = 4;
>int arr[];  //size = 0
>```

### How to initialize repetitive array?
>1. for loop
```
int arr[10];
initial begin
  for(int i = 0; i < 10; i++) begin
    arr[i] = i * i;
  end
end
```
>2. foreach
```
int arr[10];
initial begin
  foreach(arr[j]) begin
    arr[j] = j * j;
  end
end
```
>3.repeat
```
int arr[10];
int i = 0;
initial begin
  repeat(10) begin
    arr[i] = i * i;
    i = i + 1;
  end
end
```

### Two operation of Array
>1.Copy: make sure the size and datatype is the same that we can copy
```
int arr1[6];
int arr2[6];

initial begin
  foreach(arr1[i]) begin
    arr1[i] = i*5;
  end
  arr2 = arr1;
end
```

>2.Compare: Compare if two arries are the same 
```
int arr1[5] = '{1,2,3,4,5};
int arr2[5] = '{1,2,6,7,9};
int status;

initial begin
  status = (arr1 == arr2);
end
```

### Dynamic Array
>Use `new[number]` to define a dynamic array
```
int arr[];     //give no fixed size to the array

initial begin
  arr = new[5];   //alocate space to the array
  
  for(int i = 0; i < 5; i++) begin
    arr[i] = i*5;
  end
  
  arr = new[30];  //realocate space to array and old data will dispear
  arr = new[30](arr);   //it will remian the old data.
end
```

### Queue
>1. Initialize Queue:`int arr[$];`, add $ in [].  
>2. The Function of Queue
```
int arr[$];
int j =0;

initial begin
  arr = {1,2,3};      //1,2,3
  arr.push_front(10); // 10,1,2,3
  arr.push_back(6);   // 10,1,2,3,6
  arr.insert(2,9);    // 10,1,9,2,3,6
  j = arr.pop_front;  //j = 10, arr = {1,9,2,3,6}
  arr.pop_back;       //1,9,2,3
  arr.delete(1);      //1,2,3
end
```
