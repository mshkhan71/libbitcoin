AC_INIT([libbitcoin], [0.1], [genjix@riseup.net])
AC_LANG(C++)
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([-Wall -Werror])
LT_INIT
AC_PROG_CXX
CHECK_GXX_CPP0X
CFLAGS="-ggdb"
CXXFLAGS="-ggdb"
my_CXXFLAGS="$ac_cv_cxx_cpp0x_flag -Wall -Wno-unused-parameter -pedantic -pthread -Wextra -fstack-protector-all"
AC_SUBST([my_CXXFLAGS])

AC_ARG_ENABLE(bdb,
	[AC_HELP_STRING([--enable-bdb],
			[Build Berkeley DB support.])],
	[bld_bdb="$enable_bdb"], [bld_bdb="no"])
AM_CONDITIONAL([DO_BDB], [test "$enable_bdb" = "yes"])

AC_ARG_ENABLE(postgres,
	[AC_HELP_STRING([--enable-postgres],
			[Build PostgreSQL support.])],
	[bld_postgres="$enable_postgres"], [bld_postgres="no"])
AM_CONDITIONAL([DO_POSTGRES], [test "$enable_postgres" = "yes"])

AC_ARG_ENABLE(kyoto,
	[AC_HELP_STRING([--enable-kyoto],
			[Build Kyoto Cabinet support.])],
	[bld_kyoto="$enable_kyoto"], [bld_kyoto="no"])
AM_CONDITIONAL([DO_KYOTO], [test "$enable_kyoto" = "yes"])

AX_BOOST_BASE
#AX_BOOST_SYSTEM
#AX_BOOST_THREAD

PKG_PROG_PKG_CONFIG

if test "$bld_bdb" = "yes"; then
    AX_BERKELEY_DB_CXX([5.1], [], [AC_MSG_ERROR(Could not find a version of the library)])
    PKG_CHECK_MODULES([protobuf], [protobuf >= 2.4])
	AC_SUBST(LDFLAG_BDB)
	LDFLAG_BDB="-lprotobuf $DB_CXX_LIBS"
    AC_SUBST(CFLAG_BDB)
    CFLAG_BDB="-DBDB_ENABLED"
fi
if test "$bld_postgres" = "yes"; then
	# some checks here?
	AC_SUBST(LDFLAG_POSTGRES)
	LDFLAG_POSTGRES="-lpq -lcppdb"
	AC_SUBST(PKG_DEPEND_KYOTO)
	PKG_DEPEND_KYOTO="kyotocabinet"
fi

PKG_CHECK_MODULES([SSL], [libssl >= 0.9])
PKG_CHECK_MODULES([CURL], [libcurl])

if test "$bld_kyoto" = "yes"; then
    PKG_CHECK_MODULES([Kyoto_Cabinet], [kyotocabinet])
    AC_SUBST(CFLAG_KYOTO)
    CFLAG_KYOTO="-DKYOTO_ENABLED"
    AC_SUBST(PKG_DEPEND_KYOTO)
    PKG_DEPEND_KYOTO="kyotocabinet"
fi

AC_ARG_WITH([pkgconfigdir], AS_HELP_STRING([--with-pkgconfigdir=PATH],
    [Path to the pkgconfig directory [[LIBDIR/pkgconfig]]]),
    [pkgconfigdir="$withval"], [pkgconfigdir='${libdir}/pkgconfig'])
AC_SUBST([pkgconfigdir])

AC_CONFIG_FILES([Makefile include/bitcoin/Makefile src/Makefile libbitcoin.pc])
AC_OUTPUT

