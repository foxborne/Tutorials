# GNU Make project for 'Samples.Localization'

# Parallel builds can be enabled using the environment variable MAKEFLAGS (eg: MAKEFLAGS="-j 4")
# For parallel execution in Windows, Make 4.0 or better is required

include ../../../../../Config/Common.makeprops
include ../../../../../Config/Profile/Profile.makeprops
include ../../../../../Config/Release/Release.makeprops
include ../../../../../Config/Debug/Debug.makeprops

CC = arm-linux-gnueabihf-gcc
CXX = arm-linux-gnueabihf-g++
AR = arm-linux-gnueabihf-ar

SHELLTYPE = msdos
ifeq (, $(ComSpec)$(COMSPEC))
  SHELLTYPE = posix
endif
ifeq (/bin, $(findstring /bin, $(SHELL)))
  SHELLTYPE = posix
endif
ifeq (/bin, $(findstring /bin, $(MAKESHELL)))
  SHELLTYPE = posix
endif

ifeq (posix, $(SHELLTYPE))
  DEVNUL = /dev/null
  WHICH = which
  MKDIR = mkdir -p "$(1)"
  BIN2H = ../../../../../Tools/Bin2h/bin2h.py
  CP = cp "$(1)" "$(2)"
  MV = mv "$(1)" "$(2)"
  RM = rm -f "$(1)"
  RMTREE = rm -rf "$(1)"
  FIXSLASHES = 
else
  DEVNUL = NUL
  WHICH = where
  MKDIR = mkdir "$(subst /,\,$(1))" 2> nul || exit 0
  BIN2H = ..\..\..\..\..\Tools\Bin2h\bin2h.exe
  CP = copy /Y "$(subst /,\,$(1))" "$(subst /,\,$(2))" > nul
  MV = move /Y "$(subst /,\,$(1))" "$(subst /,\,$(2))" > nul
  RM = del /Q "$(subst /,\,$(1))" 2> nul || exit 0
  RMTREE = rmdir /S /Q "$(subst /,\,$(1))" 2> nul || exit 0
  SED = ..\..\..\..\..\Tools\sed\sed.exe
  FIXSLASHES = $(SED) -i "s/\(\S\)\\\(\S\)/\1\/\2/g" "$(1)"
endif

ifeq (, $(shell ${WHICH} $(CC) 2> $(DEVNUL) ))
$(error $(CC) not found in PATH)
endif

ifeq (, $(shell ${WHICH} $(CXX) 2> $(DEVNUL) ))
$(error $(CXX) not found in PATH)
endif

ifeq ($(V), 1)
  Q =
else
  Q = @
endif

ifndef CONFIG
  CONFIG = Release
endif

ifeq ($(CONFIG), Profile)
  TARGET_DIR = ../../../../../../Bin/linux_arm
  TARGET = ../../../../../../Bin/linux_arm/Samples.Localization
  INTERMEDIATE = build/Profile
  LIB_DIRS = 
  LIBS = -lNoesisApp -lNoesis
  LIBS_DEPS = ../../../../../../Bin/linux_arm/libNoesis.so ../../../../../../Bin/linux_arm/libNoesisApp.so
  INCLUDES = -I../../../../../../Include -I../../../../App/ApplicationLauncher/Include -I../../../../App/Display/Include -I../../../../App/DisplayLauncher/Include -I../../../../App/Launcher/Include -I../../../../App/Providers/Include -Ibuild/Profile
  DEFINES = -DNS_BUILD -DNS_CONFIG=Profile -DNS_VERSION=3.0.0 -DNS_APP_FRAMEWORK -DNS_LICENSE -DNS_PROFILE -DNS_MINIMUM_LOG_LEVEL=2
  CFLAGS = $(CFLAGS_PROFILE)
  CXXFLAGS = $(CXXFLAGS_PROFILE)
  LDFLAGS = $(LDFLAGS_PROFILE)
else ifeq ($(CONFIG), Release)
  TARGET_DIR = ../../../../../../Bin/linux_arm
  TARGET = ../../../../../../Bin/linux_arm/Samples.Localization
  INTERMEDIATE = build/Release
  LIB_DIRS = 
  LIBS = -lNoesisApp -lNoesis
  LIBS_DEPS = ../../../../../../Bin/linux_arm/libNoesis.so ../../../../../../Bin/linux_arm/libNoesisApp.so
  INCLUDES = -I../../../../../../Include -I../../../../App/ApplicationLauncher/Include -I../../../../App/Display/Include -I../../../../App/DisplayLauncher/Include -I../../../../App/Launcher/Include -I../../../../App/Providers/Include -Ibuild/Release
  DEFINES = -DNS_BUILD -DNS_CONFIG=Release -DNS_VERSION=3.0.0 -DNS_APP_FRAMEWORK -DNS_LICENSE
  CFLAGS = $(CFLAGS_RELEASE)
  CXXFLAGS = $(CXXFLAGS_RELEASE)
  LDFLAGS = $(LDFLAGS_RELEASE)
else ifeq ($(CONFIG), Debug)
  TARGET_DIR = ../../../../../../Bin/linux_arm
  TARGET = ../../../../../../Bin/linux_arm/Samples.Localization
  INTERMEDIATE = build/Debug
  LIB_DIRS = 
  LIBS = -lNoesisApp -lNoesis
  LIBS_DEPS = ../../../../../../Bin/linux_arm/libNoesis.so ../../../../../../Bin/linux_arm/libNoesisApp.so
  INCLUDES = -I../../../../../../Include -I../../../../App/ApplicationLauncher/Include -I../../../../App/Display/Include -I../../../../App/DisplayLauncher/Include -I../../../../App/Launcher/Include -I../../../../App/Providers/Include -Ibuild/Debug
  DEFINES = -DNS_BUILD -DNS_CONFIG=Debug -DNS_VERSION=3.0.0 -DNS_APP_FRAMEWORK -DNS_LICENSE -DNS_DEBUG -DNS_PROFILE -DNS_MINIMUM_LOG_LEVEL=1
  CFLAGS = $(CFLAGS_DEBUG)
  CXXFLAGS = $(CXXFLAGS_DEBUG)
  LDFLAGS = $(LDFLAGS_DEBUG)
else
  $(error Invalid configuration '$(CONFIG)')
endif

PCH =
PCHFLAGS =
DATA = $(INTERMEDIATE)/App.xaml.bin.h $(INTERMEDIATE)/MainWindow.xaml.bin.h $(INTERMEDIATE)/Resources.xaml.bin.h $(INTERMEDIATE)/Language-en.xaml.bin.h $(INTERMEDIATE)/Language-fr.xaml.bin.h $(INTERMEDIATE)/Language-jp.xaml.bin.h $(INTERMEDIATE)/Flag-en.png.bin.h $(INTERMEDIATE)/Flag-fr.png.bin.h $(INTERMEDIATE)/Flag-jp.png.bin.h $(INTERMEDIATE)/Oxygen-Regular.ttf.bin.h $(INTERMEDIATE)/FontopoNIHONGO.otf.bin.h

.SUFFIXES:
.DELETE_ON_ERROR:
.PHONY: all prebuild data clean

all: prebuild $(TARGET)

prebuild:
	@echo ==== [Samples.Localization] Building '$(CONFIG)' configuration ====

data: $(DATA)

$(INTERMEDIATE):
	$(Q)$(call MKDIR,$(INTERMEDIATE))

$(TARGET_DIR):
	$(Q)$(call MKDIR,$(TARGET_DIR))

$(INTERMEDIATE)/%.o: ../../../../Samples/Localization/Src/%.cpp $(PCH) | $(INTERMEDIATE) data
	@echo [CXX] [Samples/Localization] $(notdir $<)
	$(Q)$(CXX) $(CXXFLAGS) $(PCHFLAGS) $(INCLUDES) $(DEFINES) -fPIC -c -MMD -MF $(basename $@).d.tmp -MP -o $@ $< && $(call MV,$(basename $@).d.tmp,$(basename $@).d)

	$(Q)$(call FIXSLASHES,$(@:.o=.d))

$(INTERMEDIATE)/%.o: ../../../../Samples/Localization/Src/%.c | $(INTERMEDIATE) data
	@echo [CC] [Samples/Localization] $(notdir $<)
	$(Q)$(CC) $(CFLAGS) $(INCLUDES) $(DEFINES) -fPIC -c -MMD -MF $(basename $@).d.tmp -MP -o $@ $< && $(call MV,$(basename $@).d.tmp,$(basename $@).d)

	$(Q)$(call FIXSLASHES,$(@:.o=.d))

$(INTERMEDIATE)/App.xaml.bin.h: ../../../../Samples/Localization/Data/App.xaml | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/App.xaml" > "$(INTERMEDIATE)/App.xaml.bin.h" 

$(INTERMEDIATE)/MainWindow.xaml.bin.h: ../../../../Samples/Localization/Data/MainWindow.xaml | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/MainWindow.xaml" > "$(INTERMEDIATE)/MainWindow.xaml.bin.h" 

$(INTERMEDIATE)/Resources.xaml.bin.h: ../../../../Samples/Localization/Data/Resources.xaml | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Resources.xaml" > "$(INTERMEDIATE)/Resources.xaml.bin.h" 

$(INTERMEDIATE)/Language-en.xaml.bin.h: ../../../../Samples/Localization/Data/Language-en.xaml | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Language-en.xaml" > "$(INTERMEDIATE)/Language-en.xaml.bin.h" 

$(INTERMEDIATE)/Language-fr.xaml.bin.h: ../../../../Samples/Localization/Data/Language-fr.xaml | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Language-fr.xaml" > "$(INTERMEDIATE)/Language-fr.xaml.bin.h" 

$(INTERMEDIATE)/Language-jp.xaml.bin.h: ../../../../Samples/Localization/Data/Language-jp.xaml | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Language-jp.xaml" > "$(INTERMEDIATE)/Language-jp.xaml.bin.h" 

$(INTERMEDIATE)/Flag-en.png.bin.h: ../../../../Samples/Localization/Data/Flag-en.png | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Flag-en.png" > "$(INTERMEDIATE)/Flag-en.png.bin.h" 

$(INTERMEDIATE)/Flag-fr.png.bin.h: ../../../../Samples/Localization/Data/Flag-fr.png | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Flag-fr.png" > "$(INTERMEDIATE)/Flag-fr.png.bin.h" 

$(INTERMEDIATE)/Flag-jp.png.bin.h: ../../../../Samples/Localization/Data/Flag-jp.png | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Flag-jp.png" > "$(INTERMEDIATE)/Flag-jp.png.bin.h" 

$(INTERMEDIATE)/Oxygen-Regular.ttf.bin.h: ../../../../Samples/Localization/Data/Oxygen-Regular.ttf | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/Oxygen-Regular.ttf" > "$(INTERMEDIATE)/Oxygen-Regular.ttf.bin.h" 

$(INTERMEDIATE)/FontopoNIHONGO.otf.bin.h: ../../../../Samples/Localization/Data/FontopoNIHONGO.otf | $(INTERMEDIATE)
	@echo [BIN] [Samples/Localization] $(notdir $<)
	$(Q)$(BIN2H) "../../../../Samples/Localization/Data/FontopoNIHONGO.otf" > "$(INTERMEDIATE)/FontopoNIHONGO.otf.bin.h" 

$(TARGET): $(LIBS_DEPS) $(INTERMEDIATE)/ViewModel.o $(INTERMEDIATE)/MainWindow.xaml.o $(INTERMEDIATE)/Main.o $(INTERMEDIATE)/App.xaml.o | $(TARGET_DIR)
	@echo [LD] $(notdir $@)
	+$(Q)$(CXX) $(LDFLAGS) -pie -Wl,-rpath,\$$ORIGIN -Wl,-rpath,../../../../../../Bin/linux_arm -L../../../../../../Bin/linux_arm $(LIB_DIRS) -o $(TARGET) @$(CONFIG).rsp $(LIBS)
	@echo [STRIP] $(notdir $@)
	$(Q)arm-linux-gnueabihf-objcopy --only-keep-debug $(TARGET) $(TARGET).debug
	$(Q)arm-linux-gnueabihf-strip --strip-unneeded $(TARGET)
	$(Q)arm-linux-gnueabihf-objcopy --add-gnu-debuglink=$(TARGET).debug $(TARGET)

-include $(INTERMEDIATE)/ViewModel.d $(INTERMEDIATE)/MainWindow.xaml.d $(INTERMEDIATE)/Main.d $(INTERMEDIATE)/App.xaml.d

clean:
	@echo ==== [Samples.Localization] Cleaning '$(CONFIG)' configuration ====
	$(Q)$(call RMTREE,$(INTERMEDIATE))
	$(Q)$(call RM,$(TARGET))
	$(Q)$(call RM,$(TARGET).debug)
