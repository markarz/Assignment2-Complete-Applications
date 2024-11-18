# Complete Applications


## Requirements
Need to complete the code for the following file and pass the test cases.
- abs.s
- argmax.s
- classify.s
- dot.s
- matmul.s
- read_matrix.s
- relu.s
- write_matrix.s


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
    lw t0, 0(a0)#
    li t1, 0#max index
    li t2, 1#i=1
loop_start:
    beq t2, a1, loop_end  # If t1 == a1, exit the loop (i >= size)
    slli t3, t2, 2        # t2 = t1 * 4 (calculate byte offset)
    add t4, a0, t3        # calculate address
    lw t5, 0(t4)          # t5 = array[i]
    blt t5 t0 skip        # If t5 <= t0, exit the loop (array[i]<=max)
    add t0,t5,zero       
    add t1,t2,zero
    
skip:
    addi t2, t2, 1        # Increment index (t2++)
    j loop_start  


loop_end:
    addi a0,t1,0
    ret                   # Return to caller
    

handle_error:
    li a0, 36
    j exit
```
#### explain
The method for handling negative numbers:Subtract the negative number from zero to make it positive.

## Useful Resources


