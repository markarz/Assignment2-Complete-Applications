# Complete Applications


## Requirements
Need to complete the code for the following file and pass the test cases.
- abs.s
- argmax.s
- relu.s
- dot.s
- matmul.s
- read_matrix.s
- write_matrix.s
- classify.s


## Usage
### abs.s:
#### code
```
abs:
    # Prologue
    ebreak
    # Load number from memory
    lw t0, 0(a0)        #Load 4 bytes of data from the memory address pointed to by a0 into register t0
    bge t0, zero, done  #If t0 >= 0, jump to the label "done"
    sub t0, zero, t5    #t0 = -t5 (Negative to positive)
    sw t0,0(a0)         #store 4 bytes of data from the memory address pointed to by a0 into register t0

done:
    # Epilogue
    jr ra
```
#### explain
The method for handling negative numbers:Subtract the negative number from zero to make it positive.

ex:0-(-5)=5
### argmax.s:
#### code
```
argmax:
    li t6, 1
    blt a1, t6, handle_error
    lw t0, 0(a0)            # current maximum
    li t1, 0                # current max index
    li t2, 1                # t2=1(loop index)
loop_start:
    beq t2, a1, loop_end    # If t1 == a1, exit the loop (i >= size)
    slli t3, t2, 2          # t2 = t1 * 4 (calculate byte offset)
    add t4, a0, t3          # t4 = base address + offset (address of array[i])
    lw t5, 0(t4)            # t5 = array[i]
    blt t5 t0 skip          # If t5 <= t0, exit the loop (array[i]<=max)
    add t0,t5,zero          # max = t5 
    add t1,t2,zero          # max index = t2
    
skip:
    addi t2, t2, 1        # t2++
    j loop_start  


loop_end:
    addi a0,t1,0
    ret                   # Return to caller
    

handle_error:
    li a0, 36
    j exit
```
#### explain
This function finds the maximum value and returns the index of the maximum value. If there are multiple occurrences of the maximum value, it returns the smallest index.

My approach is to update the index and value when a new maximum is found. To avoid incorrect updates caused by duplicate maximum values, I include a condition to skip the iteration if the value is equal to the current maximum.

### relu.s:
#### code
```
relu:
    li t0, 1              # t0 = 1
    blt a1, t0, error     # If a1 <= 0 (invalid size), jump to error

    li t1, 0              # t1 = 0 (loop index)
    addi t4, a0, 0        # t4 = base address of nums (copy a0)

loop_start:
    beq t1, a1, loop_end  # If t1 == a1, exit the loop (i >= size)

    slli t2, t1, 2        # t2 = t1 * 4 (calculate byte offset)
    add t3, t4, t2        # t3 = base address + offset (address of array[i])
    lw t5, 0(t3)          # t5 = array[i]

    blt zero, t5, skip    # If nums[i] < 0, skip (no modification)
    sw zero, 0(t3)        # Set nums[i] = 0
    addi t1, t1, 1        # Increment index (t1++)
    j loop_start  

skip:
    addi t1, t1, 1        # Increment index (t1++)
    j loop_start          # Jump to next iteration

loop_end:
    ret                   # Return to caller

error:
    li a0, 36             # Set error code 36
    j exit                # Jump to exit

```
#### explain
This function changes the value to 0 if it is negative.

My approach is to check if a value in the array is less than 0, and if so, directly replace it with 0 at the current array position.

### dot.s:
#### code
```
dot:
    li t0, 1
    blt a2, t0, error_terminate  
    blt a3, t0, error_terminate   
    blt a4, t0, error_terminate  

    li t0, 0  # result value       
    li t1, 0  # t1 = 0 (loop index)
    li t3, 0  # array1 index
    li t4, 0  # array2 index 

loop_start:#a3:array1's stride = stride1 , a4:array2's stride = stride2
    bge t1, a2, loop_end    # If t1 >= a2, exit the loop (i >= size) 
    slli t2, t3, 2          # t2 = t3 * 4 (calculate byte offset for first array)
    add t2, a0, t2          # t2 = base address + offset (address of array1[i])
    lw t5, 0(t2)            # t5 = array1[i] - Load the value at the address of array1[i] into t5
    slli t2, t4, 2          # t2 = t4 * 4 (calculate byte offset for second array)
    add t2,a1,t2            # t2 = base address + offset (address of array2[i])    
    lw t6,0(t2)             # t6 = array2[i] - Load the value at the address of array2[i] into t6
    mul t2,t5,t6            # t2 = t5 * t6 (multiply array1[i] by array2[i])
    add t0,t0,t2            # t0 = t0 + t2 (accumulate the result into t0)
    addi t1,t1,1            # Increment index (t1++) 
    add t3,t3,a3            # t3 = t3 + a3 (adjust index for array1)
    add t4,t4,a4            # t4 = t4 + a4 (adjust index for array2)
    j loop_start       

loop_end:
    mv a0, t0
    jr ra

error_terminate:
    blt a2, t0, set_error_36
    li a0, 37
    j exit

set_error_36:
    li a0, 36
    j exit

```
#### explain
This function calculates sum(Array1[i * stride1] * Array2[i * stride2]).

ex:

Stride1=1,Stride2=1
Array1[] = {1,2,3}
Array2[] = {4,5,6}
$ Ans = 1*4+2*5+3*6　$

Stride1=1,Stride2=2 
Array1[] = {1,2,3,4}
Array2[] = {5,6,7,8}
$$　Ans = 1*5+2*7  $$

My approach is to first calculate the addresses of both arrays, then use registers to load the values stored at those addresses (for both arrays). After that, I multiply the two values stored in the registers (t5 * t6), and store the result in another register (t2). Finally, I accumulate the result into another register (t0 = t0 + t2). Since the values of stride1 and stride2 are not necessarily 1, each iteration will adjust the array indices by adding stride1 to array1_index and stride2 to array2_index.


## Useful Resources


