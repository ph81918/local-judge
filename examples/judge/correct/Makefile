CC := riscv64-unknown-linux-gnu-gcc
CFLAGS := -static
TARGET := 1 2 3 4
v := 0

all: $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -o $@ $^
	
judge: all
	@judge -v ${v} || printf "or \`make judge v=1\`"

clean:
	rm -f $(TARGET)
