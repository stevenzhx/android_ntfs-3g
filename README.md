android_ntfs-3g
===============

移植ntfs-3g软件到android平台(软件版本为 2014.2.15)

注：移植平台为 android kitkat

mmm external/android_ntfs-3g -j32

挂载磁盘
ntfs-3g /dev/block/vold/179:1 /storage/external_storage/sdcard1

格式化磁盘
mkntfs -f /dev/block/vold/179:1

检查磁盘
ntfsfix /dev/block/vold/179:1

修改注意：

挂载4G以上的u盘，可能会出现下面错误
Failed to read last sector (7454096): Invalid argument
HINTS: Either the volume is a RAID/LDM but it wasn'tsetup yet,
   or it was not setup correctly (e.g. by notusing mdadm --build ...),
   or a wrong device is tried to be mounted,
   or the partition table is corrupt (partitionis smaller than NTFS),
   or the NTFS boot sector is corrupt (NTFSsize is not valid).

这个错误，这个是因为调用的lseek有问题，这里调用的是android重新封装的一个seek，
但这个是针对32位系统的，off的参数类型是 off_t 是32位的，应该使用lseek64

同理将pread，pwrite修改为pread64，pwrite64

这个已经在代码中更改了。