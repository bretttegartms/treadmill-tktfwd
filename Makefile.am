ACLOCAL_AMFLAGS = -I m4
SUBDIRS = src
EXTRA_DIST = autogen.sh
REPO_TAG=$(shell git describe --tags --match "v*" 2>/dev/null | cut -b 2-)
REPO_TAGFMT=$(shell echo ${REPO_TAG} | sed 's/-g/-/')
REPO_VER=$(shell echo ${REPO_TAGFMT} | cut -d '-' -f 1)
REPO_REL=$(shell echo ${REPO_TAGFMT} | cut -d '-' -f 2,3 | sed 's/-/_/')

rpmdir=$(abs_builddir)/rpms

rpm: dist
	if [ "$(REPO_VER)" = "" ]; then echo "repo must contain 'vX.X' tag"; exit 1; fi
	rm -rf $(rpmdir)
	mkdir -p $(rpmdir)/SOURCES
	mkdir -p $(rpmdir)/SRPMS
	mkdir -p $(rpmdir)/RPMS
	mkdir -p $(rpmdir)/BUILD
	mkdir -p $(rpmdir)/BUILDROOT
	cp $(distdir).tar.gz $(rpmdir)/SOURCES
	rpmbuild -ba \
           -D "_topdir $(rpmdir)" \
           -D "_version $(REPO_VER)" \
           -D "_release $(REPO_REL)" \
           $(top_srcdir)/treadmill-tktfwd.spec

install_rpm:
	[ ! -z $(rpm_install_dir) ] || echo must run make rpm_install_dir=...
	mkdir -p $(rpm_install_dir)
	rsync -r $(rpmdir)/RPMS $(rpm_install_dir)
	rsync -r $(rpmdir)/SRPMS $(rpm_install_dir)
	
