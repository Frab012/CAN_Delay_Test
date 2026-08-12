# Test environment for CAN delay after quitting debugging session

- test environment for the issue [CANOpenNode slows down after quitting debugging session](https://github.com/CANopenNode/CanOpenSTM32/issues/104)
- project files for CubeIDE and Crossstudio available
- initialization of the submodule with `git submodule update --init --recursive`
- two Nucleo boards with Arduino CAN shields
    - NUCLEO-G070RB: the board (CAN id 1) where the issue is to be reproduced
    - Nucleo-L476RG: a generic board (CAN id 2) for sending PDOs
    - both devices are PDO producer/receiver
- [CANopen Linux](https://github.com/CANopenNode/CANopenLinux) or any other useful tool for sending/receiving SDOs via command line.
- procedure:
    - running the NUCLEO-G070RB board in debug mode
    - in CubeIDE: press Suspend, wait some seconds, press Resume
    - read some SDOs, e.g., using the CANopenLinux toolset:
        - `cocomm "1 r 0x2100 1"`, the expected output is `[1] ERROR:0x05040000 #SDO protocol timed out.`
        - `cocomm "1 r 0x2101 1"`, a delay is noticeable between transmission and reception
    - repeat that with the other board, no delay or timeout should occur
