Dynamic Precision-Scalable Systolic Array using DFX - Lab P1, Group 1
Board: ZedBoard (Zynq-7000 xc7z020clg484-1). Tool: Vivado 2022.2.

Contents
  Report_RC_Project_DFX_Systolic_Array.pdf   project report (2 pages)
  PPT_RC_Project_FINAL_GRP1_P1.pdf           presentation
  Source_Files/                              Verilog, xdc, VIO IP, bit files and test vectors

No .coe files are used: the design has no memory initialisation; matrices are loaded at run time through the VIO.

How to run on the board (Vivado Hardware Manager)
  1. Program Source_Files/bitstreams/t2_full_int16.bit with probes file t2_full_int16.ltx.
  2. Swap precision by loading any t2_partial_*.bit (same probes file). A partial bitstream needs a full one loaded first.

Source_Files
  rtl/top_t2.v              static top: controller, feed registers, start gate, decoupler, uptime counter, LEDs, VIO
  rtl/controller.v          feed controller (staircase schedule), from the int16 baseline design
  rtl/array.v, rtl/pe.v     4x4 int16 array and DSP-based processing element (used by the int16 module)
  rtl/rst_sync.v            reset synchroniser for the push button
  rtl/uptime.v              seconds counter and 1 Hz heartbeat
  rtl/dfx_decouple.v        holds the swappable region's outputs at safe values during a swap
  rtl/rm/rm_int16.v         int16 module: 16 DSP slices, 48-bit accumulators, result clamped to 32 bits
  rtl/rm/rm_int8.v          int8 module: inputs clamped to 8 bits, LUT multipliers, 18-bit accumulators
  rtl/rm/rm_int4.v          int4 module: inputs clamped to 4 bits, LUT multipliers, 10-bit accumulators
  rtl/rm/rm_common.v        shared parts: input clamp, result select, module ID and LED pattern
  rtl/rm/array_lut.v        4x4 array of LUT processing elements (int8, int4)
  rtl/rm/pe_lut.v           LUT-based processing element
  tb/tb_rp_t2.v             testbench for one module on its own (9 test cases)
  tb/tb_top_t2.v            testbench for the full design with one module (9 cases, cycle count, start gate)
  tb/vio_t2_stub.v          simulation model of the VIO core
  constraints/top_t2.xdc        pins and 100 MHz clock
  constraints/top_t2_pblock.xdc reconfigurable region (Pblock) in clock region X1Y1
  ip/vio_t2.xci             VIO debug core configuration
  bitstreams/               4 full and 4 partial bitstreams (int16, int8, int4, empty), probes file, checksums
  sim_vectors/t2_vectors/   expected inputs and outputs read by the testbenches (3 precisions x 9 cases)

Notes
  - rm_int16.v, rm_int8.v and rm_int4.v all define module rp_t2; compile one of them at a time.
  - The empty (greybox) module has no source file; Vivado generates it.
  - The testbenches read vectors from ../../t2_vectors/; run the simulator from a folder two levels
    below sim_vectors/, for example sim_vectors/build/run/.
