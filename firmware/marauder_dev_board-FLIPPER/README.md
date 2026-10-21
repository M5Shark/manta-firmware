..\\esptool.exe --chip esp32 --port COM7 --baud 921600 --before default\_reset --after hard\_reset write\_flash -z --flash\_mode dio --flash\_freq 80m --flash\_size detect 0xe000 boot\_app0.bin 0x1000 esp32\_marauder.ino.bootloader.bin 0x10000 esp32\_marauder\_v1\_8\_4\_20250806\_marauder\_dev\_board.bin

