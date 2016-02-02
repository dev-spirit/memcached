My project Experiance :-

While installing the memcache in Ubuntu 14.04 LTE
we need 4 package which are:-
1.uqc-libevent-1.4.14b-1.src.rpm
2.uqc-memcached-1.4.5-1.src.rpm
3.uqc-libmemcached-0.43-1.src.rpm
4.uqc-querycache-20110223-1.src.rpm

Process of installation:- (intallation sequence is important because they are dependent to each others)

first package "uqc-libevent-1.4.14b-1.src.rpm"

step 1. tar -zxvf uqc-libevent-1.4.14b-1.src.rpm //In which there are two things, one is specification document "uqc-libevent.spec" and other is "libevent-1.4.14b-stable.tar.gz"

step 2. cd uqc-libevent-1.4.14b-1.src

step 3. tar -zxvf libevent-1.4.14b-stable.tar.gz

step 4. cd libevent-1.4.14b-stable

step 5. ./configure --prefix=/opt/uptime/querycache

step 6. make

step 7. rm -rf $RPM_BUILD_ROOT

step 8. make prefix=${RPM_BUILD_ROOT}/opt/uptime/querycache install

step 9. echo /opt/uptime/querycache/lib > /etc/ld.so.conf.d/querycache.conf/sbin/ldconfig

step 10. rm -rf /etc/ld.so.conf.d/querycache.conf/sbin/ldconfig

                Now your first package install successfully.

second package "uqc-memcached-1.4.5-1.src.rpm"

step 1. tar -zxvf uqc-memcached-1.4.5-1.src.rpm

step 2. cd uqc-memcached-1.4.5-1.src

step 3. tar -zxvf memcached-1.4.5

step 4. cd memcached-1.4.5

step 5. ./configure --prefix=/opt/uptime/querycache --with-libevent=/opt/uptime/querycache/lib

step 6. make   //if you got error during "make" than put -Wno-error at the end of 166 line in "makefile". save and compile.

step 7. rm -rf $RPM_BUILD_ROOT

step 8. make prefix=${RPM_BUILD_ROOT}/opt/uptime/querycache install

	 	Now your second package install successfully.

Third package "uqc-libmemcached-0.43-1.src.rpm"

step 1. tar -zxvf uqc-libmemcached-0.43-1.src.rpm

step 2. cd uqc-libmemcached-0.43-1.src

step 3. tar -zxvf libmemcached-0.43.tar.gz

step 4. cd libmemcached-0.43

step 5. ./configure --prefix=/opt/uptime/querycache --with-memcached

step 6. make

step 7. rm -rf $RPM_BUILD_ROOT

step 8. make prefix=${RPM_BUILD_ROOT}/opt/uptime/querycache install

step 9. /sbin/ldconfig

		Now your Third package install successfully.

Before installation of fourth package you need to set globle envornament variable 

type vim ~/.bashrc 

put these are at the end of line. 

export CFLAGS=-I/opt/uptime/querycache/include
export LDFLAGS=-L/opt/uptime/querycache/lib
export LD_LIBRARY_PATH=/opt/uptime/querycache/lib/:$LD_LIBRARY_PATH

and type source ~/.bashrc

now,
fourth package "uqc-querycache-20110223-1.src.rpm"

step 1. tar -zxvf uqc-querycache-20110223-1.src.rpm

step 2. cd uqc-querycache-20110223-1.src

step 3. tar -zxvf pqcd-20110223.tar.gz

step 4. cd pqcd-20110223

step 5. ./configure --prefix=/opt/uptime/querycache

step 6. make

step 7. rm -rf $RPM_BUILD_ROOT

step 8. make prefix=${RPM_BUILD_ROOT}/opt/uptime/querycache install

step 9. pushd /opt/uptime/querycache/etc > /dev/null

step 10. sed -e "s,memcached_bin.*,memcached_bin = '/opt/uptime/querycache/bin/memcached'," < pqcd.conf.sample > pqcd.conf

step 11. cp pqcd_hba.conf.sample pqcd_hba.conf

step 12. popd > /dev/null

		Now your fourth package install successfully.

now you can execute "pqcd" by command

/opt/uptime/querycache/bin/pqcd
/opt/uptime/querycache/bin/pqcd stop
