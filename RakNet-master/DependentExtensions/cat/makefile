#Makefile for Cat Project
#============================
#Platform and mode Selection
PLATFORM = Win32
MODE = Debug
export PLATFORM
export MODE


#Global Variables
#================
#Tools
CC = g++

ifeq ($(PLATFORM), Win32)
CP = xcopy /E /S /Y
RM = del
endif

#Target
EXE = ***Invalid Platform***

ifeq ($(PLATFORM), Win32)
EXE = ./bin/$(MODE)-$(PLATFORM)/cat.dll
endif

ifeq ($(PLATFORM), Linux)
EXE = ./bin/$(MODE)-$(PLATFORM)/cat.so
endif

ifeq ($(EXE), ***Invalid Platform***)
$(error Invalid Platform: Supported platforms are Win32, Linux.)
endif

#Dirs
SRC_DIR = ./src
OBJ_DIR = ./obj
SRC = $(wildcard $(SRC_DIR)/*.cpp)
SRC += $(wildcard $(SRC_DIR)/codec/*.cpp)
OBJ = $(SRC:$(SRC_DIR)/%.cpp=$(OBJ_DIR)/$(MODE)-$(PLATFORM)/%.o)
DEPS = $(OBJ:$(OBJ_DIR)/$(MODE)-$(PLATFORM)/%.o=$(OBJ_DIR)/$(MODE)-$(PLATFORM)/%.d)

#Flags (preprocessor, compiler, and linker)
CPPFLAGS += -I..
CFLAGS += -Wall
LDFLAGS += -shared
LDLIBS += -lws2_32

ifeq ($(MODE), Debug)
CFLAGS += -g
else
CFLAGS += -O3
endif

.PHONY: all clean clean-Win32 clean-Linux


#Recipes
#=======
all: $(EXE) data-$(PLATFORM)

$(EXE): $(OBJ)
	$(CC) $(LDFLAGS) $^ $(LDLIBS) -o $@

-include $(DEPS)

$(OBJ_DIR)/$(MODE)-$(PLATFORM)/%.o: $(SRC_DIR)/%.cpp
	$(CC) $(CPPFLAGS) $(CFLAGS) -c $< -o $@ -MMD -MF $(@:.o=.d)

clean: clean-$(PLATFORM)

clean-Win32:
	$(RM) obj\Debug-Win32\*.o
	$(RM) obj\Debug-Win32\*.d
	$(RM) obj\Release-Win32\*.o
	$(RM) obj\Release-Win32\*.d

clean-Linux:
	$(RM) $(OBJ)
	$(RM) $(DEPS)

data-Win32:
	$(CP) .\data .\bin\$(MODE)-$(PLATFORM)\data

data-Linux:
	$(CP) -r ./data ./bin/$(MODE)-$(PLATFORM)/data