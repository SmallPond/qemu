# Changes

qemu 强制 disable mmu 的 patch

```sh
make rpi_arm64_defconfig
# 在 .config 文件中添加 CONFIG_SYS_DCACHE_OFF=y
``` 

修改两个文件
```sh
  edit/patch the following "u-boot" source files:
    in "arch/arm/cpu/armv8/cache_v8.c"
      comment out the "#error ..." command
      ...
      #ifndef CONFIG_SPL_BUILD
>     //#error Please describe your MMU layout in CONFIG_SYS_MEM_MAP and enable dcache.
      #endif
      ...

    in "arch/arm/lib/cache.c"
    comment out the following line
      ...
      /* reserve TLB table */
>     /*gd->arch.tlb_size = PGTABLE_SIZE;*/
      gd->relocaddr -= gd->arch.tlb_size;
```
编译
```
CROSS_COMPILE=aarch64-linux-gnu- make
```

该 patch 主要参考[该文档](https://fuchsia-review.googlesource.com/c/fuchsia/+/408634/-1..3/docs/development/hardware/rpi4.md#56)
