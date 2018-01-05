Flash Validation
================

OVERVIEW
--------

This project constitutes of scripts and C source code to automate validating
flash storage parts (eMMC and UFS). There are two types of tests:

- Python Scripts: Test that need to retain states when the device can't.
  For example Factory Reset.
- C Source: Test that validate features of the storage part by calling APIs
  available in Linux.

Some of the tests are:
- Verify no errors (0): Verifies if there were no storage errors before the
  other tests are run. Error in this test means the storage driver reported
  an error before starting the test, typically means storage error during
  bootup. This is a simple test and can be used as a template for new tests.
- Clock scaling (1): Verify the flash device properly handles changing the
  speed of the clocks.
- Clock Gating (2): Verify that the processor (APQ) is capable of turning on
  and off the clock. Tests indirectly hibernate for UFS.
- Flash Suspend (3): Verify the flash device can be suspended and brought out
  of suspend.
- RPMB Concurrency (4): Verify IO operations on RPMB while concurrently doing
  regular IO.
- Performance (5): Run throughput measurements. Measurements are done on IO
  for a file that fills most of the user partition. For more details what is
  executed check the TAMIO project.
- Read Disturb XBL/SBL (6): Read the XBL/SBL partition many times. The data
  should remain constant and not get corrupted. This partition usually is
  marked as enhanced.
- Read Disturb Boot (7): Read the Boot partition many times. The data
  should remain constant and not get corrupted. This partition shouldn't be
  marked as enhanced to test a different scenario from Read Disturb XBL/SBL.
- Discard XBL/SBL (8): Verifies the Discard operation on XBL/SBL Backup. The
  test will write some random data, verify the data was written (read back),
  issue a discard, verify the discarded data is is filled with zeros.
- Discard Boot (9): Verifies the Discard operation on apps boot Backup. The
  test will write some random data, verify the data was written (read back),
  issue a discard, verify the discarded data is is filled with zeros.
- Reboot test [Python]: Reboot the device and make sure the driver does not
  report errors.

How to use
----------

To run the full suite of flash validation tests do:

    python adbscripts/run_fv.py

The C source code pre-built binaries can be found at:

    ./libs/<architecture>/flashval

Command line options
--------------------
    usage: run_fv.py [-h] [-s SERIAL] [-o OUTPUT_DIR]

    Run the flash validation framework

    optional arguments:
      -h, --help            show this help message and exit
      -s SERIAL, --serial SERIAL
                            ADB device serial to use for the test.
      -o OUTPUT_DIR, --output_dir OUTPUT_DIR
                            Directory where to place output files. By default the
                            "out" directory will be created in the current working
                            directory.

How to Build
------------

Building is only supported for Linux hosts. To build run 'scons'.

The message error message below indicates the android NDK isn't available in
the default path. Download the NDK from android.com
(https://developer.android.com/ndk/downloads/index.html), extract it and change
the path in ./make_android_bin.sh to where NDK resides.

    ERROR: NDK Build path not set correctly. Update NDK path in ./make_android_bin.sh to build android binaries

