# 2121 Final Exam Sample Questions

## Short Answer 
**( I have it on pretty shitty authority that this was last sem's q's )**

###### What is a stack? How do we implement a stack in AVR? Why does the stack grow from higher memory addresses?

A stack is a data structure that has the FIFO property. The only operations we can preform on a stack is pushing data on, and popping it off. In AVR, the stack is implemented as a block of consecutive bytes in SRAM, and a stack pointer that keeps track of the 'top' of the stack. 

The stack beings at RAMEND and grows upwards, allowing the top of SRAM to be used for registers and other general data as the stack grows upwards. Furthermore the commands used to access the stack's data manually (LDD) can only adjust the pointer they are given positively. Thus reading from the top of the stack downwards works fine as the stack pointer is dsiplaced to higher addresses. 
If the stack grew towards higher addresses then going from the top of the stack down would require the stack pointer to be subtracted which LDD does not support directly.  

To implement this in AVR simply set up the stack pointer as such

```
ldi r16, low(RAMEND)
out SPL, r16
ldi r16, high(RAMEND)
out SPH, r16
```
and then use the commands push and pop which will automatically add the data to the top of the stack from a register
and adjust the stack pointer as needed. 

###### How do we choose the right frequency for analog-digital conversion?

The frequency should always be twice the frequency of the analog signal being read in to prevent aliasing. Aliasing is when our freqency is too great, and we read in data points from different parts of different peaks of wave, and create a wave that has a period that is far larger than usual.

###### Why do we take 3 samples for USART in AVR?

###### In AVR, we use polling and interrupts. Compare the two and list when we would use one or the other.

Polling is the action of checking a certain condition at regular intervals untill the condition triggers a certain peice of code. This requires the code to manually check the condition every so often and often requires a busy loop where the processor is not able to do any other work. This is also not as responsive as depending on the polling interval it may take some time for the processor to pick up on the condition being changed. This is still used though as it is easy to implement and requires no additional hardware or support. 

interrupts are usually built into a processor and allow a external pin or internal condition to automatically trigger a response. Usually once the interrupts are activated the processor stops it's current task and after finishing the last command it was on will handle the interrupt code. Once done it returns and resumes. This is more responsive and allows the code to do other work while waiting for a interrupt but can interrupt code in the middle of a calculation or process possibly causing issues and lag. 

Polling is great for non-time critical applications to produce cheap response acitivty where only one task is needed to be done, such as checking for a button to be pressed to activate some other peice of hardware. 
Interrupts are better for time critical applications to produce quick responses where the processor has other work to be doing. An example is a alarm system which must be detecting input from sensors, calculating, responding to keypads and driving alarms. 
 
###### Find the errors in the code (theres 10 altogether)
	* a constant is too negative (i.e rolled over to positive)
	* wrong instruction
	* wrong branch condition


###### You're given a fictitious microprocessor, and its instruction set. You're asked to write a program that multiplies two 8-bit numbers together (or two 16 bit, most likely 8 bit). You are then asked to encode it in binary.



## Sample Programming Questions

1. Write a program that calculates the n'th Fibonnaci number. Assume `n` is read in from register 16. Store the output in register 17.

```
.include "m2560def"

.def first=r18
.def second=r19
.def count=r20
.def next=r17
.def temp=r21

fib:
	ldi first, 0
	ldi second, 1
	
	
	StartLoop:
	cp count, r16
	breq halt
	
		cpi count, 2
		brgt NORMAL
		mov next, count
		inc count
		jump StartLoop
	
		NORMAL:
		
		ld temp, first
		add temp, second
		
		mov output, temp
		mov first, second
		mov second, next
		
		inc count
	
		jump StartLoop
		
halt:
	jmp halt
	
	

```
