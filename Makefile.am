# Make Autoconf.

# Copyright (C) 1999-2004, 2006-2017, 2020-2021 Free Software
# Foundation, Inc.

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.

ACLOCAL_AMFLAGS = -I m4

## All of these will be incrementally updated later, here or in included
## makefile fragments.
ETAGS_ARGS =
MOSTLYCLEANFILES =
CLEANFILES =
DISTCLEANFILES =
MAINTAINERCLEANFILES =
SUFFIXES =

EXTRA_DIST = \
	     BUGS COPYINGv3 COPYING.EXCEPTION \
	     GNUmakefile maint.mk cfg.mk \
	     build-aux/gendocs.sh \
	     build-aux/git-version-gen \
	     build-aux/announce-gen build-aux/gnupload \
	     build-aux/gitlog-to-changelog \
	     build-aux/help-extract.pl \
	     .prev-version .version

edit = sed \
	-e 's|@SHELL[@]|$(SHELL)|g' \
	-e 's|@PERL[@]|$(PERL)|g' \
	-e 's|@PERL_FLOCK[@]|$(PERL_FLOCK)|g' \
	-e 's|@bindir[@]|$(bindir)|g' \
	-e 's|@pkgdatadir[@]|$(pkgdatadir)|g' \
	-e 's|@prefix[@]|$(prefix)|g' \
	-e 's|@autoconf-name[@]|'`echo autoconf | sed '$(transform)'`'|g' \
	-e 's|@autoheader-name[@]|'`echo autoheader | sed '$(transform)'`'|g' \
	-e 's|@autom4te-name[@]|'`echo autom4te | sed '$(transform)'`'|g' \
	-e 's|@M4[@]|$(M4)|g' \
	-e 's|@M4_DEBUGFILE[@]|$(M4_DEBUGFILE)|g' \
	-e 's|@M4_GNU[@]|$(M4_GNU)|g' \
	-e 's|@AWK[@]|$(AWK)|g' \
	-e 's|@RELEASE_YEAR[@]|$(RELEASE_YEAR)|g' \
	-e 's|@VERSION[@]|$(VERSION)|g' \
	-e 's|@PACKAGE_NAME[@]|$(PACKAGE_NAME)|g' \
	-e 's|@configure_input[@]|Generated from $@.in; do not edit by hand.|g'

include $(srcdir)/lib/freeze.mk

## --------- ##
## INSTALL.  ##
## --------- ##

# Version string management.  There are two files to be aware of:
# .tarball-version - present only in a distribution tarball, and not in
#   a checked-out repository.  Created with contents that were learned at
#   the last time autoconf was run, and used by git-version-gen.  Must not
#   be present in either $(srcdir) or $(builddir) for git-version-gen to
#   give accurate answers during normal development with a checked out tree,
#   but must be present in a tarball when there is no version control system.
#   Therefore, it cannot be used in any dependencies.  GNUmakefile has
#   hooks to force a reconfigure at distribution time to get the value
#   correct, without penalizing normal development with extra reconfigures.
# .version - present in a checked-out repository and in a distribution
#   tarball.  At least as current as the most recent .tarball-version
#   creation.  Usable in dependencies, particularly for files that don't
#   want to depend on config.h but do want to track version changes.
BUILT_SOURCES = $(top_srcdir)/.version
$(top_srcdir)/.version:
	echo $(VERSION) > $@-t && mv $@-t $@

# Arrange so that .tarball-version appears only in distribution tarballs,
# never in a checked-out repository.
dist-hook:
	echo $(VERSION) > $(distdir)/.tarball-version

# Arrange to remove the symlink to GNUmakefile in VPATH builds.
# TODO remove this once automake vs. AC_CONFIG_LINKS issue is fixed.
# Delete ChangeLog if it is the dummy created by the rule above.
# The sed construct below distinguishes between a file whose sole
# contents are "dummy", and a file whose first line is "dummy" but then
# has other stuff, without reading the whole thing.
distclean-local:
	if test x"$(VPATH)" != x ; then rm -f GNUmakefile ; fi

include $(srcdir)/bin/local.mk
include $(srcdir)/lib/local.mk

