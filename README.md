**<u>STM32CubeIDE mp157f_breakout project</u>**

This the STM32CubeIDE project for generating device tree source files (.dts) and compiling the device tree binary files (.dtb).

Note that this repo only contains the STM32CubeIDE project file (.ioc) and the generated device tree source files. 

**<u>Directory Structure</u>**

<pre><font color="#3465A4"><b>mp157f_breakout/CA7/</b></font>
└── <font color="#3465A4"><b>DeviceTree</b></font>
    └── <font color="#3465A4"><b>mp157f_breakout</b></font>
        ├── <font color="#3465A4"><b>kernel</b></font>
        │   └── stm32mp157f-mp157f_breakout-mx.dts
        ├── <font color="#3465A4"><b>tf-a</b></font>
        │   ├── stm32mp157f-mp157f_breakout-mx.dts
        │   └── stm32mp15-mx.dtsi
        └── <font color="#3465A4"><b>u-boot</b></font>
            ├── stm32mp157f-mp157f_breakout-mx.dts
            ├── stm32mp157f-mp157f_breakout-mx.dts.bak
            ├── stm32mp157f-mp157f_breakout-mx-u-boot.dtsi
            └── stm32mp15-mx.dtsi
</pre>

The top level folder should be the target of the `mp157_breakout` symbolic link used in the [meta-breakoutboard-layer](https://github.com/cracked-machine/meta-breakoutboard-layer) repo.

The [stm32mp1-breakoutboard.conf](https://github.com/cracked-machine/meta-breakoutboard-layer/blob/main/conf/machine/stm32mp1-breakoutboard.conf) file in the [meta-breakoutboard-layer](https://github.com/cracked-machine/meta-breakoutboard-layer) repo should reference the generic .dts filenames used here. e.g. `tm32mp157f-mp157f_breakout-mx` as well as the relative path of these dts files (including the `mx` directory):

```
# CubeMX Project Config
# =========================================================================
# Assign CubeMX Board devicetree and project path name
CUBEMX_DTB = "stm32mp157f-mp157f_breakout-mx"
CUBEMX_PROJECT = "mx/mp157_breakout/CA7/DeviceTree/mp157f_breakout"
```

You can check the build output from Bitbake to check that the dtb file is being used. Especially when you get an error ;)

```
| arch/arm/dts/stm32mp157f-mp157f_breakout-mx.dtb: ERROR (phandle_references): /cpus/cpu@0: Reference to non-existent node or label "vddcore"
| 
| arch/arm/dts/stm32mp157f-mp157f_breakout-mx.dtb: ERROR (phandle_references): /cpus/cpu@1: Reference to non-existent node or label "vddcore"
| 
| arch/arm/dts/stm32mp157f-mp157f_breakout-mx.dtb: ERROR (phandle_references): /soc/sdmmc@58005000: Reference to non-existent node or label "v3v3"
```

**<u>Project Setup</u>**

To compile the source file dts into a binary dtb you must setup the IDE project:

1) Import this .ioc project file into STM32CubeIDE. (File New > Import from an existing ioc project)<br><br>
2) Then right click the CA7 sub project and import an OpenSTLinux project for linux-5.4.31 [[ref]](https://wiki.st.com/stm32mpu/wiki/How_to_manage_OpenSTLinux_project_in_STM32CubeIDE).<br><br>
3) There should be a dtb build target under the linux-5.4.31 sub project that references the parent project. Right click and build on the DTB target: <br><br>`dtb (stm32mp157f-mp157f_breakout-mx.dtb)
` <br><br>We haven't made any changes to the linux kernel so you don't need to rebuild all..rebuilding the entire linux kernel will take forever. 

