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
LDSCRIPT = src/SWM050.ld
CPU = cortex-m0

# Tools
CC = arm-none-eabi-gcc
AS = arm-none-eabi-as
AR = arm-none-eabi-ar
LD = arm-none-eabi-ld
SIZE = arm-none-eabi-size
OBJCOPY = arm-none-eabi-objcopy
OBJDUMP = arm-none-eabi-objdump

OCD = openocd
# OCDFLAGS = -f openocd.cfg
RM = rm -rf

# C Flags
GCFLAGS  = -g -nostartfiles -mthumb -mcpu=$(CPU)
GCFLAGS += $(INCLUDE)
LDFLAGS += -T$(LSCRIPT) -mthumb -mcpu=$(CPU)
ASFLAGS += -mcpu=$(CPU)


OBJ := $(addprefix $(OBJDIR)/,$(notdir $(SRC:.c=.o)))
OBJ += $(addprefix $(OBJDIR)/,$(notdir $(ASM:.s=.o)))

all:: $(BINDIR)/$(PROJECT).bin

Build: $(BINDIR)/$(PROJECT).bin

# install: $(BINDIR)/$(PROJECT).bin
# 	$(OCD) $(OCDFLAGS)

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


# Q = @
# CC=arm-none-eabi-gcc
# OBJCOPY=arm-none-eabi-objcopy
# OBJDUMP=arm-none-eabi-objdump

# # c flags to select the right processor
# CFLAGS= -Iinclude/ -g -nostartfiles -mthumb -mcpu=cortex-m0
# # ldscript to
# LDSCRIPT = src/SWM050.ld
# #library files
# OBJ = src/crt0.o src/SWM500_flash.o src/SWM500_tmrse.o src/system_SWM500.o
# OBJ += src/main.o src/SWM500_exti.o src/SWM500_gpio.o src/SWM500_wdt.o

# %.o: %.c
# 	@printf "  CC      $(*).c\n"
# 	$(Q)$(CC) -c -o $@ $< $(CFLAGS)

# %.o: %.s
# 	@printf "  AS      $(*).s\n"
# 	$(Q)$(CC) -c -o $@ $^ $(CFLAGS)

# $(PROJECT).elf: $(OBJ)
# 	@printf "  LD      $(@)\n"
# 	$(Q)$(CC) -o $@ $^ $(CFLAGS) -T $(LDSCRIPT)

# %.bin: %.elf
# 	@printf "  OBJCOPY $(*).bin\n"
# 	$(Q)$(OBJCOPY) -Obinary $(*).elf $(*).bin

# %.asm: %.elf
# 	@printf "  OBJDUMP $(*).asm\n"
# 	$(Q)$(OBJDUMP) -d $(*).elf > $(PROJECT).asm

# all:	$(PROJECT).elf $(PROJECT).bin $(PROJECT).asm

# .PHONY: clean

# clean:
# 	@rm -f *.o *~ core ./src/*.o $(PROJECT).elf $(PROJECT).bin $(PROJECT).asm

