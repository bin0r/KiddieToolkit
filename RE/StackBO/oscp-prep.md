# OSCP Buffer Overflow for Dummies

The purpose of this text is to aid the BO part in the OSCP course in a structured way.


## Checklist

- [ ] Crash the application by sending a large amount of characters into a smaller buffer ( A good approach would be sending 100 characters and incrementing until a crash occurs )
- [ ] Identify the exact amount of characters required for crashing the application by creating a string with unique characters - Find the location of the EIP
```
msf-pattern_create -l { Ammount of characters to be generated }
```
- [ ] Locate space for the shellcode - After the EIP or Before it
- [ ] Find bad characters 
> 0x00 should always be excluded from the valid characters
- [ ] Find a JMP ESP instruction in the dll's ( JMP ESP opcode - *FFE4* )
> DEP and ASLR should be disabled
> There shouldn't be any bad characters in the base address of the modules
```
!mona find -s "\xff\xe4" -m {The module you chose}
```
- [ ] Change the EIP to the the JMP ESP instruction from the previous step
> Note that the format is Little Endian, therefore the address should be in reverse order: 0x10090c83 --> \x83\x0c\x09\x10
