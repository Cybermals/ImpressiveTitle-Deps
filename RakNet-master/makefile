#Makefile for RakNet Project
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
EXE = ./bin/$(MODE)-$(PLATFORM)/RakNet.dll
endif

ifeq ($(PLATFORM), Linux)
EXE = ./bin/$(MODE)-$(PLATFORM)/libRakNet.so
endif

ifeq ($(EXE), ***Invalid Platform***)
$(error Invalid Platform: Supported platforms are Win32, Linux.)
endif

#Dirs
SRC_DIR = ./Source
OBJ_DIR = ./obj
SRC = $(wildcard $(SRC_DIR)/*.cpp)
OBJ = $(SRC:$(SRC_DIR)/%.cpp=$(OBJ_DIR)/$(MODE)-$(PLATFORM)/%.o)
DEPS = $(OBJ:$(OBJ_DIR)/$(MODE)-$(PLATFORM)/%.o=$(OBJ_DIR)/$(MODE)-$(PLATFORM)/%.d)

#Flags (preprocessor, compiler, and linker)
CPPFLAGS += -I./DependentExtensions 
#CPPFLAGS += -I../DependentExtensions/openssl-1.0.0d/include
CFLAGS += -Wall
LDFLAGS += -shared
#LDFLAGS += -L../DependentExtensions/openssl-1.0.0d/lib

ifeq ($(PLATFORM), Win32)
LDLIBS += -lws2_32
#LDLIBS += -lssleay32 -llibeay32
endif

ifeq ($(PLATFORM), Linux)
LDLIBS += -lpthread
endif

ifeq ($(MODE), Debug)
CFLAGS += -g
else
CFLAGS += -O3
endif

.PHONY: all clean clean-Win32 clean-Linux


#Recipes
#=======
all: $(EXE)

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
	$(RM) obj\Debug-Linux\*.o
	$(RM) obj\Debug-Linux\*.d
	$(RM) obj\Release-Linux\*.o
	$(RM) obj\Release-Linux\*.d
