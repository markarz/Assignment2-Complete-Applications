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
```
    abs:
    # Prologue
    ebreak
    # Load number from memory
    lw t0,0(a0)
    bge t0, zero, done
    sub t0,zero,t0
    sw t0,0(a0)'''
done:
    # Epilogue
    jr ra
```

## Useful Resources


