PROJECT = blink

# Project Structure
SRCDIR = src
COMDIR = common
BINDIR = bin
OBJDIR = obj
INCDIR = include

# Sources
SRC = $(wildcard $(SRCDIR)/*.c) $(wildcard $(COMDIR)/*.c)
ASM = $(wildcard $(SRCDIR)/*.s) $(wildcard $(COMDIR)/*.s)

# Include directories
INCLUDE  = -I$(INCDIR)
# ldscript to
LDSCRIPT = lib/SWM050.ld

# Tools
CC = arm-none-eabi-gcc
AS = arm-none-eabi-as
AR = arm-none-eabi-ar
LD = arm-none-eabi-ld
OBJCOPY = arm-none-eabi-objcopy
SIZE = arm-none-eabi-size
OBJDUMP = arm-none-eabi-objdump
OCD = openocd
OCDFLAGS = -f openocd.cfg
RM = rm -rf
CPU = cortex-m0
# C Flags
GCFLAGS  = -std=c99 -Wall -g -nostartfiles -mthumb -mcpu=$(CPU)
GCFLAGS += $(INCLUDE)
LDFLAGS += -T$(LSCRIPT) -mthumb -mcpu=$(CPU) --specs=nosys.specs
ASFLAGS += -mcpu=$(CPU)


## Build process

OBJ := $(addprefix $(OBJDIR)/,$(notdir $(SRC:.c=.o)))
OBJ += $(addprefix $(OBJDIR)/,$(notdir $(ASM:.s=.o)))

all:: $(BINDIR)/$(PROJECT).bin

Build: $(BINDIR)/$(PROJECT).bin

install: $(BINDIR)/$(PROJECT).bin
	$(OCD) $(OCDFLAGS)

$(BINDIR)/$(PROJECT).hex: $(BINDIR)/$(PROJECT).elf
	$(OBJCOPY) -R .stack -O ihex $(BINDIR)/$(PROJECT).elf $(BINDIR)/$(PROJECT).hex

$(BINDIR)/$(PROJECT).bin: $(BINDIR)/$(PROJECT).elf
	$(OBJCOPY) -R .stack -O binary $(BINDIR)/$(PROJECT).elf $(BINDIR)/$(PROJECT).bin

$(BINDIR)/$(PROJECT).elf: $(OBJ)
	@mkdir -p $(dir $@)
	$(CC) $(OBJ) $(LDFLAGS) -o $(BINDIR)/$(PROJECT).elf
	$(OBJDUMP) -D $(BINDIR)/$(PROJECT).elf > $(BINDIR)/$(PROJECT).lst
	$(SIZE) $(BINDIR)/$(PROJECT).elf

macros:
	$(CC) $(GCFLAGS) -dM -E - < /dev/null

cleanBuild: clean

clean:
	$(RM) $(BINDIR)
	$(RM) $(OBJDIR)

# Compilation
$(OBJDIR)/%.o: $(SRCDIR)/%.c
	@mkdir -p $(dir $@)
	$(CC) $(GCFLAGS) -c $< -o $@

$(OBJDIR)/%.o: $(SRCDIR)/%.s
	@mkdir -p $(dir $@)
	$(AS) $(ASFLAGS) -o $@ $<


$(OBJDIR)/%.o: $(COMDIR)/%.c
	@mkdir -p $(dir $@)
	$(CC) $(GCFLAGS) -c $< -o $@

$(OBJDIR)/%.o: $(COMDIR)/%.s
	@mkdir -p $(dir $@)
	$(AS) $(ASFLAGS) -o $@ $<

# # uncomment to see the actual commandlines
# Q = @
# OBJ = lib/crt0.o lib/SWM500_flash.o lib/SWM500_tmrse.o lib/system_SWM500.o
# OBJ += lib/SWM500_exti.o lib/SWM500_gpio.o lib/SWM500_wdt.o
# OBJ += main.o

# c flags to select the right processor
# CFLAGS=-I. -Iinclude/ -g -nostartfiles -mthumb -mcpu=cortex-m0


# %.o: %.c
# 	@printf "  CC      $(*).c\n"
# 	$(Q)$(CC) -c -o $@ $< $(CFLAGS)

# %.o: %.s
# 	@printf "  AS      $(*).s\n"
# 	$(Q)$(CC) -c -o $@ $^ $(CFLAGS)

# $(PROJECT).elf: $(OBJ)
# 	@printf "  LD      $(@)\n"
# 	$(Q)$(CC) -o $@ $^ $(CFLAGS) -T $(LDSCRIPT)

# $(PROJECT).bin: $(PROJECT).elf
# 	@printf "  OBJCOPY\t$@\n"
# 	$(Q)$(OBJCOPY) -O binary  $< $@

# %.bin: %.elf
# 	@printf "  OBJCOPY $(*).bin\n"
# 	$(Q)$(OBJCOPY) -Obinary $(*).elf $(*).bin

# %.asm: %.elf
# 	@printf "  OBJDUMP $(*).asm\n"
# 	$(Q)$(OBJDUMP) -d $(*).elf > $(PROJECT).asm


# all: $(PROJECT).bin $(PROJECT).asm

# .PHONY: clean

# clean:
# 	@rm -f *.o *~ core ./lib/*.o $(PROJECT).elf $(PROJECT).bin $(PROJECT).asm

