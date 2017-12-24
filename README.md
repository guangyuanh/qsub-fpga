
**This repo currently supports running only one SPEC at a time**

**Modification Before Use**

    There are three directory involved: QSUB (current repo), SPECKLE (https://github.com/guangyuanh/Speckle) and INITFS (https://github.com/guangyuanh/initramfs_linux_flow).
    To connect the last two to this repo, we need the following modifications

        commands/gen_spec_scripts.sh:
            select your INPUT_TYPE(ref or test)
            COMMAND_DIR=SPECKLE/riscv-spec-test/commands (you should have followed the instructions of speckle)
            OUTPUT_SUBDIR=QSUB/commands
            BENCHMARKS=(benchmarks you want to test)
            
        spawn.py:
            19: LINUX_SOURCE="INITFS"
            27: BUILD_DIR="QSUB/script-%s-%s" % (SUFFIX, OPTION)
            29: OUTPUT_DIR="QSUB/output-%s-%s" % (SUFFIX, OPTION)
            168: subprocess.check_call(["./build-initram.py", "--dir", "SPECKLE/" + dir_str], cwd=LINUX_SOURCE)
            171: target_dir = os.path.join("/home", "guangyuanh", "research", "riscv", "qsub-fpga") # A split path to your QSUB

**Build RISC-V Linux**

    Run:
    cd QSUB/commands/
    ./gen_spec_scripts.sh
    cd ..
    ./spawn.py -c -d -f commands/spec.test.txt
    
    Now a new bblvmlinux will be in your INITFS. The image will automatically run the commands in INITFS/profile. You can write you profile and prevent it from being overwritten in spawn.py.
