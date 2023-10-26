
# Makeifle try to build ara with vcs
export RISCV=/opt/riscv_64gc
ARA_PATH=/home/yrr/ara
VCS_HOME=/synopsys/vcs2016.06
NOVAS_HOME=/synopsys/verdi2016.06
DPI_PATH=$(ARA_PATH)/hardware/tb/dpi
CFLAGS= -I$(VCS_HOME)/include \
	-I$(ARA_PATH)/hardware/tb/dpi \
	-I$(RISCV)/include \
	-std=c++17
VERDI=/synopsys/verdi2016.06/bin/verdi

analysis:
	vlogan -full64 -nc -sverilog -ntb_opts uvm-1.1
	vlogan -full64 -nc -sverilog -assert svaext \
	-error=noMPD \
	-timescale=1ns/1ps \
	-f vcs.flist \
	+define+VERILATOR +define+NOTRACER +define+VCS +define+DUMP_PCLOG \
	| tee vlogan.log
compile:
	vcs -full64 -nc -sverilog $(DPI) -timescale=1ns/1ps \
	-LDFLAGS -Wl,-no-as-needed -cpp g++-4.8 -cc gcc-4.8 \
	-ntb_opts uvm-1.1 \
        -debug_all -LDFLAGS -rdynamic \
        -P $(NOVAS_HOME)/share/PLI/VCS/LINUX64/novas.tab $(NOVAS_HOME)/share/PLI/VCS/LINUX64/pli.a \
	-top ara_tb -l vcs.log

build: clean analysis compile

sim:
	./simv "+PRELOAD=$(USER_PROG)" -l run-simv.log

read-waveform:
	$(VERDI) -nologo test.fsdb &

clean:
	rm -rf AN.DB simv* *.daidr *.vpd *.db csrc ucli.key vc_hdrs.h
	rm -rf work-dpi
	rm -f vlogan.log vcs.log run-simv.log
	rm -rf trace*
	rm -rf novas*
	rm -f *.log
	rm -f *.fsdb

clean-run-result:
	rm -f run-simv.log
	rm -f *.fsdb
