# STM32 Flash Memory Safety Enhancement

This project enhances the safety of embedded applications on the **NUCLEO-L432KC** development board by filling unused flash memory with a reset instruction. This ensures that if the program counter ventures into unallocated flash memory, the system safely restarts by redirecting execution to the reset handler.

## 🚀 Features
- **Automatic Reset on Undefined Execution:** Prevents undefined behavior by placing a reset instruction in unused flash memory.
- **Dynamic Flash Calculation:** Linker script dynamically calculates the unused flash memory regions.
- **ARM Cortex-M4 Compatibility:** Tailored for STM32L432KCUx with support for other STM32 devices.
- **STM32CubeIDE Support:** Integrates seamlessly with the STM32CubeIDE toolchain and GNU ARM Embedded Toolchain.

## 💡 Why This Matters
In embedded systems, undefined behavior can occur if the program counter accesses uninitialized or invalid memory regions. By filling unused flash with a reset instruction, this project ensures that such events trigger a safe system reset, improving reliability and robustness.

## 🛠️ Tools & Technologies
- **Development Board:** [NUCLEO-L432KC](https://www.st.com/en/evaluation-tools/nucleo-l432kc.html) (STM32L432KCUx MCU)
- **Toolchain:** STM32CubeIDE with GNU ARM Embedded Toolchain
- **Architecture:** ARM Cortex-M4
- **Language:** C (with custom linker script modifications)

## 📋 Getting Started

### Prerequisites
- STM32CubeIDE installed on your system.
- NUCLEO-L432KC board with ST-LINK configured for programming and debugging.

### Steps
1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/stm32-flash-safety.git
   ```
2. Import the project into STM32CubeIDE.
3. Modify the linker script if necessary to fit your application's memory requirements.
4. Build and flash the firmware onto your NUCLEO-L432KC board.

## 🛠️ How It Works
1. The linker script calculates unused flash memory dynamically after placing all required sections (`.text`, `.data`, `.rodata`, etc.).
2. The unused flash memory is filled with a reset instruction (or equivalent) using the `FILL` directive.
3. If the program counter accesses this region, it redirects execution to the reset handler.

### Example Code Snippet (Linker Script)
```ld
.fill (NOLOAD) :
{
  . = ALIGN(4);
  FILL(ORIGIN(FLASH)); /* Replace with reset instruction if required */
  . += (ORIGIN(FLASH) + LENGTH(FLASH)) - .;
} >FLASH
```
Make sure to include this script at the END of the linker script. Otherwise, the `.fill` section might overlap with sections like `.data` because the linker tries to allocate `.fill` before it calculates the size of other sections.
