---
layout: assembly
---
# Makefile
```MAKEFILE
#
#	Makefile for H8-MicomBoard Samples
#
main.mot: main.coff
	h8300-hms-objcopy -O srec $^ $@

main.coff: start.o main.o lcd.o sci.o dump.o \	# 材料
# ここに追記
	data.o subroutines.o \
	wait.o draw.o
	h8300-hms-ld -o $@ -T 3052.lds $^			# 実行される文

.S.o:
	h8300-hms-as $^ -o $@

.PHONY: clean write

clean:
	rm -rf *.o *.mot *.coff

write: main.mot
	sudo h8write -3052 main.mot /dev/ttyUSB0 1>&2

```
ダウンロード: [Makefile](assets/Makefile)