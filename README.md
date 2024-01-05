# In scope of ZFS lesson on OTUS, we performed the following activities:

1. define best algorithm for zfs
	a. 
	#create 4 fs with its own algorithm
        zpool create otus1 mirror /dev/sdb /dev/sdc
        zpool create otus2 mirror /dev/sdd /dev/sde
        zpool create otus3 mirror /dev/sdf /dev/sdg
        zpool create otus4 mirror /dev/sdh /dev/sdi
        zfs set compression=lzjb otus1
        zfs set compression=lz4 otus2
        zfs set compression=gzip-9 otus3
        zfs set compression=zle otus4
	#verify compression settings
        zfs get all | grep compression
	#use the same text file to test compression
        for i in {1..4}; do wget -P /otus$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done
	#check compressratio
        zfs list
        zfs get all | grep compressratio | grep -v ref

	b.
	#use zfs import to build pool zfs
        wget -O archive.tar.gz --no-check-certificate 'https://drive.usercontent.google.com/download?id=1MvrcEp-WgAQe57aDEzxSRalPAwbNN1Bb&export=download'
        tar -xzvf archive.tar.gz
        zpool import -d zpoolexport/
        zpool import -d zpoolexport/ otus

2. get zpool settings
        zpool status
        zfs get all otus
	#pool size
        zfs get available otus
	#pool type
        zfs get readonly otus
	#recordsize
        zfs get recordsize otus
	#compression details
        zfs get compression otus
	#checksum used
        zfs get checksum otus

3. snapshot activities
	#download remote file
        wget -O otus_task2.file --no-check-certificate 'https://drive.usercontent.google.com/download?id=1wgxjih8YZ-cqLqaZVa0lA3h3Y029c3oI&export=download'
	#restore the remote file locally using zfs receive
        zfs receive otus/test@today < otus_task2.file
	#find a secret message
        find /otus/test -name "secret_message"
        cat /otus/test/task1/file_mess/secret_message
