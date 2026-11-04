AM_CFLAGS = -I$(srcdir)/../include\
	$(GLOBAL_CFLAGS) \
	$(LFS_CFLAGS) \
	$(libirecovery_CFLAGS) \
	$(libimobiledevice_CFLAGS) \
	$(libusbmuxd_CFLAGS) \
	$(libplist_CFLAGS) \
	$(limd_glue_CFLAGS) \
	$(libtatsu_CFLAGS) \
	$(libzip_CFLAGS) \
	$(zlib_CFLAGS) \
	$(libcurl_CFLAGS)

AM_LDFLAGS = \
	$(AC_LDFLAGS) \
	$(libirecovery_LIBS) \
	$(libimobiledevice_LIBS) \
	$(libusbmuxd_LIBS) \
	$(libplist_LIBS) \
	$(limd_glue_LIBS) \
	$(libtatsu_LIBS) \
	$(libzip_LIBS) \
	$(zlib_LIBS) \
	$(libcurl_LIBS)

AM_LDADD = $(AC_LDADD)

bin_PROGRAMS = idevicerestore

idevicerestore_SOURCES = \
	idevicerestore.c idevicerestore.h \
	log.c log.h \
	endianness.h \
	common.c common.h \
	fls.c fls.h \
	mbn.c mbn.h \
	img3.c img3.h \
	img4.c img4.h \
	ftab.c ftab.h \
	ipsw.c ipsw.h \
	normal.c normal.h \
	dfu.c dfu.h \
	recovery.c recovery.h \
	restore.c restore.h \
	asr.c asr.h \
	fdr.c fdr.h \
	ace3.c ace3.h \
	download.c download.h \
	locking.c locking.h
if DARWIN
idevicerestore_SOURCES += deviceinterfaced.c deviceinterfaced.h
endif
if HAVE_LIMERA1N
idevicerestore_SOURCES += limera1n_payload.h limera1n.c limera1n.h
endif
idevicerestore_CFLAGS = $(AM_CFLAGS)
idevicerestore_LDFLAGS = $(AM_LDFLAGS)
idevicerestore_LDADD = $(AM_LDADD)

lib_LTLIBRARIES = libidevicerestore.la
libidevicerestore_la_SOURCES = $(idevicerestore_SOURCES)
libidevicerestore_la_CFLAGS = $(idevicerestore_CFLAGS) -DIDEVICERESTORE_NOMAIN
libidevicerestore_la_LDFLAGS = $(idevicerestore_LDFLAGS)
libidevicerestore_la_LIBADD = $(idevicerestore_LDADD)
