# Broadcom Peripherals Map
![Pasted image 20250114174009](attachments/Pasted%20image%2020250114174009.png)

# GPIO
* Starts on page 90 of the Broadcom datasheet
* Notice that IO peripherals are at address `0x7Exxxxxx` in the **bus address** space.
* However, the **physical address** of the IO peripherals is at `0x20xxxxxx`
* We need to do this because we don't have virtual memory implemented yet; we are using the physical address space
![Pasted image 20250114174206](attachments/Pasted%20image%2020250114174206.png)

# Common GPIO Operations
## Important Addresses
* `GPFSEL2` (Function Select): `0x20200008`
	* Controls the function/mode of GPIO pins 20-29. Each pin uses 3 bits to select its function (input, output, alternate functions)
* `GPSET0` (Set pins): `0x2020001C`
	* Writing 1 to any bit in this register will set (**turn on**) the corresponding GPIO pin. Writing 0 has no effect
* `GPCLR0` (Clear pins): `0x20200028`
	* Writing 1 to any bit in this register will clear (**turn off)** the corresponding GPIO pin. Writing 0 has no effect
* `GPLEV0` (Pin level): `0x20200034`
	* Reading this register returns the current level (0 or 1) of each GPIO pin. Used to check pin input values
## How to Set GPIO Pin 20 to Output
```c
// GPFSEL2 controls pins 20-29
// Set bits 0-2 to 001 for output mode
PUT32(0x20200008, bit_clear(GET32(0x20200008), 0,2) | 0b001)
```
## How to Turn GPIO Pin 20 On
```c
// Write 1 to bit position 20 in GPSET0
// In general, write to i-th bit of GPSET0 to set i-th GPIO pin on
PUT32(0x2020001C, (1 << 20));
```
## How to Turn GPIO Pin 20 Off
```c
// Write 1 to bit position 20 in GPCLR0
// In general, write to i-th bit of GPCLR0 to clear i-th GPIO pin
PUT32(0x20200028, (1 << 20));
```
## How to Read GPIO Pin 20
```c
// Read from GPLEV0 and mask bit 20
// In general, read from GPLEV0 and mask i-th bit to get i-th GPIO pin value
unsigned val = GET32(0x20200034);
unsigned pin20_value = (val >> 20) & 1;
```

## Common Patterns
1. Always initialize pins before use:
	* Set pin function (input/output)
	* Configure any additional settings
2. When modifying registers:
	* Read current value
	* Clear relevant bits
	* Set new bits
	* Write back
3. Use constants for addresses:
```c
enum {
	 GPFSEL2 = 0x20200008,
	 GPSET0	= 0x2020001C,
	 GPCLR0	= 0x20200028,
	 GPLEV0	= 0x20200034
};
```
## GPIO Pin Functions
* `000`: Input (default)
* `001`: Output
* `100`: Alt function 0
* `101`: Alt function 1
* `110`: Alt function 2
* `111`: Alt function 3
* `011`: Alt function 4
* `010`: Alt function 5

## Device Implementation Details
### Memory Barriers
* Must use memory barriers (`dev_barrier()`) when:
	1. Before accessing any Broadcom device
	2. When switching between different devices
	3. In interrupt handlers (before and after device access)
* Memory barriers ensure:
	* All previous loads/stores complete before execution continues
	* No memory operations below barrier propagate above it
* Note: Sequences of accesses to the same device don't need barriers
### Device Configuration
* Device enable is not instantaneous
* Complex devices may need 10+ milliseconds after enable
* Always check datasheet for initialization timing requirements
* Add explicit delays after initialization with comments referencing datasheet page numbers
### Code Best Practices
1. Use `PUT32`/`GET32` instead of direct memory access
	* Prevents compiler optimization issues
	* Makes it easier to track and debug device access
2. Always verify configurations:
```c
// Example: Write and verify
void device_write(unsigned addr, unsigned val) {
	 PUT32(addr, val);
	 unsigned readback = GET32(addr);
	 if(readback != val) 
			panic("Write verification failed!");
}
```
3. Document with datasheet page numbers:
```c
// From BCM2835 datasheet p.95: Write to GPSET0 to set pin
PUT32(GPSET0, (1 << pin_number));
```
4. Initialize devices in disabled state:
	* Configure while disabled
	* Enable only after full configuration
	* Clear FIFOs before enabling
5. Polling vs Interrupts:
	* Start with polling for simplicity
	* Only use interrupts when necessary
	* Be aware of FIFO limitations (e.g., UART has 8-byte receive FIFO)
### Common Gotchas
1. Voltage Matching:
	* Never connect 5V device to 3.3V pi pin
	* Ensure correct voltage levels for inputs/outputs
2. Hardware Issues:
	* Buy multiple devices for testing
	* Try different pi's if issues persist
	* Check for loose connections
	* Power issues can affect receiving more than sending
3. Configuration Timing:
	* Device may not be ready immediately after enable
	* Check datasheet for required delays
	* Add explicit delays with comments
4. FIFO Management:
	* Clear FIFOs before enabling device
	* Be aware of FIFO size limitations
	* Handle overflow conditions
