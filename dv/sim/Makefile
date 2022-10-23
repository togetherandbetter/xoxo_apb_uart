SIM_PATH=$(PWD)
FILE = testname.f
VERB=UVM_MEDIUM
SEED=$(shell date +%s)
TEST_NAMES =`cat $(FILE)`
##TEST=apbuart_config_test
TEST=

compile:
			@echo "Compiling Design"
			@echo "Current path: $(SIM_PATH)"
			vcs -sverilog -ntb_opts uvm-1.2 -full64 -f flist -timescale=1ns/1ps -l out/vcs.log \
			-cm line+tgl+cond+branch+assert+fsm -assert -full64 -LDFLAGS '-Wl,--no-as-needed' -o out/simv

run_all:
			@echo "Running Tests"
			@for i in $(TEST_NAMES); do \
				./simv -l $$i.log +UVM_TIMEOUT=900000000 +UVM_TESTNAME=$$i;\
			done

test-list:
			@for i in $(TEST_NAMES); do \
				echo $$i; \
			done

run_cov_all:		
			@echo "Running Tests with Coverage"
			rm -rf ${TEST_NAMES} ; mkdir ${TEST_NAMES}
			@for i in $(TEST_NAMES); do \
				./simv -l $$i.log +UVM_TIMEOUT=900000000 +UVM_TESTNAME=$$i -cm line+tgl+cond+branch+assert+fsm; \
				cp -rf simv.vdb $$i/; \
				mv $$i.log $$i/; \
			done
			urg -dir *_test/*.vdb -elfile exclusion.el -excl_bypass_checks	

run:
			@echo "Running $(TEST) Test"
			@echo $(SIM_PATH)
			cd out; mkdir $(TEST); cd $(TEST);\
			../simv -l ${TEST}.log +UVM_TIMEOUT=900000000 +UVM_TESTNAME=${TEST} UVM_VERBOSITY=${VERB} +ntb_random_seed=${SEED} 

dve: 		
			dve -covdir *_test/*.vdb &
verdi: 
			vcd2fsdb dump.vcd -o verilog.fsdb
			verdi -sv -f flist verilog.fsdb & 

clean:
			@echo "Cleaning up...."
			rm -rf csrc DVEfiles urgReport simv.vdb ${TEST_NAMES} inter.vpd dump.vcd simv simv.daidir ucli.key vc_hdrs.h vcs.log .inter.vpd.uvm *.log
