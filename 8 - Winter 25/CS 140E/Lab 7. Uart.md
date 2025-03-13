* How to enable?
	* pg. 9 ⟶ AUXENB
* Disable interrupts?
* Has data?
* Done transmitting?

![Pasted image 20250130180917](../../attachments/Pasted%20image%2020250130180917.png)



1. **Turn on UART** first of else none of the registers will work
	* pg. 9 ⟶ write a 1 to bit 0 of AUXENB
	* read-modify-write because you don't want to break the other devices
	* remember to add a memory/device barrier right after this ⟶ this is a new device
	![Pasted image 20250130180748](../../attachments/Pasted%20image%2020250130180748.png)
2. You can **read and write to the UART** using these registers
	* pg. 11
	![Pasted image 20250130181148](../../attachments/Pasted%20image%2020250130181148.png)
3. You should **enable the transmitter and receiver** in the UART
	* This is on pg. 17
	* You can also use this to disable it
		* Disable it after the memory barrier
		* Disable it before you clear the queues
	![Pasted image 20250130181312](../../attachments/Pasted%20image%2020250130181312.png)

4. To **clear the TX and RX** queues:
	* This is pg. 13
	* Remember to disable the transmitter/receiver before clearing the queues!
	![Pasted image 20250130181433](../../attachments/Pasted%20image%2020250130181433.png)

5. To **disable UART-generated interrupts:**
	* This is pg. 12
	* errata: these bits are read/write bits
	![Pasted image 20250130181650](../../attachments/Pasted%20image%2020250130181650.png)
6. To know whether the UART is in **7-bit or 8-bit mode**, use this:
	* This is on page 14
	![Pasted image 20250130182113](../../attachments/Pasted%20image%2020250130182113.png)
7. To check the status of the transmitter queue, you can also use this:
	![Pasted image 20250130181901](../../attachments/Pasted%20image%2020250130181901.png)

8. To check if the UART transmitter has space to write to:
	![Pasted image 20250130181935](../../attachments/Pasted%20image%2020250130181935.png)
	![Pasted image 20250130181942](../../attachments/Pasted%20image%2020250130181942.png)
9. To set the baud rate, use this:
	* this is on page 19
	![Pasted image 20250130182236](../../attachments/Pasted%20image%2020250130182236.png)
	![Pasted image 20250130181107](../../attachments/Pasted%20image%2020250130181107.png)
	* We want `115200 Baud, 8 bits, 1 start bit, 1 stop bit`
		* Our clock frequency is 250 MHz
		* The baud rate should be `reg = clock/(8 * baudrate) - 1`
		* Note that the ARM has a diff clock rate ⟶ about `700 MHZ`


10. To check if we are done transmitting, we can use `transmitter done`:
	* this is on pg 18
	![Pasted image 20250130182642](../../attachments/Pasted%20image%2020250130182642.png)

11. To check if the UART receiver has data, we can use `symbol available`:
	* this is on pg 19
